project(pnnx)
cmake_minimum_required(VERSION 3.10)

# c++14 is required for using torch headers
set(CMAKE_CXX_STANDARD 14)

#set(CMAKE_BUILD_TYPE debug)
#set(CMAKE_BUILD_TYPE relwithdebinfo)
#set(CMAKE_BUILD_TYPE release)

option(PNNX_COVERAGE "build for coverage" OFF)

#set(Torch_INSTALL_DIR "/home/nihui/.local/lib/python3.9/site-packages/torch" CACHE STRING "")
#set(Torch_INSTALL_DIR "/home/nihui/osd/pnnx/pytorch-v1.10.0/build/install" CACHE STRING "")
set(Torch_INSTALL_DIR "/home/nihui/osd/pnnx/libtorch" CACHE STRING "")
set(TorchVision_INSTALL_DIR "/home/nihui/osd/vision/build/install" CACHE STRING "")

set(Torch_DIR "${Torch_INSTALL_DIR}/share/cmake/Torch")
set(TorchVision_DIR "${TorchVision_INSTALL_DIR}/share/cmake/TorchVision")

find_package(Torch REQUIRED)
find_package(TorchVision QUIET)

message(STATUS "Torch_VERSION = ${Torch_VERSION}")
message(STATUS "Torch_VERSION_MAJOR = ${Torch_VERSION_MAJOR}")
message(STATUS "Torch_VERSION_MINOR = ${Torch_VERSION_MINOR}")
message(STATUS "Torch_VERSION_PATCH = ${Torch_VERSION_PATCH}")

if(Torch_VERSION VERSION_LESS "1.8")
    message(FATAL_ERROR "pnnx only supports PyTorch >= 1.8")
endif()

if(TorchVision_FOUND)
    message(STATUS "Building with TorchVision")
else()
    message(WARNING "Building without TorchVision")
endif()

include_directories(${TORCH_INCLUDE_DIRS})

add_subdirectory(src)

enable_testing()
add_subdirectory(tests)
