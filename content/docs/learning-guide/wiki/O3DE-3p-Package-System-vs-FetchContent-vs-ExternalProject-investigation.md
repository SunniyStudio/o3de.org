---
title: "O3DE-3p-Package-System-vs-FetchContent-vs-ExternalProject-investigation"
description: ""
toc: false
---

(Author, Nick L)

Over the last month or so, I've been using Address Sanitizer to check for various issues in the O3DE codebase.  I've managed to fix each one I've found in at least all of the tests.  The tests in O3DE provide pretty good coverage for the core, less so for the gems.

While doing so, I found very few "False positives" - just about every single time it notified of something, it was a legitimate problem.  Most of the problems were actually in the tests themselves, the test doing something like submitting an object to the engine, the engine taking ownership of the object and deleting, then the test doing something else with that deleted object.  Some of them, however, were legitimate engine issues and have since been fixed.

Before going futher, here's a really brief rundown of the three options for 3p

## Open 3D Engine's 3p system

These classic 3p packages are created by modifying a build script in https://github.com/o3de/3p-package-source and then triggering a build using build automation (github actions). 

The build scripts are pull-requestable, and each package consist of a script that, for each supported platform:
* Fetches the original source code from the original author's site (so clones their github, or whatever)
* OPTIONALLY, patches it in some way to make it work better with O3DE.
* Executes a set of build commands, usually just a cmake configure + build - usually twice - building in debug, and release.
* Copies artifacts from the build to an install image folder (include files, lib files, license files...)
* Generates a `FindXXXXX.cmake` file that declares the targets, where the include files are, what static libraries, etc.

The script can also be run by a user to locally build an test the package.

The build machine zips up the install image folder, then validates the package, and uploads the zip to a non-public CDN (These steps are gated by maintainer scrutiny to avoid malicious intent).   

The O3DE repo that uses the packages can then have a pull request made that pulls the new package, since the O3DE build machines have access to the private CDN even if the public doesn't.  Once that passes AR and maintainer scrutinty, maintainers can then opt to deploy the new package to the public CDN, and the pull request to update O3DE to pull the new package can be merged.

It is quite a disconnected set of operations but it works especially well for really large binaries that take forever to compile.  

The way O3DE uses these packages is that when it realizes it needs a package, it downloads it, verifies the hash, then unzips them to a folder (`%USERPROFILE%/.o3de/3rdParty/packages` by default) and then adds the root of the unzipped package to the global path that CMake uses to 'find' packages when `find_package(xxx)` is invoked.  From there on, its pure normal CMake that kicks in.  If something in CMake calls `find_package(Blah)`, CMake will search all those folders for `FindBlah.cmake` and if it finds it, it will run it, which then declares all the libraries and stuff.  

## FetchContent

FetchContent is another normal part of cmake https://cmake.org/cmake/help/latest/module/FetchContent.html not an invention by O3DE.

When you invoke the FetchContent_xxxx commands, it is very similar to the steps for making a 3p - you give them the git url to fetch the source code from, you give it any patch you'd like to apply, and you have a chance to set any defines and so on before/during/after.

The difference though, is that FetchContent on source code will cause the source code to be pulled into your project during configure, and built during compile.  The targets inside the 3p library will appear as if they are targets in your project, ie, alongside your own targets, you will see theirs, and they will compile as part of your compilation tree.  Essentially, CMake is downloading the 3rd party source code, patching it, and then executing the CMakeLists.txt (or a different script of your choosing, should you supply it) to generate targets, which then get embedded in your normal compile step.  Specifically, the building of these targets happens during your build phase, amongst your other built targets, not during configure.

## ExternalProject

Externalproject is another normal part of cmake, https://cmake.org/cmake/help/latest/module/ExternalProject.html

Unlike FetchContent, which generates targets in your build tree with the same compile options and linker options as the rest of your code, FetchContent is isolated in its own build and compile.  Its very similar to FetchContent in that you can specify a github repo to download and patch, and a build script to execute (or just let it use cmake), and so on, but the build is isolated entirely.  

