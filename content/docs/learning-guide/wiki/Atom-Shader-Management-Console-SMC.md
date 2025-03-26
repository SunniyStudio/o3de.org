---
title: "[Atom]-Shader-Management-Console-(SMC)"
description: ""
toc: false
---

Open 3D Engine (O3DE) includes a tool called Shader Management Console (SMC) that aims to help the user configure and manage a game project's set of shader variants, to find an optimal balance between runtime performance and developer iteration time. This tool and its design is a work in progress. In this document, we'll try to capture the background for SMC, some history, design ideas, and plans for moving forward. A way for the community to learn what's been done so far and work together to steer toward a solution for the challenging problem of shader variant management.

# Overview

One challenge that every large-scale game (and game engine) must address is how to manage shader variants or shader permutations. In order for a renderer to run at peak performance, compiled shaders usually need to be reduced to a minimal set of instructions and resources that are required for each use case. This can result in thousands of unique compiled shader programs, taking hours to compile, adding significant friction to developer workflows. For a thorough discussion of this problem, see [The Shader Permutation Problem - Part 1](https://therealmjp.github.io/posts/shader-permutations-part1/) and [Part 2](https://therealmjp.github.io/posts/shader-permutations-part2/).

One common approach to generating these many compiled shader programs is to use preprocessor flags to disable sections of code at compile time. In this case you either need to compile every possible combination of these flags, or if there are too many flags you need to know ahead of time which permutations will actually be needed at runtime. If there is some mistake and a permutation is missing at runtime, the application will give incorrect results or maybe even crash. The Atom renderer's shader variant system addresses this with a feature called _shader options_. The shader author can use an option like any other variable to control the flow of code, like using regular _if_ or _switch_ statements. If a value for the shader option is provided at compile time, then the options are replaced by constants and the resulting dead code is eliminated during compilation. Otherwise, the value is provided at runtime through a constant buffer. The game project will still have a collection of multiple shader variants, each with a different combinations of shader option values (or some shader options unspecified at compile time). Each shader will always have a _root_ variant that receives all shader option values at runtime. Therefore the list of shader variants does not have to be exhaustive. If the runtime requests a variant with a combination of shader options that does not exist, a fallback will be used. Because the same values are available from a different source, the final visual result should be the same (but with reduced performance).

Note that simple preprocessor flags are still supported and can be used through a feature called shader supervariants, but that topic is outside the scope of Shader Management Console (at least in its currently imagined form).

The shader variant system provides a lot of flexibility because of the fallback mechanism. A game could fully enumerate every possible shader permutation, or it could solely rely on root variants, or anything in between. In any case, the system needs some way to define the set of shader variants to be compiled for any given shader file. This is done through a .shadervariantlist file, which lists every shader variant that should be compiled, and which shader option values are predefined for each variant. The Shader Management Console is used to author these shader variant list files.

We took this approach for a few reasons:

1.  We assume that in many cases, there will need to be a human in the loop to analyze and massage the set of shader variants that are produced. It's just too complicated of a problem to assume that some automated system will be able to generate an appropriate list of variants. So there must be a data file to capture the user's intent.
2.  The set of shader variants is project-specific, while the shader itself may be located in a common Gem. For example, one project might need 100 variants of StandardPbr's forward pass shader, while another project needs 500 variants.
3.  The shader build pipeline is part of O3DE's Asset Processor (AP), and this carries some design constraints. The AP cannot write files to the source folders, only other tools (like SMC) are allowed to do that. The AP is not designed to scan a bunch of files and correlate inter-dependencies between them (like materials depend on shaders and variants but shaders depend on materials to know which variants are needed), rather there needs to be one main file. (And if we were to make a placeholder main file that just depends on all .material files, then every time you touch a material it would trigger the full shader variant scan).
4.  Using a direct list of variants and options is the simplest approach we could take for a first pass at shader management.

We envision several ways that users may want generate a list of shader variants for a shader. None of these approaches can satisfy every use case, and projects are likely to need more than one of these approaches used together. SMC will be the hub through which all of these processes are run. Although material shaders are the most prominent type of shaders that need to be managed, we need a way to manage variants for other kinds of shaders as well, such as full-screen shaders or compute shaders.

## Shader Management Use Cases

It's important to recognize that the shader management strategy may vary greatly depending on the design of the render pipeline, especially whether the pipeline takes a _deferred_ approach or _forward+_ approach. Permutation management in a forward+ pipeline is more of a challenge because the material code and lighting code coexist in the same shader, so each material's permutation space is much larger. In a deferred pipeline, where material shaders and lighting shaders are separate, all the permutation spaces are smaller and it may be possible to just fully enumerate all possible shader variants. One of the core tenets of the Atom renderer is to be customizable, data driven, and flexible enough to support any kind of render pipeline you design (also a work in progress, see [https://github.com/o3de/sig-graphics-audio/blob/main/rfcs/rfc-prs-20210913-1.md](https://github.com/o3de/sig-graphics-audio/blob/main/rfcs/rfc-prs-20210913-1.md)), so it's important to consider a wide range of possible shader management use cases.

1.  For any given shader, manually define a list of each shader variant and its shader option values. (This is especially useful for small numbers of shader options, or for testing and debugging the variant system).
2.  For any given shader, enumerate the full set of all possible shader option permutations. (Again, this is useful for small numbers of shader options).
3.  Scan a project to find every material and inspect its property values to determine the combination of shader options it requires. Generate a list of all shader variants used by all materials. (Note that this does not account for system-level shader options that could be set at runtime, like adjusting performance levels).
4.  Write a script to customize the material-scan above, to skip folders, filter out some options, fully enumerate some options, etc. (This could address those system-level shader options also mentioned above).
5.  Collect metrics at runtime about what shader variants are requested and how often, and then SMC can use that data to tune the shader variant list, either adding missing variants or pruning less-used variants.
6.  For any given shader, use a dependency graph to define the relationship between shader options, so the tools can skip impossible or unnecessary permutations.

# Shader Variant List File Details

Here is an example .shadervariantlist file. After referencing the relevant .shader file, it simply lists the prescribed shader variants. Each variant has a list of shader option values that are defined at compile time, and baked into the compiled shader bytecode. Any shader options that are not assigned a value here, will be set dynamically at runtime through a constant buffer.

The StableId is a unique number assigned to each shader variant. This value is necessary for the AP to maintain consistent references to each shader variant. They do not need to be sequential, there may be gaps, and removing a shader variant should not shift the IDs of the other variants.

It should be noted that the .shadervariantlist file can only be stored in two specific locations, in order to be found at runtime. Normally an asset dependency would lead the runtime to the right file location, but in this case the .shader file doesn't have a direct reference to the .shadervariantlist file because it could appear in the user's project folder. So in order for the runtime to be able to find the shader variant list, it has to appear in a specific path. It can either be in the same folder as the .shader file, or it can be in a specific path under the user's project folder: if the shader is {SomeGem}/Assets/Ocean/Shaders/ComputeWaves.shader then the shader variant list must be in {MyProject}/ShaderVariants/Ocean/Shaders/ComputeWaves.shadervariantlist.

```
{
    "Shader": "Materials/Types/StandardPBR_ForwardPass.shader",
    "Variants": [
        {
            "StableId": 3,
            "Options": {
                "o_directional_shadow_filtering_method": "ShadowFilterMethod::None",
                "o_parallax_feature_enabled": "false"
            }
        },
        {
            "StableId": 5,
            "Options": {
                "o_directional_shadow_filtering_method": "ShadowFilterMethod::Pcf",
                "o_parallax_feature_enabled": "false"
            }
        }
    ]
}
```
  

# Original Prototype

The first version of SMC gave the bare minimum of functionality to quickly generate a best-effort .shadervariantlist file. The core functionality was centered around a python script that would scan all the materials in a project and automatically generated a .shadervariantlist based on the results. This covered use cases 3 and 4 above. It would also display the variants and options in a read-only table. This allowed users to quickly get a list that was pretty close to the runtime needs of the project, and since it was a script, the user could duplicate and customize that script if they needed something special.


<img src="https://user-images.githubusercontent.com/55155825/180095971-1fca3c2a-733f-4961-8463-afdec9588688.png" width="50%">
<br><br>
<img src="https://user-images.githubusercontent.com/55155825/180095987-c21c6443-08ba-4836-89a5-86e131589169.png" width="50%"> 
<br><br>
<img src="https://user-images.githubusercontent.com/55155825/180096004-670331e5-57ca-4d07-837e-4f548f14300d.png" width="50%">


Note there is also a preliminary shader metrics collection system for gathering usage statistics at runtime. This project was put on hold due to time constraints, and it was disabled to avoid performance impact since it was not sufficiently optimized at the time. See [https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Include/Atom/RPI.Public/Shader/Metrics/ShaderMetricsSystem.h](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Include/Atom/RPI.Public/Shader/Metrics/ShaderMetricsSystem.h)

# What's implemented now

Quite a bit of work has gone into the underlying UI systems to share a common robust framework between Shader Management Console, Material Editor, and the new Material Canvas. This ensures a consistent UI experience, integration with core O3DE systems, and a clear path forward for general tooling improvements.

The table of variants and options can be edited now, with undo/redo, but needs some improvement to be a smooth experience.

new variant lists can be created from a blank state, or using the material crawl python script that will prepopulate with option values found in the materials.

![image](https://user-images.githubusercontent.com/55155825/180096040-78e86b0b-6421-42d0-a8a4-633eb9be37ec.png)

## Further features from [PR16358 (smc evolutions)](https://github.com/o3de/o3de/pull/16358)

### filtering of assets

Filters have been revamped to let all shader related files display. Also the cache folder now accessible allows to get .shader from pipeline templates.

![image](https://github.com/o3de/o3de/assets/25970839/be2b2c5d-e070-42b9-aed1-c75aa68749c8)

### Create empty

We now have the choice to create an empty variant list asset at startup, for a .shader. Use the context menu in the asset browser as such:  
![image](https://github.com/o3de/o3de/assets/25970839/21e93ea7-55be-47cf-b5fd-f217459dbce2)

You can also prepopulate with discovered material property values using Python scripts context submenu, and call `GenerateShaderVariantListForMaterials.py`.

### Tabe view context menu

You can now right click on empty space in a document and get the opportunity to add a new variant, which will present itself as a row.

step 1  
![image](https://github.com/o3de/o3de/assets/25970839/b0d7ae90-1a90-4277-ab83-521e0e0a4426)
result  
![image](https://github.com/o3de/o3de/assets/25970839/16a8f2bc-3863-42c3-a2ec-0fcb67938509)

### Partial combinatorial enumeration

There is now a script that will expand selected options with a full list of possible values.  
Access through the context menu of the document too:  
![image](https://github.com/o3de/o3de/assets/25970839/70882e3e-6484-4725-bd47-659c5d80ca7b)

It presents itself like this:  
![image](https://github.com/o3de/o3de/assets/25970839/2eefecc9-a316-4eb8-a5b0-54d7c406ee1e)

On the left you have the available options of the shader file, and on the right, the selected options that will participate to the full enumeration. The preselected number of variants to generate as a target is 32, from that number, you can use the button "generate auto selection" and it will slide options from the left panel to the right panel to prepare them to participate in the expansion. In case of boolean options, it will select 5 options.  

As such:  
![image](https://github.com/o3de/o3de/assets/25970839/69b78f01-660a-4df6-b8fd-1b988bd1d96a)

Then click "generate variant list" to send the result to the document that originated the opening of the script.  
When you exit, the SMC will start to append the result to the current list of variants.

Result:  
![image](https://github.com/o3de/o3de/assets/25970839/ce9293f6-3d99-49ad-8532-a958f4a134c7)

### Recompaction

It could add variants that already existed and became redundant, to remediate, there is now a recompaction button.

Let's take the example of a starting variant count of 3, as such:  
![image](https://github.com/o3de/o3de/assets/25970839/d40d22b0-1375-4644-9c34-92479a454d07)

Appending an expansion will result in this:  
![image](https://github.com/o3de/o3de/assets/25970839/4f9f22fc-1fae-4d26-8f57-93d333996b9a)

You can call the recompaction, and the result will be unique variants only:  
![image](https://github.com/o3de/o3de/assets/25970839/83ee515c-3847-4efe-a0b2-eda8af879d13)

Note that the first line with all dynamic options is already present in the base shader asset, it is the root. This might be part of this recompaction button to make sure that variant is not present after recompaction.

### display ordering:

Using the combo box on top of the document you can also chose a display mode for the columns, the default is the cost impact analysis reflected by azslc.  
![image](https://github.com/o3de/o3de/assets/25970839/23a820e8-aa90-4ec8-8e20-385596807ffc)

# Near-term goals

[Dedicated page on historical evolutions and comments toward goals](https://github.com/o3de/o3de/wiki/%5BSMC%5D-development-goals-evolution)

## "left to do" taken from the page

- [ ] 3.  Do not automatically do this process when a .shader file is opened. Instead, prompt the user with a list of available Shader Actions:  
         1.  If a corresponding .shadervariantlist already exists, they could opt to open that file.  
         2.  The user may opt to run one of the available python scripts.
- [ ] arguments to scripts are not configurable, a way to specify i.e. @documentid@
- [ ] IV.3.  Undo/redo is slow
- [ ] save location is still free if using some "create from"
- [ ] V.2. Remove Save As Child
- [ ] verify that the recompacting system isn't creating false-negatives to the rebuild-economy system
- [ ] VII.  Users can inspect which shader variant is being used in the runtime.

# Variant pickup strategy

One aspect of the shader variant fallback mechanism is that the shader variants are logically organized into a tree. At runtime, if a requested shader variant doesn't exist, this tree is used to fall back to a close match that has as many high-priority shader options pre-baked as possible, rather than immediately going to the root variant.

## It's a DAG

In reality, it's more a directed acyclic graph because of the diamond relations that are possible between leaf variants and the root variant. Multiple choices are possible and the selection algorithm will walk them.

![image](https://github.com/o3de/o3de/assets/25970839/ff3093dd-f21b-4e45-a4c0-9aca8455df8e)

That's an example of a variant tree, with 2 options. greyed options are set on "runtime" (unspecified value).  
Arrows represent fallbacks, and blocks represent unique bytecode.  
Not every variant exist, only the one saved as assets. Materialized by lines in the SMC table view.  
For the sake of the mathematics of selection, inexistent variants can be considered as virtual, but the walk system might start there, and be forced to pick the next candidate, repeat if don't exist, until the root.

In this image, there is a suggestion of cost per node, but it is based on the option value so it is not robust and must not be considered for implementation. The idea presented above is that unspecified options have half their associated analyzed cost. OFF options have 0 and ON options have full cost. But since shader code may use complex logics and integer values, ON and OFF is not identifiable. The current method for picking is based on rank and number of specified (hard) options.

### Example

![image](https://github.com/o3de/o3de/assets/25970839/e5f056bf-7561-4648-912a-c7627eaf60b2)
If you query for "fog on" there are 2 candidates that are valid in any case. 3 candidates that will depend on final option value decisions before rendering. If that decision is never made, the `shadow` option MUST be unset so only the hard candidates can be used. Among those 2, the one with the most hard options is selected. Here: the leftmost variant.

***
Same with off:  
![image](https://github.com/o3de/o3de/assets/25970839/21612f36-c271-40c4-8152-26132a2a23c4)

***
Full set of options specified in the query:  
![image](https://github.com/o3de/o3de/assets/25970839/e0022c90-b392-4d61-8b9f-93b24d5dabc4)
In that case, the lowermost variant is selected but it isn't without actually walking the DAG before.
***
In the case that a virtual variant is implicated:  
![image](https://github.com/o3de/o3de/assets/25970839/19b1d808-21c0-4abe-a385-e51645cf6fb5)
In this case, we have an ambiguity, two candidates have equal estimated "cost" based on number of hard options.  
The current system strives at prioritizing based on secondary criterion: rank. (rank can be based on declaration order or static cost impact analysis).

This mechanism is in the `ShaderVariantTreeAsset::FindVariantStableId` method.

# Long-term goals and ideas

## SMC GUI proposition

We could organize the shader variant list display into a tree that corresponds to this fallback tree. Since the tree is organized according to the relative priority of each shader option, this could give a helpful way of visualizing the data. We could then give users a way to prune entire subtrees of low-priority shader options. Imagine they right-click on a node, select Always-Prune, and now that subtree will never show up again in the list, even if some automated process tries to add it again.

Make the SMC and variant list file record the user's manual edits, so they can permanently exclude/include certain variants and preserve this setting even if they re-run an auto-populate script.

There is a lot we could explore in the area of collecting shader metrics at runtime. As mentioned above, there is already some beginnings of a metrics system that collects data about which variants are being requested. A lot more thought needs to be put into how we should collect this data, where to store it (so the whole game team can contribute to a single metrics database), how to correlate it with the shader source files, how to clear out old stale data, and how to apply it using Shader Management Console.

Jeremy has presented an idea for a dependency graph to define the relationship between shader options, so the tools can skip impossible or unnecessary permutations. This would be especially helpful for cases where the user is trying to fully-enumerate a permutation space, and would like to reduce that space to only valid combinations of shader options.

It would be great if we could come up with a more direct solution to the fact that some material shader options are changed at runtime, making the material-scan process ineffective for those options. With the above features, a game team could customize the material-scan to enumerate the relevant system-level options. Or using the shader option dependency graph you might be able to fully enumerate the permutation space. I wonder if there are other solutions to be found here.

The .shadervariantlist file doesn't necessarily need to specify just a literal flat list of shader variants. This file could turn into a generalized configuration file for shader variant generation that does some more complex things through the AP. One example would be defining a shader option dependency graph as described above. Another example: for any given option, instead of specifying the actual value there could be an "$all" value that causes the AP generate variants for every permutation of whichever options that use the _all_ setting.

It would be nice if Shader Management Console would expose statistics about each shader variant's compilation like how long it took to compile, how many dynamic branches are found (something the AP already reports), and static analysis of VGPR pressure (something we've talked about adding in general).

We need to think about what to do with large shader variant lists when a shader adds, removes, or renames a shader option. If a new shader option is added, SMC could prompt to either fill them in with the default value or unspecified value. If a shader option is removed, SMC should notify the user and remove any duplicates. Things like that. Or we could model it after the material type version update system with some update steps in the .shader file.

# Alternatives

One major caveat here is that we're working on Material Canvas. On the one hand, this only amplifies the need for shader variant management as users will have new tools to easily add lots more material shaders. On the other hand, it's possible that Material Canvas designs will impact shader and material systems dramatically, so that the above SMC designs end up needing significant revision. For example, suppose that Material Canvas ends up removing the ability for material properties to set shader options, and the only shader options that exist are at the lighting/system level.
