---
layout: post
title:  "Add External git project in cmake"
date:   2019-12-14 12:00:00 -0400
categories: cmake external lib git ExternalProject_Add
---

I've strugle to find a correct and easy way to integrate external project into my personnal c++ project.

CMake can be very powerfull but I wanted to find a way to avoid passing days and days to code script and spending time on the build system more than I could on the real project "meat".

The first thing is to create a 'CMakeLists.txt' file in a 'external' folder.

```bash

include(ExternalProject)

set(EXTERNAL_INSTALL_LOCATION ${PROJECT_SOURCE_DIR}/extern)

ExternalProject_Add(
    projectName
    GIT_REPOSITORY https://git/url/to/whatever/you/want.git
    GIT_TAG master
    SOURCE_DIR ${PROJECT_SOURCE_DIR}/extern/temp
    BINARY_DIR ${PROJECT_SOURCE_DIR}/extern/temp
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${EXTERNAL_INSTALL_LOCATION}
    )

```

This code will ask cmake to download from a remote repository 'https://git/url/to/whatever/you/want.git' the tagged version 'master'.
It will download it into external/temp, build it, install it.

The only way, that I found, to force the install to be in a specific folder is to specify the CMAKE_ARGS with the 'CMAKE_INSTALL_PREFIX'.

Once that is done, you are able to configure your project by doing some modification in the CMakeLists.txt of your project.

```bash

add_executable(SomeProject main.cpp)
add_dependencies(SomeProject projectName)

include_directories(${EXTERNAL_INSTALL_LOCATION}/include)
link_directories(${EXTERNAL_INSTALL_LOCATION}/lib)

```

Don't forget the include_directories and link_directories, it will allow your library or application to have include and libs to link with.