Note that the build still happens during the build phase - so you still have to (in windows) go into visual studio and trigger a build, for these ExternalProject compiles to happen, but when they do, they are executed by **custom commands** that launch a new shell instance, and run a new CMake or whatever script you supply, to build in complete isolation.  None of your project's compile flags and other things are necessarily transferred over, and its essentially like just manually downloading the 3p library source, and compiling it separately.  You then have to figure out how to use it, ie, make Find files, or declare targets, or do whatever to import it, depending on how its built.

## Case study: OpenMesh library.

OpenMesh, as in https://www.graphics.rwth-aachen.de/software/openmesh/

OpenMesh is used privately by the WhiteBox gem, which is a gem that lets you sculpt meshes via moving verts, splitting faces, extruding, and so on, directly in O3DE.  Its an impressive library, but one of the things that most makes it stand out is that its a c++ interface that allows you to deeply customize its internal data structures to use YOUR types.  So for example, you can tell it that the vertices are AZ::Vector3 vertices, and via template magic, that is what happens internally, so there's no "converting from non-native to native" data structures.  You can use your own Vector<> your own unordered_set, your own index and vertex and face types, etc.

OpenMesh currently uses the classic 3p system in O3DE, so its a zip package prebuilt.  OpenMesh was compiled using the lowest supported VC that we have, VS2019 I believe, for maximum compatibility with older toolchains and studios.

This works fine, right up until you try to use Address Sanitizer on it.  The problem is, Address Sanitizer is a compiler flag.  You build your code (especially on windows) with Address Sanitizer enabled, and it changes a bunch of defines, adds hooks in, to intercept malloc and so on.  Especially in the case of the STL, special additional things are done inside <vector> and <string> to detect common problems.

OpenMesh uses heavy template code gen to support its dynamic typing system (the Mesh class is defined by the user, and mostly exists via template manipulation so that you can substitute your own Vertex / Face / Edge classes in), which essentially results in leaking STL object manuipulation, notably, vector<int> option flags, straddling partially across its code and user code in the other module.  

This means that your heavily templated object runs *some* of STL code from YOUR version (vs2022) of STL with Address Sanitizer enabled, and *some* STL baked deeply inlined into the OpenMesh library, from VS2019, without Asan, on the same STL structures.

This blows ASAN up in a number of ways - 
* its probably just straight up memory corruption for real, as the sizeof some microsoft STL structures between VS2019 "with nothing" and VS2022 "with Asan" are not the same size, and notably vector and string specifically contain new asan-related tracking structures and things in vs2022.
* The STL library itself is probably not the same between vs2019 and vs2022.  The standard controls what happens when you call certain functions like push_back and so on, but it doesn't control how the STL implementation actually achieves it internally, allowing for different actual object layout and such.  

It is notable that of all the libraries used in O3DE, only OpenMesh is used in such a manner that triggers such an ASAN problem.  Nobody else actually uses the STL in such a way as vector or string code is called in VS2019 STL in one usage and VS2022 in another.  However, its a legit usage of the OpenMesh library - its designed to templatize in such a manner, and because of this, some of the code has to be generated in your own module, such as constructors, of these novel compound types and so on.  But methods on the baseclasses will be invoked in the library itself, especially inlines.  

This infers that usage of OpenMesh has to be made safe in terms of STL usage and compiler flags, and there's basically 4-ish ways to do this
A) Make the entire engine use the same compile options and flags that OpenMesh uses
B) Make a custom wrapper library around openmesh that does not allow any leakage of STL usage between modules, which is compiled with the same flags as OpenMesh, and then make sure all its reachable interfaces are not templatized, do not generate code, etc.   An example of C) would be to create a special O3DE-OpenMesh library that does all that template stuff, declares all the types and so on, but has a public API that uses concrete finalized "O3DEMesh" types or something, wrapping every possible OpenMesh api.
C) Make OpenMesh use the same compile options and flags that the Engine uses

A) is infeasable.   Sort of.

