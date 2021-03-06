# What is CMake?
> CMake is an extensible open-source system that manages build process in an 
operating system and in a compiler-independent manner. CMake is designed to be
used with native build environments.

>Simple configuration files in each source directory(called CMakeLists.txt) are
used to generate standard build files(Makefiles in Linux and 
Projects/Workspaces in Windows) which are used in usual way.

CMake can;
1. Generate native build environment that will compile code
2. Create libraries
3. Generate Wrappers
4. Build executables
in any arbitrary combination.

CMake can support in/out-of-place builds and therefore can support multiple
builds from single source tree. CMake also supports static and dynamic library
builds.

In short conclusion: CMake helps to you manage build your source codes 
effectively.

# Quick Start

Create the example file below and save it.

```
// test.cpp
#include <iostream>
using namespace std;

int main(void) {
     cout << "Hello World" << endl;
     return(0);
}
```

Create the example CMakeLists.txt below and save it.

    ```
    # Specify the minimum version for CMake

    cmake_minimum_required(VERSION 2.8)

    # Project's name

    project(hello)
    # Set the output folder where your program will be created
    set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
    set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
    set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

    # The following folder will be included
    include_directories("${PROJECT_SOURCE_DIR}")

    # Build target from source file
    add_executable(hello ${PROJECT_SOURCE_DIR}/test.cpp)
    ```

