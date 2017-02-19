cmake_minimum_required(VERSION 3.0)

include(cmake/Download.cmake)
include(cmake/AddExternal.cmake)

project(external-example CXX)

# download an external project
download(
    # destination directory, source code will be in subdirectory `src`
    3rdparty/gflags
    # Standard CMAKE ExternalProject parameters starting from here
    GIT_REPOSITORY https://github.com/gflags/gflags.git
    GIT_TAG v2.2.0
)
# add the external project sandboxed to the current project
add_external(
    # namespace for the targets
    gflags
    # source directory
    3rdparty/gflags/src
    # settings for the build
    BUILD_STATIC_LIBS=ON
    -Wno-dev
)

# build the example
add_executable(example main.cc)
# link to the external library
target_link_libraries(example gflags::gflags_static)