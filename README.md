# cmake_external

A better way to integrate 3rd party projects into CMAKE. It downloads external projects, and includes them in the project in a sandboxed way. Only needed targets are built.

This repository is an example project, it can be compiled like normal CMake projects:
```sh
> mkdir build && cd build
> cmake -GNinja ..
> ninja
``` 
Note: The use of the `ninja` build system is optional, `Make` works fine too

## download(destination, parameters)

Downloads a project from an online source. Supports everything that is supported by CMake's `ExternalProjectAdd`: Git, Mercurial, SVN, CVS and various archives. The download happens during the CMake generation step.

### Example:
```cmake
download(
    # destination directory, source code will be in subdirectory `src`
    3rdparty/gflags
    # Standard CMAKE ExternalProject parameters starting from here
    GIT_REPOSITORY https://github.com/gflags/gflags.git
    GIT_TAG v2.2.0
)
```

## add_external(namespace, source, parameters)

Adds an external CMake project to the current project, not unsimiliar to `add_subdirectory`. But the project is generated in an isolated way: The project doesn't need support for being compiled in a subdirectory, and no variables, settings or targets can conflict with the main project. The external project's targets are imported into the main project with a namespace, they are built only if needed by the main project.

### Example:
```cmake
add_external(
    # namespace for the targets
    gflags
    # source directory
    3rdparty/gflags/src
    # settings for the build
    BUILD_STATIC_LIBS=ON
    -Wno-dev
)
```