---
title: Install # will be overwritten by seo.title below
date: 2022-01-19
layout: page
seo:
  title: Installing OpenCilk v1.1
eleventyNavigation:
  key: main
  title: Install # as it will appear in the nav
  order: 1 # order to display in the nav
---

## Requirements

OpenCilk v1.1 has been tested on Ubuntu (18.04, 20.04), FreeBSD (13), Fedora (32), and macOS (10.15, 11.2). It runs on Intel x86 64-bit processors (Haswell and newer), AMD x86 64-bit processors (Excavator and newer), and Apple M1 and other 64-bit ARM processors.

## Download


| Platform            |   Download        |
|---------------------|-------------------|
| Linux               | blah blah         |
| macOS               | blah b            |

## Install

There are binary releases for two common systems (Linux/Ubuntu 18 and
Mac OS) at
link:https://github.com/OpenCilk/opencilk-project/releases/tag/opencilk/beta3\[OpenCilk
Binary Releases\]

A docker image is now available as well.

## Building OpenCilk from source

If you want to build from source, our welcome message and build
instructions are here:

  - link: https://github.com/OpenCilk/infrastructure/blob/release/README
    \[OpenCilk Build Instructions\]
  - link:
    https://github.com/OpenCilk/infrastructure/blob/release/INSTALLING
    \[OpenCilk Installation Instructions\]

