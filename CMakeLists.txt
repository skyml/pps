
cmake_minimum_required(VERSION 3.0)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(PROJ_ROOT ${CMAKE_CURRENT_SOURCE_DIR})

include(system)
include(simd)

find_package(CUDA QUIET)
find_package(Git REQUIRED)
find_package(Threads REQUIRED)


option(WITH_GPU         "Compile PaddlePaddle with NVIDIA GPU"          OFF)#${CUDA_FOUND})
option(WITH_AVX         "Compile PaddlePaddle with AVX intrinsics"      ${AVX_FOUND})
option(WITH_DSO         "Compile PaddlePaddle with dynamic linked CUDA" ON)
option(WITH_RDMA        "Compile PaddlePaddle with RDMA support"        OFF)
option(WITH_TIMER       "Compile PaddlePaddle with stats timer"         OFF)
option(WITH_PROFILER    "Compile PaddlePaddle with GPU profiler"        OFF)

set(THIRD_PARTY_PATH "${PROJ_ROOT}/third_party" CACHE STRING
  "A path setting third party libraries download & build directories.")

include(external/zlib)      # download, build, install zlib
include(external/gflags)    # download, build, install gflags
include(external/glog)      # download, build, install glog
include(external/gtest)     # download, build, install gtest
include(external/protobuf)  # download, build, install protobuf
include(external/openblas)  # download, build, install openblas

include(generic)            # simplify cmake module
include(cpplint)            # set paddle c++ style
include(ccache)             # set ccache for compilation
include(util)               # set unittest and link libs
include(rdma)               # set rdma libraries
include(cudnn)              # set cudnn libraries
include(flags)              # set paddle compile flags
include(configure)          # add paddle env configuration
include_directories("${PROJ_ROOT}")
include_directories("${PROJ_ROOT}/paddle/cuda/include")
include_directories("${CMAKE_CURRENT_BINARY_DIR}/proto")

set(EXTERNAL_LIBS
    ${GFLAGS_LIBRARIES}
    ${GLOG_LIBRARIES}
    ${CBLAS_LIBRARIES}
    ${PROTOBUF_LIBRARY}
)

add_subdirectory(proto)
add_subdirectory(paddle)
