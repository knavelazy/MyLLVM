# A LLVM-based Compiler Optimizing C/C++ Atomic Accesses

This repo contains the source code of my Thesis Project at TU Delft. It is a modified version of LLVM 14.0.0.

## To Build

Most of the following contents are taken from the [Clang Getting Started](http://clang.llvm.org/get_started.html) page. **Bold words** are my own comments.

This is an example work-flow and configuration to get and build the LLVM source:

1. Get the required tools.
    * See [Getting Started with the LLVM System - Requirements.](https://llvm.org/docs/GettingStarted.html#requirements)
    * Note also that Python is needed for running the test suite. Get it at: https://www.python.org/downloads/
    * Standard build process uses CMake. Get it at: https://cmake.org/download/
      
2. Check out the project:
    * Change the directory to where you want the compiler directory placed.
    * ``git clone https://github.com/knavelazy/MyLLVM.git``

3. Configure and build Clang:
     * ``cd MyLLVM``
  
     * ``mkdir build``
  
     * ``cd build``

     * ``cmake -DLLVM_ENABLE_PROJECTS=clang -G <generator> [options] ../llvm``

        Some common build system generators are:

        * ``Ninja`` --- for generating [Ninja](https://ninja-build.org)
          build files. Most llvm developers use Ninja. **Personally, I recommend using Ninja. Building the whole project for the first time may take more than one hour on my machine. Ninja can be much faster than others.**
        * ``Unix Makefiles`` --- for generating make-compatible parallel makefiles.
        * ``Visual Studio`` --- for generating Visual Studio projects and
          solutions.
        * ``Xcode`` --- for generating Xcode projects.

        Some common options:
  
        * ``-DLLVM_USE_LINKER=lld`` --- Link with the [lld linker](https://lld.llvm.org/), assuming it is installed on your system. This can dramatically speed up link times if the default linker is slow. **This is especially helpful when you have limited RAM. And Ninja runs out of it.**
      
        * ``LLVM_PARALLEL_COMPILE_JOBS:STRING`` and ``LLVM_PARALLEL_LINK_JOBS:STRING`` --- **Constrain the numbers of compile and link jobs in parallel respectively. Set and reduce them when you run out of RAM**

        * ``-DCMAKE_BUILD_TYPE=type`` --- Valid options for *type* are Debug,
          Release, RelWithDebInfo, and MinSizeRel. Default is Debug. **If you are an LLVM developer, use Debug. If you are just building it for use, Release will save much space for you.**

      * ``make``

        **This will build the project. Or use ``ninja -j`` if you use Ninja. ``j`` specifies the number of threads you wish to use.**

      * For more information see [CMake](https://llvm.org/docs/CMake.html)
  
## To Use
Just use the built compiler as a normal clang-14 compiler. Assuming you add ``llvm/build/bin`` to your path, you can try it like:
``clang -O3 file.c``
And to enable our modifications, use ``-mllvm`` parameters like:
``clang -O3 file.c -mllvm -dse-optimize-atomic``
This will allow DSE pass to optimize atomic memory accesses. Similarly, for EarlyCSE pass and InstCombine pass, use ``-mllvm -cse-optimize-atomic`` and ``-mllvm -ic-optimize-atomic``.

Consult the
[Getting Started with LLVM](https://llvm.org/docs/GettingStarted.html#getting-started-with-llvm)
page for detailed information on configuring and compiling LLVM. You can visit
[Directory Layout](https://llvm.org/docs/GettingStarted.html#directory-layout)
to learn about the layout of the source code tree.