Precompiled binaries for OpenCilk 1.0 are available for some systems
here:
https://github.com/OpenCilk/opencilk-project/releases/tag/opencilk%2Fv1.0.
To install, either download and run the appropriate shell archive (i.e.,
\`.sh\` file) or unpack the appropriate tarball.

A docker image for Ubuntu 18.04 with OpenCilk 1.0 installed is available
here:
https://github.com/OpenCilk/opencilk-project/releases/download/opencilk%2Fv1.0/docker-opencilk-v1.0.tar.gz.
Some documentation on how to use the docker image can be found here:
\[docker\](docker).

For other systems, instructions for downloading and building OpenCilk
from source can be found here: \[INSTALLING.md\](INSTALLING.md).

## Instructions for building OpenCilk from source

OpenCilk is available as source code in three Git repositories, plus
this infrastructure facilities repository. OpenCilk 1.0 is only
guaranteed to support 64-bit x86 on Linux and other Unix-like operating
systems, although prototype support for 64-bit ARM is included.

### Requirements

The build requirements for OpenCilk are largely consistent with those
for LLVM. In summary, to build OpenCilk on a modern system running Linux
or MacOSX, you will need the following:

  - A relatively recent version of Git.
  - A relatively modern C/C++ compiler, such as GCC or Clang, that is
    capable of building LLVM. Any compiler you are likely to have
    installed on a modern multicore system should work.
  - CMake version 3.9 or newer.
  - Approximately 1.5 GB of space for the source code (mostly LLVM) plus
    2.5 GB of space to build, for a total of 4 GB.

More details on build requirements for LLVM can be found here:
\<https://llvm.org/docs/GettingStarted.html\#requirements\>

### Quick start

Clone the OpenCilk infrastructure repository:

git clone -b opencilk/v1.0 https://github.com/OpenCilk/infrastructure

Run the following script to get the OpenCilk source code:

infrastructure/tools/get $(pwd)/opencilk

Then run the following script to build OpenCilk:

infrastructure/tools/build $(pwd)/opencilk $(pwd)/build

You should now be ready to use OpenCilk. Skip to
\[Usage\](INSTALLING.md\#Usage) now, or read on for more explicit
directions on building OpenCilk from source.

### Obtaining the OpenCilk source code

Clone the OpenCilk compiler, runtime, and productivity tool
repositories. The Cheetah runtime and OpenCilk tool repositories must be
cloned into sub-directories of the OpenCilk project directory:

git clone -b opencilk/v1.0 https://github.com/OpenCilk/opencilk-project

git clone -b opencilk/v1.0 https://github.com/OpenCilk/cheetah
opencilk-project/cheetah

git clone -b opencilk/v1.0
https://github.com/OpenCilk/productivity-tools
opencilk-project/cilktools

Note that, because these commands clone specific tags of the OpenCilk
repositories, it is normal for Git to report that each clone is in a
'detached HEAD' state after cloning.

Clone the OpenCilk infrastructure repository, which contains the
OpenCilk build script:

git clone -b opencilk/v1.0 https://github.com/OpenCilk/infrastructure

### Building OpenCilk

Run the \`infrastructure/tools/build\` script with two or three
arguments. The 1st argument is the absolute pathname to the
\`opencilk-project\` repository directory. The 2nd argument is the
absolute pathname of a directory to build OpenCilk. The 3rd argument, if
present, tells the build system how many parallel jobs to run. Default
parallelism is equal to the number of logical cores, or 10 if the number
of cores is not detected.

For example:

\# ...git clone as above...

infrastructure/tools/build $(pwd)/opencilk-project $(pwd)/build

Alternatively, to explicitly build OpenCilk using 8 build threads:

\# ...git clone as above...

infrastructure/tools/build $(pwd)/opencilk-project $(pwd)/build 8

OpenCilk takes a few CPU-hours to build on a modern system --- less than
10 minutes on a 24-core Ryzen with a fast disk. It might take all day
single-threaded on an older machine.

To echo the OpenCilk build script call syntax, use the \`--help\`
switch:

infrastructure/tools/build --help

### \*\*\*Advanced build options:\*\*\* 

If you wish, you can customize your build of OpenCilk beyond what the
script provides --- e.g., to build additional LLVM subprojects --- by
running the necessary Cmake commands yourself. When run, the
\`infrastructure/tools/build\` script will print out the \`cmake\`
commands it runs to build OpenCilk from source. OpenCilk supports many
of the same CMake build options as standard LLVM, which are documented
here: \<https://llvm.org/docs/CMake.html\>. If you wish to customize
your OpenCilk build with these options, we recommend keeping \`clang\`
in the list passed to \`-DLLVM\_ENABLE\_PROJECTS\` and
\`cheetah;cilktools\` in the list passed to
\`-DLLVM\_ENABLE\_RUNTIMES\`.

### Usage

You can run the OpenCilk C compiler out of its build tree, adding
\`/bin/clang\` to the build directory name. Similarly, add
\`/bin/clang++\` for the OpenCilk C++ compiler.

Running on x86, you must have a chip with Intel's Advanced Vector
Instructions (AVX). This includes Sandy Bridge and newer Intel
processors (released starting in 2011), and Steamroller and newer AMD
processors (released starting in 2014).

OpenCilk should work on any 64-bit ARM via its experimental ARM support.
In particular, OpenCilk has been tested on Apple's M1. It may be helpful
to try different values of the CILK\_NWORKERS environment variable on
chips like the M1 that mix low- and high-power cores.

On MacOSX, you will need an XCode or CommandLineTools installation to
provide standard system libraries and header files for clang. To run
clang with those header files and libraries, invoke the clang binary
with \`xcrun\`; for example:

xcrun $(pwd)/build/bin/clang

### Optional: Installing OpenCilk

You can install OpenCilk into a directory of your choosing by running
the \`cmake\_install.cmake\` script generated in the build directory.
For example, run the following to install OpenCilk into the directory
\`/tmp/llvm\`:

cd $(pwd)/build

cmake -DCMAKE\_INSTALL\_PREFIX=/tmp/llvm -P cmake\_install.cmake

### Troubleshooting

Here are a few common problems encountered when building from source,
along with suggested workarounds.

\> The build fails with the error message, \`collect2: error: ld
returned 1 exit status\`.

This error typically occurs when the build process exhausts the physical
memory available on the system. Building OpenCilk from source with many
parallel build threads can consume a large amount of physical memory,
roughly speaking, in the tens of gigabytes.

\*\*Fix:\*\* Try reducing the number of parallel threads for building
OpenCilk. Alternatively, try building OpenCilk from source using
\`clang\` and LLVM's linker, \`lld\`, which tends to consume less
physical memory than \`ld\`.

\> The build fails with the error message, \`unrecognized argument to
'-fno-sanitize=' option: 'safe-stack'\`.

This error typically occurs when the C and C++ compilers on the system
are mismatched, e.g., if \`gcc\` and \`g++\` refer to different compiler
versions on the system.

\*\*Fix:\*\* Make sure that the versions of \`gcc\` and \`g++\`
installed on the system are consistent.