There is a global flag that can be set to make it so that ASAN does not add any extra housekeeping to the microsoft STL <string> and <vector> classes (and only those!).  That housekeeping seems to mainly be used to help get more information when a problem does occur.  Turning this flag on would prevent <string> and <vector> from having that extra housekeeping and make it slightly harder if a problem does occur, to pinpoint it.  But it still keeps iterator debugging, overflow detection, all the rest.  You don't lose, as far as I can tell, the ability to detect problems, just to narrow what caused them easier, if and only if, <string> and <vector> are involved.  Which, in O3DE, they are not.  We use a custom AzStd::string and vector class.  So setting this flag would actually impact very little in O3DE.

However, this flag would only allow VS2022- STL compiled without ASAN to be loaded and used in a VS2022 module that did have ASAN enabled, without it throwing a linker error about the 2 different STLs having different flags.  It does not solve the problem of VS2019 STL being used mixed with VS2022 STL.  And similar for linux different versions of STL for different clangs and gccs.

B) Is, frankly, just too much work, and defeats the purpose of using an actual 3p library whos primary superpower is that it lets you have flexible custom Vertex, Face, Edge, halfEdge and other structures kept in your own native format so there is no conversion.  instead of option B), if you're going down that path, you may as well discard OpenMesh and switch to another 3rd party library that uses a pure C api, or uses a fixed, internal "Vertex", "Face", "edge" type and you just copy the data out of those structures back and forth.  Of course, OpenMesh was chosen because the use case in WhiteBox is a fully dynamic mesh situation where the mesh is being deformed, moved, faces are being added, edges reworked, etc.  

So what about C), making sure that the OpenMesh library matches the environment you're using.

We have three ways to do that (FetchContent, 3p Package System, and ExternalProject), as listed above.  How can each be approached

### ExternalProject for OpenMesh
Doesn't really make sense, if the goal is to use the same compiler flags.  By definition, ExternalProject sub-shells with a completely blank environment, to build in isolation, for the purpose of NOT polluting the sub-target with parent environment flags.  Doing it with ExternalProject could at least guarintee the same compiler, but there is a lot of extra complexity with ExternalProject dependencies in general.

For something like this, we actually prefer if it used as much of the engine's compile architecture and compile flags as possible.  One way to achieve this would be to ExternalProject it, and manually copy as much of the compiler flags as possible across.  This is essentially like creating a whole new build system for the external project that duplicates a lot of what O3DE does in its normal build system set up to sync the two.

