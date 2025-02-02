[![Build Status](https://travis-ci.org/hoangmle/metaSMT.svg?branch=qf_abv)](https://travis-ci.org/hoangmle/metaSMT)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/8696/badge.svg)](https://scan.coverity.com/projects/hoangmle-metasmt)

metaSMT is a wrapper around various SMT or SMT-like solvers.
This means you are able to program against this library and use any
of the implemented solvers.

The implementation here is a slimmed-down version of original metaSMT (https://github.com/agra-uni-bremen/metaSMT).

# Building metaSMT

0. Preparations
1. Install solver backends
2. Configure metaSMT
3. Build and test


# 0. Preparation 

You need: a C/C++ Compiler (tested: *gcc* or recent *clang*), *make*, *cmake*, *wget*,
most of them you can install from your package manager.

If you want to use the SMT-LIB2 file backend, please ensure that you have
installed the *zlib* and *bzip2* development packages (*zlib1g-*dev, *libbz2-dev* on
Debian-based systems).  Otherwise boost will be build without *iostreams* and you
will get linking errors. This is not required to use metaSMT in general.

metaSMT heavily relies on different dependencies (e.g. the different solving
engines). For a succesfull build, a folder named 'dependencies' containing them
is expected to be present in the metaSMT main folder.  You can either freshly
clone the repository from https://github.com/agra-uni-bremen/dependencies.git
or symlink to an existing dependencies folder. The folder name 'dependencies' is
fixed and cannot be changed.

Alternatively if only dependencies is installed and can be found with cmake find_package,
no "dependencies" submodule is needed.<br/>
Here is what I have done in my environment:<br/>
. install Boost in different directory and set environment BOOST_ROOT to point it:<br/>
  ```bash
    export BOOST_ROOT=\<Boost installation directory\> <br/>
  ```
. install Z3 (up to version 4.8.7 verified), and install in default directory, or specified
  directory. In later case, set Z3_DIR environmen to point to the installation direction
  wit file Z3Config.cmake

## 1. The `bootstrap.sh` script

The `bootstrap.sh` script provided by metaSMT can automatically download and
install dependencies needed by metaSMT and setup a metaSMT build folder. You
may require subversion, or git to be installed.

Calling `./bootstrap.sh` without arguments will display a complete list of
options. It is recommended to call

 ```
./bootstrap.sh --deps deps/ --free     -m RELEASE build/
```

or

```
./bootstrap.sh --deps deps/ --non-free -m RELEASE build/
```

to download and build all dependencies in the "deps" folder and configure a
metaSMT build in the "build" folder. The source folder can be shared for
different build folders. The parameters --free and --non-free select which
set of backend tools to download and include in the build, you need to select
which you want.

Please note that some free and non-free backends use licenses which
are not compatible (e.g. Boolector: GPL, SWORD: Proprietary).
Check the compatibility of the selected backends with your own projects
license.

If your system does not provide a recent version of *cmake*, *bootstrap.sh* can
build a local version when provided with `--cmake`.

## 2. Build metaSMT

An already configured `build` folder can be build using

```
   cd build
   make
```

and installed (optional, only when used as external library) using

  ```make install```

If no install folder is given to bootstrap.sh by default metaSMT is installed
to the build/root/ folder.

Please also note that metaSMT by default compiles an excessive test suite for
all available backends. After building it can be called with `make test`.
Note that some Tests are expected to fail, e.g. due to timeouts, backend
restrictions or work in progress (new) functionality.


Building the test suite can take a lot of time. If you want to disable
building the tests you can  pass `-DmetaSMT_ENABLE_TESTS=off` to the bootstrap
script or call

  ```cmake ./ -DmetaSMT_ENABLE_TESTS=off```

in the build folder or by setting the option using `ccmake .` or
`cmake-gui .`.

## 3. Using metaSMT

To use metaSMT with your (already existing) project you need to tell `cmake` that it should 
be looking for metaSMT. To do so, edit the main `CMakeLists.txt` file and add the line

    find_package( metaSMT REQUIRED )

You still have to tell `cmake` where to look for metaSMT by setting the variable 
`CMAKE_PREFIX_PATH`, either by

    export CMAKE_PREFIX_PATH=/path/to/metaSMT/install
    cmake [...]

or

    cmake -DCMAKE_PREFIX_PATH=/path/to/metaSMT/install [...]

## 4. Documentation

In addition to the examples, the API documentation is located in the doc/html/
folder. Simply open the doc/html/index.html file in your favorite browser.
Note: The documentation is not yet complete. Only selected modules are
currently documented.

