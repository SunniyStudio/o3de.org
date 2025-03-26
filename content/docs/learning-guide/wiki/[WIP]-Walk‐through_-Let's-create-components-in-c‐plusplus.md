---
title: "[WIP]-Walk‐through_-Let's-create-components-in-c‐plusplus"
description: ""
toc: false
---

In this document, I create a set of c++ components step by step, and integrate it with O3DE, as a tutorial for others to follow and learn.

This document is not just to give the TLDR bare minimum steps to follow without understanding anything, but instead to guide the person following along with it, with an understanding of how it actually works so that they can make choices for their game and plugins that make sense.

# End Goal
* A C++ component which is 'full featured' enough to give someone the basics of common interaction with the engine and tools
* Somewhat useful component if possible, not a hello world
* Exposed to scripting, ebus, etc.
* Tools and Game component, so that it does something in the editor when not in "Run in editor" CTRL+G mode.

## Background Info: Gems, Components, Modules

You can skip this section if you understand how O3DE Gems relate to O3DE projects, and how the CMakeLists.txt files work in a Gem.  Just create a project to follow along with, and skip straight to `Step 2`.

Components are the primary way to add new functionality to O3DE.  It doesn't matter if what you're making is a new always-on system (Such as an audio backend or service that is globally available) or a single piece of functionality you want to put on a single entity somewhere in a single level, that functionality ends up being in a component, or at least the life cycle of the system is managed by a component.

The O3DE documentation does go over this here: https://www.docs.o3de.org/docs/user-guide/programming/gems/overview/ but I will try to simplify and summarize:

* O3DE's calls its plugins "Gems".  
* A Gem is defined literally as any folder containing a valid `gem.json` and `CMakeLists.txt` at its root.
  * The `gem.json` file gives it simple properties like its `gem_name`, simple description, author, license, etc.
  * The `CMakeLists.txt` file tells the build system how to use it.  Most of the time, this file recurses into additional `CMakeLists.txt` in subfolders.  It does not have to contain build system instructions to build binaries.  
     * For example, a Gem that only contains art assets and no C++ code might have a very simple `CMakeLists.txt` that just notifies the Asset system that the assets in the gem should be made available to the asset system, and no build system instructions related to compiling C++.
     * On the other hand, a really complicated Gem's `CMakeLists.txt` might specify several static libraries, dynamic libraries (.so or .dll), and even standalone executables, and also how and when the O3DE ecosystem of applications should load them, and which ones should be loaded where.  The architecture of the internals of the Gem and what it contains is up to the author of the Gem.

* To make a project in O3DE you must create a game project using the Project Manager application, or its CLI.
* To O3DE, a game project is any folder containing a valid `project.json` and a `CMakeLists.txt` at its root.
  * The `project.json` file gives it simple properties like its `name`, simple description, author, license, etc.
  * `project.json` also includes a list *by name* of which Gems should be active for this project.
  * `project.json` also includes a list *by relative path* of folders containing additional Gems that are project-specific.

* When you tell a project to build, the build system will search for any matching gems that the project wants, in all the known folders where gems are found, including any that are listed in `project.json`.  When it finds a match, it executes that Gem's `CMakeLists.txt` build script as part of building the project.
* The Gem's `CMakeLists.txt` describes which c++ files (if any) are to be compiled into which libraries (`.dll`, `.so` files), and which libraries should be loaded into which programs that are part of O3DE.  
  * For example, a 'Weather' gem might inform the build system that `Weather.dll` must load into the game runtime, but `Weather.Tools.dll` must load into the Editor and Material Editor instead.

Given the Background Information above, you can understand how this works via the below exercise which is the first step in this walk through:

## Exercise 1 - Create a new project for this walk-through, and inspect it

You can skip this the first step below if you already have an example project you have created using the Project Manager. 
1. create a project based on the Default Template.  You don't have to build it to follow the next steps, it is enough to just create it.
2. look at that project's `project.json` file.

I will call this project ExampleFlash but you don't have to.