Theoretically, for packages that are NOT OpenMesh, that do benefit from an isolation build (ie, don't matter to have exactly the same sanitizer flags and stuff), especially standalone C packages, the ExternalProject setup might be ideal - that is, we could probably just copy the build script from the https://github.com/o3de/3p-package-source repo into O3DE, and use it in an isolated ExternalProject to construct the package locally.  At least then the compiler would likely be the same.  

### 3p System for OpenMesh
If we really wanted this to work, we'd have to have a seperate 3p package for every STL version with every compiler flag permutation.  This would mean at least having
* VS2019 (Debug, Release)
* VS2022 (Debug, Release, ASAN debug, ASAN release)
* Linux and mac, every version of every stl for each of GCC/clang with and without ASAN.

This quickly becomes infeasable due to permutation spam.

### FetchContent for OpenMesh
I successfully implemented a FetchContent approach for OpenMesh, but there are a lot of edge cases to consider.  I still think for OpenMesh in particular, FetchContent is probably the correct approach here, purely due to the STL issue.  If it wasn't for that, I would stick with 3p packages.

The main issue is that when using fetchcontent, unless you are willing to rewrite the entire build script for a target, you have to run their own build scripts, inline with your build scripts, as if it were your own, just being "included", and everything in there will execute... now, cmake is designed for this, and a well-behaved build script will do things like check if they are an embedded project and not do certain things if they are.  OpenMesh is such a fairly well behaved script, but there are edge cases.

Most notably, the following edge cases had to be dealt with for fetchcontent in OpenMesh:
* OpenMesh does not compile at warning level 4 + warnings as errors.  In the 3p package, of course, its built in isolation and warnings are not turned to level 4.  In fetchcontent, it embeds it in the project, so it uses the same everything that O3DE does.  Which includes warnings as errors, W4.
    * Solved by calling `target_compile_options` on the OpenMesh targets, turning off warnings, etc.
* OpenMesh assumes `-fvisibility=default`.  O3DE changes it to `hidden`, which makes it export nothing into its library.
    * Solved by calling `target_compile_options` on the OpenMesh targets, turning it to default just for them.
* OpenMesh uses exceptions.  O3DE turns them off.
    * Same solution.  Turned it on to use exceptions, and did so in a way that modules using it would also get exceptions recursively.  Only WhiteBox.Editor module uses it, so it does not spread far, and certainly not into the runtime.
* OpenMesh's own CMake scripts had a number of issues on them in our version (8.1) which required an upgrade to 11.0, which fixed most of them.
    * Solved via implementing a special patch function that can detect if a patch is already applied and upgrading to 11.0.
* OpenMesh uses its own special build system which is known as VCI https://gitlab.vci.rwth-aachen.de:9000/cmake/cmake-library which tends to print things out in the console, which we don't want.
    * Solved via `set(CMAKE_WARN_DEPRECATED OFF CACHE BOOL "" FORCE)`  as well as `set(CMAKE_MESSAGE_LOG_LEVEL ERROR)` scoped around the block that fetches and runs it (restored again after)
* OpenMesh's VCI build system only obeys "Build static libraries only" on win32.  Even setting `OPENMESH_BUILD_SHARED=OFF`, all usage of that variable inside thier build scripts are encased in a `if (WIN32)` block.  This means on linux, it always makes a shared library called OpenMeshCore AND a static library called OpenMeshCoreStatic.  But on windows, since we pass that flag, it doesn't make an OpenMeshCoreStatic library, only a OpenMeshCore library.  We want the static library.
    * Solved by checking for a `OpenMeshCoreStatic` library, and if present, making sure that our `3rdParty::OpenMesh` target was an alias of that one instead of `OpenMeshCore`
* For some reason, they invoke install() only on `OpenMeshCore` - which means the static library if present is never copied to the install tree.  This may be a bug in their cmake scripts or intentional, either way...
    * Solved by calling ly_install if the static target is present.

Ultimately, not too much work, but it does highlight the strengths and weaknesses of using FetchContent.
* You have to understand and know exactly what the foreign build files are doing.  You must essentially read and understand their own build system.  For every package.
* Any command that the foreign build files do, you can't avoid calling them unless the foreign build scripts are nice enough to define a configuration variable like "DONT_DO_THING_X".  Many do - but not always.  For example, on linux, OpenMesh unconditionally calls `install(OpenMeshCore)` which is a shared library.  This results in `OpenMeshCore.so` unconditionally ending up in install/lib in an o3de installer.  we don't need it.  Its not wrong to ship it, but there's no way to turn it off because the build script doesn't provide the option to do so.  Note that we could patch the build script.  But is it worth it?
* Patching and testing patches IS a lot easier with fetchcontent.
* It builds as normal targets in your own source, so you can browse the source, put breakpoints, etc.  It does cost you compile time though.
* It will always have the same compile flags - sort of.  Its still running their CMake, and their cmake can still do whatever it wants.  For example, the OpenMesh build system doesn't understand Ninja Multi-Config, or any multi-config, really, so it assumes its release.  However, because we set specific compiler flags based on config at a global level, it still works as intended.

## Comparison of each approach (Conclusions)

This document is not actually arguing for any given approach.  I think all three have their place in the O3DE ecosystem.  But some are better tools for specific jobs.  Below I will only be listing the things that *differ* between the approaches.

### 3p Package System
* Binaries are pre-built, users of O3DE, even those compiling from code, never build these
   * Pro:  Build Times for those compiling from code are diminished.  For most packages, this is just a few seconds, and only once.  But for big packages like Qt, or Python, it can shave hours off.
   * Pro:  Companies can host their own 3p binary blobs and add their own server(s) to pull from, like Cesium did.  This might especially be useful in circumstances where the company intends NOT to release source.
   * Con:  Its a bit mysterious - these are 'blobs of binaries' you have to trust from a CDN.  This can rub some the wrong way, mitigated by the fact that the sources to generate them are public and they can build their own.
   * Con:  You have to pre-build every permutation for every circumstance.  Windows Debug, Windows Release, Linux Debug, Linux Release, Mac Debug, Mac Release, etc.  You have to have the knowledge and ability to craft build scripts for all of those.  In cases where there are too many permutations due to compile flags, it can become overwhelming.
   * Con:  Debugging through these, managing the sources, symbols, etc, can be difficult.  For example, MCPP does not come with any debug symbols, so when it crashes, its a mystery crash, that cannot be stepped into.
   * Con:  Working on a new platform or architecture (like mac aarch64 or a console or something) means making new packages for every single third party dependency.
   * Con:  Adds one more point of failure - ie, being able to install the installer, but having the CDN fail or something.

* Builds are isolated
   * Pro:  Even a really badly misbehaving weird build system that only knows how to build itself with some obscure build tool can be used, even if it means just running the build tool, seeing what it outputs and where, and then writing a custom script to copy files into the structure you want.  Each build is isolated, can be hosted in Docker or something, and won't affect O3DE's build environment.
   * Con:  Permutation explosion for build flags.
   * Con:  If we need to globally update something in O3DE's build options which is incompatible, all 3p binaries would have to be rev'd.

* The process is somewhat disconnected, with many barriers to pass thru
   * Pro:  This helps prevent mistakes as a maintainer has to sign off on numerous steps and helps put the brakes on things like new 3ps creeping in.
   * Con:  The many steps do make it a lot more onerous to make simple upgrades to existing 3p libraries such as incrementing their revision.
   * Con:  Testing your changes as you work on a 3p is kind of a pain, as you have to essentially constantly rebuild the package, deploy it somewhere, and then go to o3de, update it to point at the package, fetch it again, only to find a mistake and start over.
   * Con:  All a maintainer sees in o3de's repo when a 3p does change is a change in a hash to a version to grab.  Its actually difficult to verify whats in there.  A one line change in O3DE PR with not a lot of "whats going on behind it" except for the explanation in the PR text.

* Parts of the 3p system are inventions by O3DE.
   * Con:  This is "yet another package system" syndrome.  Although the implementation of it is as lightweight as possible (it literally just makes a zip file with a manifest file at the root) and the usage of it on the o3de side is lightweight (just downloads the package using cmake's built in download and unzip facility) it can still be a barrier to entry to contributors who know cmake to have to learn "Yet another package system".

Ideal for large libraries that take a long time to compile, or are very complicated to compile or need very special custom treatment or post-compile steps to assemble them, as well as small but fundamental libraries (Zlib for example) that you want to use to compile all the other big ones in the package system, with the same exact settings and flags. 

### ExternalProject
* Builds as a custom build step during project build
   * Pro:  Isolated build also, so there's no way the engine can affect it nor can it affect the engine except in ways you manually choose to do.
   * Con:  Pretty much all the downsides of in-code build, but without the benefits of a prebuilt 3p package in terms of avoiding the time.
   * Con:  Most example usages involve making your own project an Externalproject too, which depends on the other Externalprojects, so its kind of an architectural change that O3DE isn't suited for.
   * You have to essentially do the same work that the 3p package build script has to do in terms of, after the project finishes building in its isolated environment, setting up targets, creating/running Find scripts, and so on.  
   * Pro:  There's a chance that it works on new platforms, with little or no change, since a lot of the architecture settings come from examining the platform its building on.

Ideal: Unknown.  I can't really think of a library that would work better as Externalproject than 3p or FetchContent in the context of O3DE.

### FetchContent

* Runs the cmake scripts directly inside the project they're in, as if they are part of the project with the same build flags and settings as the project
  * Pro:  They inherit all the compiler flags of the surrounding project, settings, architecture, toolchain, build type, everything, so tools like Address Sanitizer can be toggled on and off and code will rebuild as appropriate.
  * Pro:  Overhead is minimal, and its often faster to download sourcecode than binary since source is so much smaller
  * Pro:  source can be gotten from the original author, and verified
  * Pro:  Updating to a new version of an existing library is a PR that shows the changes involved (new git version, tag, etc) and any changes to any patches or o3de code required, in one change, easy to cherry pick or merge.
  * Pro:  Libraries can Just Work on unknown or new platforms or architecture, since they compile with the same flags as the rest of the code.  Nobody has to make a special new build script.  the same one might work for all platforms, which can result in far less maintennence cost.
  * Pro:  Does not actually affect the installer version - that is, they still ship with the prebuilt libraries, just as part of the installer, instead of fetched separately from CDN.
  * Pro:  Since the source is right there, symbols are available in all situations where engine source symbols are available for stepping and debugging.
  * Con:  Some of the compile flags for your project might not be compatible with the 3p library and you may need to tweak it
  * Con:  the library builds as part of your project, that means increased build times (by however long it takes!) on clean builds.  Use only for small libraries!
  * Con:  The build script of the library in question is run inside your own build script.  If it is written poorly (modifying global flags and so on), it can cause side effects.  If it doesn't come with a lot of options to turn things off, it might make too many targets, or targets you don't want, like tests.
  
* Fetchcontent is fairly well understood and widely used in CMake, its not an invention of O3DE.
  * Pro: Lots of examples and such out there.  Cmake people seeing it will understand it and there's no hidden magic package system.

Perfect for:  header-only libraries, which remove all of the cons!

Ideal for:  Smaller, quicker compiling libraries whos cmake scripts are fairly standard.

Required for: Libraries coded in ways that mean that the library MUST have the same version of the compiler / STL / compiler flags as the engine using it.

I'd recommend we move the following libraries to fetchcontent over time:
* assimp (may be too large)
* cityhash
* freetype
* googlebenchmark
* libsamplerate
* mcpp (or use something else)
* mikkelsen (if still used?)
* Open mesh (In progress)
* RapidJson
* RapidXML
* xxhash
* astc-encoder
* Squish-CCR (still used???)
* vulkan-validationlayers

The following would be nice but other 3p that will not be moving to fetchcontent depend on them:
* zlib
* lz4
* png
* expat
* freetype
* poly2tri (is still useD?)
* pybind11
* SQLITE
* tiff
* OpenEXR
* OpenSSL

The following take too long and/or are too complicaed to compile to use them inline
* AZSLC (?) is it?
* DirectXShaderCompilerDxc
* ISPCTexComp
* Lua
* NvCloth
* OpenImageIO
* PhysX4 and Physx5
* Python
* Qt
* SPIRVCross
* V-hacd

## Installer peculiarities

The usage of a 3rd party package in code does not necessarily mean that the 3rd party library needs to be downloaded or used when a user builds using a pre-built installer of O3DE.

For example, suppose you had a 3rd Party Library consisting of
* 3rdParty/MyStuff/Code/MyStuff.cpp
* 3rdParty/MyStuff/include/MyStuff/MyStuff.h (API)

And you write a gem which uses it.

Assuming your gem has the standard gem structure, ie,
* A private static library which is used by all modules
* A public API library (which itself could be header-only or static)
* A Gem DLL (Which uses the private static)

Then the 3rd party library only needs to be accessible by installer users if it is "visible" to them, ie, if the public API library, which is the only thing they are supposed to link to or use includes from, itself uses the 3rd party library.

If all usage of the 3rd party library is from a private header (Which is not shipped) or from a private cpp file (which is not shipped, since its turned into a shipped dll), then the 3rd party library does not need to be included in the installer.

Recommended structure (tenative)
```
* YourGem (root)
    CMakeLists.txt (root)
    * 3rdParty (folder)
          * FindMyStuff.cmake    <--- goes into the built-from-code engine
          * Installer (Folder)
               * FindMyStuff.cmake    <--- goes into the installer

     * Code (folder)
          * CMakeLists.txt <-- the normal cmakelists file for the code
```

In cases where the actual library and headers are not needed in the Installer, its still potentially useful for the FindMyStuff.cmake in 3rdParty/Installer to include a message indicating that the 3p is still present (inside the gem dll), for disclosure reasons, and to declare empty interface targets so that nothing complains.  

In cases where the actual library and headers ARE needed in the installer, you can choose then to either pull them via fetchcontent similar to the code version, or fetch them from the installer `lib` folder (FindMySTuff.cmake in the normal code version would have to export the libs and headers and whatnot).


