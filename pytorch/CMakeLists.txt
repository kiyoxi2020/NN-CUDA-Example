cmake_minimum_required(VERSION 3.1 FATAL_ERROR)
# modify to your own nvcc path, or delete it if ok
set(CMAKE_CUDA_COMPILER "/usr/local/cuda/bin/nvcc")
project(add2 LANGUAGES CXX CUDA)

find_package(Python REQUIRED) # PATHS "C:/Users/wyq/.conda/envs/torch/python.exe" REQUIRED)
find_package(CUDA REQUIRED)

execute_process(
    COMMAND
        $python -c
            "import torch.utils; print(torch.utils.cmake_prefix_path)"
    OUTPUT_STRIP_TRAILING_WHITESPACE
    OUTPUT_VARIABLE DCMAKE_PREFIX_PATH)

set(CMAKE_PREFIX_PATH "${DCMAKE_PREFIX_PATH}")

find_package(Torch PATHS "C:/Users/wyq/.conda/envs/torch/Lib/site-packages/torch" REQUIRED)
find_library(TORCH_PYTHON_LIBRARY torch_python PATHS "${TORCH_INSTALL_PREFIX}/lib")
set(TORCH_PYTHON_LIBRARY2 "C:/Users/wyq/.conda/envs/torch/libs/python37.lib")
message(STATUS "!!!" "${TORCH_PYTHON_LIBRARY2}")

# modify to your own python path, or delete it if ok
include_directories("C:/Users/wyq/.conda/envs/torch/include")
include_directories(../include)

set(SRCS add2_ops.cpp ../kernel/add2_kernel.cu)
add_library(add2 SHARED ${SRCS})

target_link_libraries(add2 "${TORCH_LIBRARIES}" "${TORCH_PYTHON_LIBRARY}" "${TORCH_PYTHON_LIBRARY2}")