(example below, with `...` ellipsis to skip parts we don't currently care about)

```json
{
    "project_name": "ExampleFlash",
    "version": "1.0.0",
    ...
    "external_subdirectories": [
        "Gem"
    ],
    ...
    "gem_names": [
        "ExampleFlash",
        "Atom",
        "AudioSystem",
        "CameraFramework",
        ...
    ],
    ...
}
```

3. Notice that `project.json` will have a gem in its `gem_names` list **named the same** as the project, `ExampleFlash` in the above snippet.
4. Notice that `project.json` will have a subfolder called 'Gem' listed in `external_subdirectories` - this is the extra folders for the build system to scan for gems that are project specific.  These paths are relative to the project, so `"Gem"` means its talking about the `project folder/Gem` subdirectory.
5. Notice that the project does indeed have a `Gem` subfolder.  That folder has a `gem.json` and a `CMakeLists.txt` in it, which means its a valid Gem.
6. Notice that the `gem.json` in the Gem subfolder gives the gem the same name as the project.

```json
{
    "gem_name": "ExampleFlash",
    ...
}
```

This all means that when building the project, o3de will read `project.json` and add the `Gem` subfolder of the project to the list of all the places it looks for gems.  It will then step through the list of Gems active for the project (`gem_names` property) and try to find each one by name, in all the places it looks for gems.   It will find a Gem with the matching name as expected, so it will activate the `CMakeLists.txt` in that folder as part of the build process.

Note that this gem could have been called anything.  It did not have to be named the same as the project, and it did not have to be in a folder called `Gem`.  

Projects can also contain more than one gem, or even zero gems.

7. Now let's look at the `CMakeLists.txt` for the gem for curiosity's sake.  The important lines are where it declares `targets`.  Targets are how CMake declares things like executables, `dll` or `so` files, or `lib` or `.a` static libraries.  Even if you're not familiar with CMake, this is a pattern you can follow by copy and paste.

```cmake
...
ly_add_target(
    NAME ${gem_name}.Private.Object STATIC
    NAMESPACE Gem
    FILES_CMAKE
        exampleflash_files.cmake
        ${pal_dir}/exampleflash_${PAL_PLATFORM_NAME_LOWERCASE}_files.cmake
    INCLUDE_DIRECTORIES
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PRIVATE
            AZ::AzGameFramework
            Gem::Atom_AtomBridge.Static
)
```

To ease understanding, I'm going to use the example of the Gem I called `ExampleFlash`, so `${Gem_Name}` will resolve to `ExampleFlash` 

The above section declares a `STATIC` library.  On windows this will result in `ExampleFlash.Private.Object.lib`.  On others operating systems, it may end with the `.a` extension instead of `.lib`.  In CMake, things generally avoid mentioning their operating specific file names and prefer to refer to each other by their target name.

In this case, it puts it in the `Gem` namespace so this target will be referred to elsewhere (things that depend on it) by its full name `Gem::ExampleFlash.Private.Object`, even though under the hood, on Windows, this will generate a `ExampleFlash.Private.Object.lib` file in the build folder.

The `FILES_CMAKE` section contains `exampleflash_files.cmake` which it will read to get the list of c++ files to compile into this library.
This file is just a simple list of cpp/h files to compile into that static library.

```cmake
set(FILES
    Include/ExampleFlash/ExampleFlashBus.h
    Include/ExampleFlash/ExampleFlashTypeIds.h
    Source/ExampleFlashSystemComponent.cpp
    Source/ExampleFlashSystemComponent.h
)
```

Note that it lists one cpp file to be compiled - `Source/ExampleFlashSystemComponent.cpp` - the rest of them are headers.  For the purpose of compiling things, the header files don't need to be listed here.  But since this is also used to generate Visual Studio or other projects, the header files are included so that they can show up in the project explorer of the IDE for easy access.  Only the CPP file is actually compiled.

The `INCLUDE_DIRECTORIES` section lists what include directories are to be declared for this static library while it is being compiled.  They are relative to the `CMakeLists.txt` file.  So it specifying just the string `Include` really refers to the folder `Include` in the same folder that the CMakeLists.txt file is in.

The `BUILD_DEPENDENCIES` section lists what other targets to depend on, by the target name.   So this one will depend on `AZ::AzGameFramework` which is a target declared by the engine itself, as well as `Gem::Atom_AtomBridge.Static` which is a target inside the Atom gem from the engine folder.  When you specify build dependencies to CMake, the build system figures out what needs to be done to support the build dependency, based on what that target declares itself as.  You merely need to list the dependency by name.

`INCLUDE_DIRECTORIES` and `BUILD_DEPENDENCIES` as well as some other properties that are available declare the visibility (PRIVATE, INTERFACE, or PUBLIC) of the include directory or dependency.  `PRIVATE` means that it applies only to this target being compiled.  `PUBLIC` means that it applies to this target, and also, anything else that depends on this target too (recursively applied).  `INTERFACE` means it only applies to things which use this target, but not the target itself.  In this case, its saying that the `Include` directory is `PUBLIC` and the `BUILD_DEPENDENCIES` are `PRIVATE`.  There can be a mixture.

The next target the Gem's `CMakeLists.txt` declares:

```cmake
...
ly_add_target(
    NAME ${gem_name} ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
    NAMESPACE Gem
    FILES_CMAKE
        exampleflash_shared_files.cmake
        ${pal_dir}/exampleflash_shared_${PAL_PLATFORM_NAME_LOWERCASE}_files.cmake
    INCLUDE_DIRECTORIES
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PRIVATE
            Gem::${gem_name}.Private.Object
            AZ::AzCore
)
...
```

Dissecting this, it is declaring a module (`dll`) called the same name as the gem (so in my example, it would result in `ExampleFlash.dll`).  

Instead of using the keyword `STATIC` or `SHARED`, it declares it using the special variable `${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}` which results in a shared library (so a `.dll` module) on platforms and configurations that support dynamic linking, and a static library when linking in static 'monolithic' mode or when the platform being built for does not support dynamic linking.  Most of the time, this just results in a `.dll` on windows, as the variable will resolve to `SHARED`, which to CMake, means generate a shared library, aka, a dynamic library, aka `.dll` file.

As before, it has a `FILES_CMAKE` that are file lists, containing which files are part of the compile for this particular `ExampleFlash.dll`.  In this case, its `exampleflash_shared_files.cmake` which looks like this:

```cmake

set(FILES
    Source/ExampleFlashModule.cpp
)
```
Just one file, the Module file.  All dynamic binaries loaded by any O3DE executables must contain exactly one Module, as it serves as the entry point into the system and registers everything else.

Of note, it also specifies the `Include` folder, but since it `DEPENDS` on the previous target we looked at (`Gem::${gem_name}.Private.Object`), and that target exports `Include` as a `PUBLIC` include directory, this is redundant as it will inherit that anyway.

The only other section of interest here, is the section that tells the engine what modules from the gem should be loaded into what programs.  Its this section:

```cmake
ly_create_alias(NAME ${gem_name}.Builders NAMESPACE Gem TARGETS Gem::${gem_name})
ly_create_alias(NAME ${gem_name}.Tools    NAMESPACE Gem TARGETS Gem::${gem_name})
ly_create_alias(NAME ${gem_name}.Clients  NAMESPACE Gem TARGETS Gem::${gem_name})
ly_create_alias(NAME ${gem_name}.Servers  NAMESPACE Gem TARGETS Gem::${gem_name})
ly_create_alias(NAME ${gem_name}.Unified  NAMESPACE Gem TARGETS Gem::${gem_name})
```

To explain this section, this is how this gem communicates to O3DE and tells it which targets it declared above in its `CMakeLists.txt` file should be loaded into what programs inside O3DE. 

O3DE doesn't actually care about what targets, static libraries, dlls, whatever a gem author includes in their Gem.  It only cares about which modules must be loaded into which of its application.   O3DE classifies all of its applications into one of these 5 buckets:
 * `Clients` are the game runtime, or things like the game runtime, which run the simulation standalone, not in the editor
 * `Tools` are any user-facing tool - so the Editor and Material Editor are `Tools`.
 * `Builders` are the asset compression pipeline, so AssetProcessor and its related AssetBuilder systems
 * `Servers` are standalone dedicated servers that don't have client functionality ("headless" servers)
 * `Unified` are applications which can be standalone game clients as well as servers.  For P2P games, for example.

Those 5 category are reserved names and are the only thing O3DE actually cares about.

Essentially, when a Gem with name `ExampleFlash` is active, any O3DE Tools like the Editor will load anything declared here `ExampleFlash.Tools`.  If there is no `ExampleFlash.Tools` it will not load anything into the editor for that Gem. 

So the above block is *aliasing* `ExampleFlash.Tools` to the one `ExampleFlash.dll` that it produces (It uses target name, not file name, so that its platform independent - this is why it aliases the `TARGETS` `Gem::ExampleFlash`).  It is also aliasing that same `ExampleFlash.dll` target to the `ExampleFlash.Clients` name, and `ExampleFlash.Servers` and so on meaning that game runtime clients will *also* load `ExampleFlash.dll`, and so will servers, and in fact, all the different executables.

This section is vital, as it is the section that allows the creation of Editor-specific, or Server- or Client- specific dlls containing C++ code just for those executables.  Using Aliases in this manner is also what allows you to re-use the same target (as in, the same `.dll`) for multiple different cases.  In this default project case, it only produces one module, and thus re-uses it for every application.  It is also a `TARGETS` list (so it can list multiple targets, in case the gem splits its functionality into a set of smaller DLLs and they don't all apply in all circumstances.

### End-of-exercise summary

The default gem we generate when we create a default project:
* Contains 2 targets
* One is a static library considered PRIVATE to the gem, and no other gem should ever interact with it.
* One is a public dynamic library, and that's the one that is aliased in such a will cause the editor to load it.
* It specifies that the dynamic library should be loaded into all executables in the O3DE ecosystem (including the editor).

## Exercise 2 - 'Architect' your new component
Specifying the basics what you need for your component before you begin can help plan it out.

In our case, I went with a simple component that is theoretically useful and reusable but not particularly complicated - something which has an API, and when triggered, will cause the attached entity to 'flash'.  To be more specific, it will animate a material parameter of the attached entity, when triggered by an EBUS event.

I'll plan for it to take the following parameters
* Color
* Material Parameter Name
* Duration of effect (from activation to it being faded completely)

In order to keep this simple I'm not going to offer options like pulsing/repeating/etc but it would not be hard to do that.

If that was the only effect I wanted, I would not need an editor component.  It doens't technically need to do anything in the editor to function, except offer the above properties.  It can flash in game when triggered.

But, for the purposes of this walkthrough, and for easy tweaking, I want to add a "preview it" option that makes it actually show its effect in the editor.