- ``project(ProjectName)``: This command names our project. If project name 
includes spaces, you should write name between quotes(").
- ``add_executable(TargetName <src1> <src2>...): This command tells CMake
you want to make an executable and add it as a target. First argument is the
name of executable and rest is source files.

## List of CMake Global Variables

- `CMAKE_SOURCE_DIR`: The directory which CMake was started from(Top level 
    directory)
- `CMAKE_BINARY_DIR`: If you are building in-source, this is the same as 
    `CMAKE_SOURCE_DIR`, otherwise top-level directory.
- `EXECUTABLE_OUTPUT_PATH`: Set this variable to a common place where you want 
    CMake should put your executable files into.
- `LIBRARY_OUTPUT_PATH`: Set this variable to a common place where you want 
    CMake should put all libraries into.
- `PROJECT_NAME`: A name given in __CMakeLists.txt__ file with `project()` 
    command. 
- `PROJECT_SOURCE_DIR`: Contains the full path of your project source 
    directory.

To compile the test cpp, add the line below to the end of "CmakeLists.txt";
`add_executable(hello ${PROJECT_SOURCE_DIR}/test.cpp)`

Now time has come to build the source code with CMake. Type the commands below;
```
$ cmake -H. -Bbuild
$ cmake --build build
```

First command will create CMake build files inside folder "build" and second 
command  will generate output of the program hello inside "bin" folder.

Go try your executable now(Wherever it is generated).

# CMake Study Ver. 2
I found [CMake Tutorial](https://www.johnlamp.net/cmake-tutorial.html) given 
in the link better than first part of the one helped me to compose first part of
this document.

## Introduction
### CMakeLists.txt
Just as any IDE has project files or Make has Makefiles, CMake has 
__CMakeLists.txt__ . These describes your project to CMake and affects its 
outputs. Below is an example CMakeLists.txt

```
project("To Do List")

add_exectuable(toDo main.cpp ToDo.cpp)
```

- ``project(ProjectName)``: This command names our project. If project name 
includes spaces, you should write name between quotes(").
- ``add_executable(TargetName <src1> <src2>...): This command tells CMake
you want to make an executable and add it as a target. First argument is the
name of executable and rest is source files.

### Source Files
CMake's documentation highly suggests that out-of-souce builds to be used 
instead of in-source-builds. It has an advantage of isolating build files from 
source code. This way, it is easier just to delete build folder and move on
when we want to clean build files really.

To build your first project, type the commands below;

```
# Type this when you are in root of your project directory.
$ mkdir build

# Change directory to build
$ cd build

# Build
$ cmake .. -G "Unix MakeFiles"

```

- ``mkdir build``: build directory does not have to be in your source directory.
- ``cmake .. -G <generator name>``: This allows us to tell CMake what kind of 
project files it should generate. Use ``cmake --help`` to see list of generators
supported by your platform. We used (..) since __CMakeLists.txt__ is in one 
upper level. Without it, we will have an error message saying __CmakeLists.txt__
could not be found.

CMake generates several files that shall not be touched. __Makefile__ is the 
most important since we will use it to build project. __CmakeCache.txt__ is 
important for CMake since it stores various information and settings for the 
project.

Finally you can type ``make`` command in the directory of MakeFile to build
project. Use ``make VERBOSE=1`` allow more output during build.

# IDE Integration
CMake could create projects for various IDEs. This is one of CMake's greatest
strengths as it allows for very diverse development environments while working
on the same project. You can use Emacs/Make combo to develop/build project and 
use an IDE to exploit its refactoring at the same time. 

> MORE POWER TO THE DEVELOPERS

Author's note: At this chapter, I will only include Visual Studio project 
generation for CMake as other build models are irrelevant to my professional 
career. Maybe i will also include Eclipse project generation.

Use ``cmake --help`` command to see supported build models.

## Visual Studio
Creating Visual Studio project files is simple. Learn which Visual Studio 
version you have installed on your machine and then type the commands below
Note: Finding Visual Studio version and using correct phrase is important. Even
though my Visual studio says it is 2012, it is installed under Microsoft Visual
Studio 2011 directory. 

```
# Create a build directory under your source  directory and cd to it.
$ mkdir VisualStudioBuild
$ cd VisualStudioBuild

# Use your generator with directory of your source as parameter(..)
$ cmake -G "Visual Studio 11" ..
-- The C compiler identification is MSVC 17.0.61030.0
-- The CXX compiler identification is MSVC 17.0.61030.0
-- Check for working C compiler: C:/Program Files (x86)/
   Microsoft Visual Studio 11.0/VC/bin/cl.exe
-- Check for working C compiler: C:/Program Files (x86)/
   Microsoft Visual Studio 11.0/VC/bin/cl.exe -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files (x86)/
   Microsoft Visual Studio 11.0/VC/bin/cl.exe
-- Check for working CXX compiler: C:/Program Files (x86)/
   Microsoft Visual Studio 11.0/VC/bin/cl.exe -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: D:/GitWorkplace/reflections/
   CMakeWorkspace/CMakeSecond/VisualStudioBuild
```

If you check your build directory you will see there are many files generated by
CMake. The most important one is __To Do List.sln__ . The generated solution is 
more complicated than it would be if we created by hand. There are 2 more 
projects in addition to ``toDo``. Let's see what is what.

### ALL_BUILD
This project builds all of the targets that are defined in the 
__CMakeLists.txt__ . Since we have only one, this build is redundant.

### ZERO_CHECK
This is a rather oddly named project. It’s purpose is to make sure that the 
Visual Studio solution and its projects are all up to date. If you modify the
CMakeLists.txt this project will update your Visual Studio solution. All other 
projects depend on this one so you don’t have to build it manually. 
Unfortunately when the solution and projects are updated by this Visual Studio 
will, for each one updated, ask you if you want to reload it, which can get a 
bit annoying.

Note: If you check toDo project, you will see it only contains __.cc__ files 
since we did not include ``todo.h``.

If you need to build from a command line or from an automated process, type;
``MSBUild ALL_BUILD.vcxproj``. In your Visual Studio installation directory, 
open command line and type ``where msbuild``. Save the path to system 
environment variable PATH. 
Note: After the steps above, if cmake gives error like " msbuild: command not 
found", type ``msbuild.exe ALL;BUILD.vcxproj`` instead.
Author's note: Visual Studio and CMake does not make the world's best couple.
There may be multiple problems along the way to get this combo work. But 
Internet is a great source to overcome these problems.

# Libraries and Subdirectories
So far,  our project was simple. But a real life project would be much more 
complex. Let's add more subdirectories, libraries and unit tests to the mix.
We will split our project to have a library which we can put in a subdirectory.

We will make the ToDo class its own library and put it in a subdirectory. Even 
though ToDo is a single source file, it will be compiled as many times as it is
included in the targets. Imagine if you got hundreds of commonly used source 
files.
```

cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
set(CMAKE_LEGACY_CYGWIN_WIN32 0)

project("To Do List")

enable_testing()


if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR
    "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    set(warnings "-Wall -Wextra -Werror")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    set(warnings "/W4 /WX /EHsc")
endif()
if (NOT CONFIGURED_ONCE)
    set(CMAKE_CXX_FLAGS "${warnings}"
        CACHE STRING "Flags used by the compiler during all build types." FORCE)
    set(CMAKE_C_FLAGS   "${warnings}"
        CACHE STRING "Flags used by the compiler during all build types." FORCE)
endif()


include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_subdirectory(ToDoCore)

add_executable(toDo main.cc)
target_link_libraries(toDo toDoCore)

add_test(toDoTest toDo)


set(CONFIGURED_ONCE TRUE CACHE INTERNAL
    "A flag showing that CMake has configured at least once.")

```

As you can see now, our target ``toDo`` now depends only on main.cc and 
``toDoCore``. The project also has a new directory called __ToDoCore__.

Now let's dissect the commands used in this CMakeLists.txt

- ``include_directories(directories)``: Adds directories to the end of directory
list this project looks for files to include. We did not use this before since
all files were in same directory before.

- ``CMAKE_CURRENT_SOURCE_DIR``: The full path to source directory which is 
processed by CMake currently.

- ``add_subdirectory(source_dir)``: Include __source_dir__ in your project. This
directory must contain __CMakeLists.txt__ file. For now, we are omitting second
parameter of this command. It only works when you are adding sub-directory of
the current directory.Check "CMakeLists.txt" to see we only give relative 
directory name instead of full path.

- ``target_link_libraries(target library…)``: Specify that target needs to be 
linked against one or more libraries. If a library name matches another target 
dependencies are setup automatically so that the libraries will be built 
first and target will be updated whenever any of the libraries are.

If the target is executable, it will be linked against the listed libraries.
If the target is not executable, it's dependency on these libraries will be 
recorded. Then when someoneis linked against target, it will also be linked 
against target's dependencies. This makes it easier to handle a library's 
dependencies as you only need to define dependency once when library is created.

We added a subdirectory that should have __CMakeLists.txt__ in it. Let's see 
what is in there;

```
add_library(toDoCore toDo.cc)
```
Let's see generic version of this command.

`` add_library(target STATIC | SHARED | MODULE sources…)``

This command creates a new library target built from sources. 

- ``STATIC``: Used to create archive of objects that are linked directly to 
other targets.
- ``SHARED``: Shared libraries are linked dynamically and loaded at runtime.
- ``MODULE``: Module libraries are plug-ins that are not linked but loaded at
  the runtime.

Generate your project files first. Then get into desired build directory, build
your project. Let's check the output of these operations

```

Cagri@DESKTOP-GAM3TQS MINGW64 /d/GitWorkplace/reflections/CMakeWorkspace/third/B                                     uild (master)
$ cmake -G "Unix Makefiles" ..

-- The C compiler identification is GNU 5.1.0
-- The CXX compiler identification is GNU 5.1.0
-- Check for working C compiler: C:/TDM-GCC-64/bin/gcc.exe
-- Check for working C compiler: C:/TDM-GCC-64/bin/gcc.exe -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: C:/TDM-GCC-64/bin/c++.exe
-- Check for working CXX compiler: C:/TDM-GCC-64/bin/c++.exe -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: D:/GitWorkplace/reflections/CMakeWorkspace
    /third/Build

$make

Scanning dependencies of target toDoCore
[ 25%] Building CXX object ToDoCore/CMakeFiles/toDoCore.dir/ToDo.cc.obj
[ 50%] Linking CXX static library libtoDoCore.a
[ 50%] Built target toDoCore
Scanning dependencies of target toDo
[ 75%] Building CXX object CMakeFiles/toDo.dir/main.cc.obj
[100%] Linking CXX executable toDo.exe

```

See that first toDoCore, then toDo.exe is built.






