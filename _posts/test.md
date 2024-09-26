---
layout: single
title:  "Draft Post"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
categories: 
  - Jekyll
tags:
  - edge case
---
# Switch to LLVM/Clang compiler for CMake (Ubuntu 22.04 aarch64 on Apple Silicon via Multipass)

A package I work with compiles fine on Linux x86, but crashes in a firey hell on my Mac.
```bash
find_package(OpenMP)
if (OPENMP_FOUND
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()
```
Results in:

```bash
CMake Error at CMakeLists.txt:161 (add_executable):
  Target "s_graphs_prefiltering_node" links to target "OpenMP::OpenMP_CXX"
  but the target was not found.  Perhaps a find_package() call is missing for
  an IMPORTED target, or an ALIAS target is missing?
```
Check libomp-dev is installed:
```bash
$ apt show libomp-dev
Package: libomp-dev
Version: 1:14.0-55~exp2
Priority: extra
Section: universe/libdevel
Source: llvm-defaults (0.55~exp2)
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: LLVM Packaging Team <pkg-llvm-team@lists.alioth.debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 16.4 kB
Depends: libomp-14-dev (>= 14~)
Download-Size: 3074 B
APT-Manual-Installed: yes
APT-Sources: http://ports.ubuntu.com/ubuntu-ports jammy/universe arm64 Packages
Description: LLVM OpenMP runtime - dev package
 The runtime is the part of the OpenMP implementation that your code is
 linked against, and that manages the multiple threads in an OpenMP program
 while it is executing.
 This is a dependency package providing the default LLVM OpenMP dev
 package.
 ```

 This was solved by using LLVM/Clang as the compiler for CMake rather than gcc:

