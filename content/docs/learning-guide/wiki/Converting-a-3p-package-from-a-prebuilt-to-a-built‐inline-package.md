---
title: "Converting-a-3p-package-from-a-prebuilt-to-a-built‐inline-package"
description: ""
toc: false
---

This page is the practical guide for actually doing the work to convert from using a downloaded 3p package to using a built-inline 3p package.

For a full overview of whats involved, see the deep dive at [This wiki page](https://github.com/o3de/o3de/wiki/O3DE-3p-Package-System-vs-FetchContent-vs-ExternalProject-investigation)

## When is it suitable to do this?

Using inline libraries via fetchcontent can bring many benefits (see the above wiki page for pros and cons) but please limit it ones which meet the following criteria
* The are approved by the 3P approval process (TSC, O3DF).  The existing 3p packages are already approved.
* They compile very quickly (This is a difficult metric to follow, but an example for me is taking about 2 hours to compile O3DE, adding 30 seconds on, would not be out of the question).  Header-only libraries automatically meet this criteria since they take 0 seconds to compile.
* They download quickly (Please no repos that contain gigabytes of 'example data')
* They can be compiled using their existing CMake scripts with no (or low amount) of patching in our engine.  If you have to build a whole new build system for them, they belong in pre-built [3P Package system](http://github.com/o3de/3p-package-source).
* They don't need much Platform Abstraction Layer stuff to do completely different things on every platform during compile.
* Nothing in the 3p Package System depends on their binaries.  For example, zlib meets all of the above criteria, but Qt, Python, and several others which don't all depend on it.  So it stays in the 3P Package system so that the compile scripts for the other packages can depend on it, and then the engine can depend on the exact same package.

## When is it mandatory to do this?
* When the library interface does things which make it extremely sensitive to the compiler or linker flags such that they must match absolutely exactly or else bad things happen.  An example is OpenMesh, which lets some STL objects (vector) leak out of its interface in such a way that causes them to be initialized in the library and destroyed in the user of the library.  This causes problems with memory tools like Address Sanitizer and others.

## How?

First, you would remove any existing code that registers the existing 3p, so search for it and eliminate any `ly_associate_package` calls or other `include` calls which try to run scripts.  Leave the dependencies on `3rdParty::xxxx` intact.

See [The Whitebox Gem](https://github.com/o3de/o3de/tree/development/Gems/WhiteBox) which uses OpenMesh 3p as an inline compile, for an example of this

The basic folder structure is as such for a 3p called XXXX.

```
+ CMakeLists.txt (cmake root file)
+ 3rdParty (folder)
    + FindXXXX.cmake (file)
    + o3de-XXXX.patch (optional patch file)
    + Installer (folder)
        * FindXXXX.CMake (file)  # this one runs when in a pre-built installer
```

**Everything is case sensitive!  If things depend on your library named `3rdParty::XxXxX` then your Find files must be named `FindXxXxX.cmake` and your targets must be named similarly.**

### The CMakeLists.txt at the root

O3DE Automatically invokes `find_package(XXXX)`, a builtin CMake function, when someone declares a dependency on `3rdParty::XXXX` (it subtracts the 3rdParty bit first).   
CMake automatically searches a list of registered locations (`CMAKE_MODULE_PATH`) for FindXXXX.cmake if someone invokes `find_package(XXXX)`, and executes the file if it is found.  It expects that file to import or declare built targets.   It also expects it to set the variable `XXXX_FOUND`.
The CMake Root file thus just adds the 3rdParty folder to the `CMAKE_MODULE_PATH`, so that the FindXXXX.cmake file will be run if someone declares a dependency on it.

```cmake
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/3rdParty)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH})
```

### The FindXXXX.cmake in 3rdParty
This is where most of the action occurs.
When this is invoked, its job is to declare `3rdParty::XXXX` in any way is necessary and then set `XXXX_FOUND`.
CMake doesn't really care how it achieves this - `IMPORTED` libraries, or specifying source code to compile.
See the [OpenMesh FindXXXX.cmake](https://github.com/o3de/o3de/blob/development/Gems/WhiteBox/3rdParty/FindOpenMesh.cmake) for a full example which goes through the process.  You'll have to highly customize this depending on the package.

The above file gives a pretty clear walkthough, but I'll go thru the phases here

#### Phase 0:  Early out if you're already declared
```
if (TARGET 3rdParty::XXXX)
    return()
endif()
```

This is like an include guard but it also lets users substitute their own 3rdParty::XXXX if they want to by getting it in there at a higher level earlier.

#### Phase 1:  FetchContent_Declare
Tell it where to get the source, or the zip file, or whatever.  It works with git repos, zip files, etc.
Always pin it to a specific tag or hash.

If you need to make a patch, or use a patch, use `PATCH_COMMAND cmake -P "${LY_ROOT_FOLDER}/cmake/PatchIfNotAlreadyPatched.cmake" o3de-XXXX.patch` because CMake tends to try to re-patch already patched things and fail.

Do not use CMAKE_ARGS here, they don't work.  FetchContent does not start a new instance of cmake, so command line arguments are not re-parsed.

#### Phase 2:  FetchContent_MakeAvailable
The command `FetchContent_MakeAvailable` actually fetches the source (via git or unzips a zip etc), and then runs its CMake file as an inline run (similar to `add_subdirectory` or `include()`.  If it doesn't have a cmake file, it does nothing.  You can also give it a bogus CMake directory in `FetchContent_Declare` to make it do nothing on purpose (`SOURCE_SUBDIR bogus`) or you can point it at a custom CMakeLists.txt file you put in 3rdParty using `SOURCE_SUBDIR` to point it there.

Since it is not starting a new instance of CMake to do a subbuld, setting CMAKE_ARGS doesn't do anything.  You must `set()` variables before invoking `FetchContent_MakeAvailable` for them to apply here.  This is why the `openmesh` example does it inside a function - to limit its scope that these variables are set.  You don't have to do that, if the library doesn't allow it - just consider saving global variables and then setting values back after the `FetchContent_MakeAvailable` call returns.

Once `FetchContent_MakeAvailable` returns, the CMakeLists.txt in the downloaded source has run, and it has added any targets it has declared into the O3DE environment.  Those targets will have the same flags as any other O3DE target, such as having debug flags in debug mode, etc. 

Ideally you disable the auto-installation of targets if possible, since we want to carefully control when/how things get installed.  Most 3p libraries have an option to turn off installation.  Built in installation files often do things like generate cmake stuff or make .pc files, which we don't need.  

If all else fails, you can either patch their code, or write your own simpler CMakeLists.txt using the `SOURCE_SUBDIR` variable.

### Phase 3:  Adapt the targets to O3DE (compiler settings, linker settings, install, etc)
This is your chance to change any compiler settings on the targets from the 3rd Party.
Useful tools
```
# target_compile_options APPENDS compile options to the target.
 target_compile_options(XXXX ${O3DE_COMPILE_OPTION_DISABLE_WARNINGS})     # XXXX does not pass Warning Level 4

# target property FOLDER makes it show up in that folder in Visual Studio and other IDEs.
# If the 3p is used by others besides this gem, consider putting it in 3rdParty rather than relative to gem.
# The path is relative to the root of the project folder structure.
get_property(this_gem_root GLOBAL PROPERTY "@GEMROOT:${gem_name}@")
ly_get_engine_relative_source_dir(${this_gem_root} relative_this_gem_root)
set_property(TARGET XXXX PROPERTY FOLDER "${relative_this_gem_root}/External")
```

#### Phase 4: Alias the targets for O3DE and setup installer.
O3DE Expects `3rdParty::XXXX` but the CMake script for the target probably made something like `XXXX` or `SomeCompany::XXXXX` or a series of targets.  O3DE's install system also expects there to be a `XXXX` as well as `3rdParty::XXXX`.

For very simple libraries, this might be as simple as aliasing them to the basic XXXX library generated by the 3p's cmake script:
```
add_library(3rdParty::XXXX ALIAS XXXX)
```

For more complicated ones, you may need to do a bit more work, looping over all their targets, and calling add_library as appropriate.

Avoid calling `ly_create_alias` unless you're confident.  
`ly_create_alias(NAMESPACE 3rdParty NAME XXXX TARGETS YYYY) This function does 3 things
1. creates an INTERFACE library called `XXXX` which is O3DE's name, that depends on `YYYY`, the internal 3p's name for itself.
2. creates an alias called `3rdParty::XXXX` to `XXXX`
3. Registers `3rdParty::XXXX` with the installer autogeneration system that will generate fake import targets for you.

Point 1 above might fail if XXXX` already exists, ie, if the 3rd party CMake script for package `zlib` already makes a target called `zlib`.  So don't use ly_create_alias in that situation.  O3DE does this because often the build scripts ask for something like `3rdParty::OpenMesh` but the actual 3rd Party library defines something like `openmesh::core`, `openmesh::tools` etc.  This allows the `3rdParty::XXXX` as well as just `XXXX` to be chosen as the primary target (or even a target that links all the other ones).
  
3 Is probably not desired.  It *might* work, since the autogeneration looks at what include files and lib files which are generated, and if it does, in your testing, you don't need to make the 3rdParty/Installer/FetchContent.   

#### Installation
If you disabled install on the target, which you may have to do to avoid name collisions, you will have to handle it yourself.
But even if you didn't, you probably want to tell O3DE to install the misc files like the installer FindXXXX.cmake to the installer image.
Directories are relative to the root of the install image.
 
```
ly_install(FILES ${CMAKE_CURRENT_LIST_DIR}/Installer/FindXXXX.cmake DESTINATION cmake/3rdParty)
ly_install(FILES ${CMAKE_CURRENT_LIST_DIR}/XXXX-o3de.patch DESTINATION cmake/3rdParty)
```

Note that cmake/3rdParty is already part of the CMAKE_MODULE_PATH in an installer, so doesn't need any addition to module paths when running there.

once this is done though, you can invoke
`set(XXXX_FOUND TRUE PARENT_SCOPE)` in the script to signal success.  if you're not inside a scoped function, remove the `PARENT_SCOPE`

### The FetchXXXX.cmake in the 3rdParty/Installer

This one is meant to be copied into the installer (see the last part of the above section).
The original cmake files don't run when in the installer.   Instead, O3DE autogenerates `IMPORTED` (prebuilt) targets.  In our case, what happens here greatly depends on how the library is used.

In the case of OpenMesh, its not actually needed at all during runtime or installer time, because the entire 3p library is only PRIVATELY used by WhiteBox gem, in only its private library, and no public header which we ship actually includes any part of OpenMesh.  This is a pretty common pattern, and in that case, the targets merely have to exist.

See (The Open Mesh installer library)[https://github.com/o3de/o3de/blob/development/Gems/WhiteBox/3rdParty/Installer/FindOpenMesh.cmake] for an example, it just makes the XXXX and 3rdParty::XXXX libraries that are expected, and prints out the "using" message so the user understands what 3rd party libraries they are using.

Note that if you did need the libraries here, in the installer (they are publically used), you will need to make a judgement call.
1. Make sure that you use ly_install(....) in the original Cmake script to ensure that the .lib, .a, .so, .h files are deployed if you turned off the original 3p install machinery.  Consider using a different folder for debug vs release or adding a wart onto the file name (blahblah_d) or something, for debug vs release, and then put them in the same folder.  Check the install folder for layout.
2. In the "Installer" version of your FindXXXX.cmake you would declare them as imported
```cmake
set_target_properties(TARGETS XXXX
                        PROPERTIES 
                          IMPORTED_LOCATION ${LY_ROOT_FOLDER}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}xxxx${CMAKE_STATIC_LIBRARY_SUFFIX}
                          IMPORTED_LOCATION_DEBUG ${LY_ROOT_FOLDER}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}xxxx_d${CMAKE_STATIC_LIBRARY_SUFFIX})
```
and you would add the include paths as usual, depending on where you installed.  Use ${LY_ROOT_FOLDER} as the project itself might be running out of a different project folder.

## Testing the installer
Configure O3DE **WITHOUT** a project configured.  For now, turn test modules off, too.
```
windows:
cmake -S . -B build/windows -ULY_PROJECTS -DBUILD_TESTING=OFF -DLY_DISABLE_TEST_MODULES=ON -DO3DE_INSTALL_ENGINE_NAME=o3de-installer
cmake --build build/windows --config debug --target install -- /m
cmake --build build/windows --config profile --target install -- /m

linux
cmake -S . -B build/linux -ULY_PROJECTS -DBUILD_TESTING=OFF -DLY_DISABLE_TEST_MODULES=ON -G"Ninja Multi-Config" -DO3DE_INSTALL_ENGINE_NAME=o3de-installer
cmake --build build/linux --config debug --target install
cmake --build build/linux --config profile --target install
```
This will make an installer **called** o3de-installer in the `install` subfolder of the current directory and then build debug and profile libraries into it (you can omit one of those if you just want a quick test with just profile or just debug).  Every different permutation you build additively puts those libraries in there.

Once you have made the installer you can go register it
```
windows:

cd install
python\get_python
scripts\o3de register --this-engine

linux:

cd install
./python/get_python.sh
./scripts/o3de.sh register --this-engine
```

You can then run o3de from the bin folder of install and make projects to test it with.  Test it with those projects, make sure they build with your library.

## Iterating on patch files.

Sometimes you have to patch a library.  Doing this with fetch content makes it super easy.
The libraries are downloaded into (buildfolder/_deps/XXXX_src) so after you do your initial fetchcontent (let it run and fail) you can cd into that folder like:
`cd build\windows\_deps\XXXX_src` and you will be in a git repo for the software.  Use your favorite IDE to make whatever change you want.

Once you've made the changes, create/update the patch file.  From the XXXX_src folder:
```
git diff > (Full absolute path to your 3rdParty folder in your gem where the FindXXXX.cmake file lives)\o3de-XXXX.patch
```

If its the first time, add 
`PATCH_COMMAND cmake -P "${LY_ROOT_FOLDER}/cmake/PatchIfNotAlreadyPatched.cmake" o3de-XXXX.patch` to the `FetchContent_Declare` block to make it use the patch.

And then re-run the O3DE configure and build as normal.  Repeat as necessary.  If the patch is new, make sure to include it in the shipped version with `ly_install` to make sure everyone understands what they're getting into, and disclose it in your `message(STATUS "` block like openmesh example does.


