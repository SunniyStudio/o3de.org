---
title: "[SMC]-development-goals-evolution"
description: ""
toc: false
---

# Remarks on near term goal paragraphs
## October 2023 status
> I.  Users can auto-populate the shader variant list for a material shader by scanning all materials in the project.
>    1.  Move this process back to a python script, it's currently implemented in C++. (Note that the asset scan should use the asset system's product dependencies to find all downstream materials; this will allow it to pick up indirect references like .fbx files that produce .material files. This should already be the case, this is the way it was done in the original prototype, but it might have changed during the move to C++).

- [x] This has been done.
>    2.  Make sure this process is performant. At one time, the process of scanning and loading the materials also ended up loading the textures which wasted a lot of time.

- [ ] It is indeed slow (not excruciatingly), how can it be made faster remains to be studied.
>    3.  Do not automatically do this process when a .shader file is opened. Instead, prompt the user with a list of available Shader Actions:
>        1.  If a corresponding .shadervariantlist already exists, they could opt to open that file.
>        2.  The user may opt to run one of the available python scripts.

- [ ] Sounds good, that can be done.
>    4.  The same Shader Actions could also show up in the asset browser's context menu.

- [x] Now they do.
> II.  Users can duplicate and customize their own script for doing the above material scan.
>    1.  We'll need some mechanism for registering new scripts so they show up in the Shader Actions list. (For inspiration, look at Material Editor's approach to finding model presets and lighting presets).

- [x] The current mechanism is to edit the `Gems/Atom/Tools/ShaderManagementConsole/Registry/scripts.shadermanagementconsole.setreg` file.
- [ ] Is it enough? For example, the arguments to the script are not configurable, a way to specify i.e. @documentid@ or @sourcefilepath@ would be highly desirable. (c.f. [ShaderManagementConsoleTableView.cpp#L98](https://github.com/o3de/o3de/blob/fc7f51a5748612eb069ec4623908311e2af99915/Gems/Atom/Tools/ShaderManagementConsole/Code/Source/Window/ShaderManagementConsoleTableView.cpp#L98) -> hardcoded argument passing from C++ -> must evolve to data driven variables)
> III.  Users can auto-populate a shader variant list by fully enumerating a shader's options.
>    1.  Make another python script that, given a shader, will generate a shader variant for every possible combination of options. (This should be pretty simple since we will already have the script from above for doing a material scan, which has the same inputs and outputs.)

- [x] This is now done in `ExpandOptionsFullCombinatorials.py`
>    2.  The script should warn the user if the permutation count would be really high.

- [ ] Right now it doesn't, starting 500 over it is impossibly slow to transfer to Qt.
> IV.  Users can manually add, remove, and edit variants. Basic editing is supported but some improvements are needed in this area.
>    1.  Fix the row numbers to display StableID value instead of index values.

- [x] Now implemented.
>    2.  Transpose the table, so that Option names are on the left and StableIDs along the top. This will make better use of the space.

- [x] Attempted, but judged that it was too strange, kept as current.
>    3.  Undo/redo is slow (tested with StandardPbr\_ForwardPass.shader).

- [ ] To investigate. There are some remarks about not using a full data model copy, but it could be a Qt bottleneck.
>    4.  Add and remove rows.

- [x] Now implemented.
>    5.  We need to handle duplicate rows in some way, highlight them, have an option to remove the duplicate.

- [x] Now implemented.
> V.  Users can save the .shadervariantlist to either the original .shader location or to their game project folder (in the predetermined location described in Shader Variant List File Details above).
    1.  Change Save As to only give the user the two valid options.

- [x] Implemented
- [ ] but save location is still free if using some "create from" options in the asset browser.
>    2.  Remove Save As Child, it doesn't make sense for variant lists.

- [ ] To investigate.
> VI.  We need to improve the build pipeline to minimize rebuilds. Currently if you change the shader option value for one shader variant, the AP will recompile every shader variant in the list.

- [x] Now implemented.
- [ ] To verify that the recompacting system isn't creating false-negatives on rebuild judgements.
> VII.  Users can inspect which shader variant is being used in the runtime. This isn't actually part of the Shader Management Console but is worth mentioning here. This is needed for debugging the shader variant system and the user's shader variant list configuration.  
    1.  The tool can be used in O3DE Editor, Material Editor, and Material Canvas.  
    2.  Show the list of all shaders in the material, and indicate which ones are active.  
    3.  For each active shader, show the shader option values that were requested, and which shader options are baked in the active variant.  
    4.  (Note we already have something like this in AtomSampleViewer, but it is implemented in ImGui and we'll need it to use O3DE's QT system)  
        ![image](https://user-images.githubusercontent.com/55155825/180096076-7cbc1dd2-7757-42b6-a84e-a7c8fae25f82.png)

- [ ] The ImGui method seems fair to me though, this system might deserve an adaptation as a Gem or callback tool that can be enabled by games.
