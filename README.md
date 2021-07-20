# Hipify-torch

hipify-torch is a python utility to convert CUDA C/C++ code into HIP C/C++ code.
It is NOT a parser; it does a smart but basic search-and-replace based on CUDA-to-HIP mappings which are specified in the hipify-torch module.
It can also "hipify" the header include statements in your source code to ensure that it's the hipified header files that are included.

<!-- toc -->

- [Usage examples](#usage-examples)
  - [Through cmake](#through-cmake)
  - [Through python](#through-python)
- [Utilities](#utilities)
  - [CMake utility function](#cmake-utility-function)
- [Intended users](#intended-users)

<!-- tocstop -->

# Usage examples

## Through cmake

From the parent `CMakeLists.txt` file include the `Hipify.cmake` file from `./cmake/Hipify.cmake`

```
list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/hipify-torch/cmake")
include(Hipify)
```
Note: Update the path if the hipify-torch repo is cloned into different directory.

Above lines trigger the hipify script for all sources & header files under the `PROJECT_SOURCE_DIR`

## Through python

Please `import hipify` present in `hipify_python.py` at `./hipify/`

# Utilities

## CMake utility function

### API function ***get_hipified_list()***

This utility function can be used to get a list of hipified files from a list of cuda files.

```
function(get_hipified_list INPUT_LIST OUTPUT_LIST FILE_SUFFIX)
```
- `INPUT_LIST` - CMake list containing a list of cuda file names
- `OUTPUT_LIST` - Cmake list containing a list of hipified files names. If the cuda file name is not changed after hipify, then it is NOT replaced in the list.
- `FILE_SUFFIX` - A string value, which is used as file name suffix during the execution. Provide a unique string for each call of `get_hipified_list()`.

#### Usage example

```
get_hipified_list("${TP_CUDA_SRCS}" TP_CUDA_SRCS "TP_CUDA_SRCS")
```

Here the `TP_CUDA_SRCS` in the input list containing cuda source files and doing a inplace update with  output list `TP_CUDA_SRCS`
For the file suffix unique string, list variable name itself is passed as a string.

# Intended users

This module can be used to build [PyTorch](https://github.com/pytorch/pytorch), as well as PyTorch CUDA extensions such as [torchvision](https://github.com/pytorch/vision), [detectron2](https://github.com/facebookresearch/detectron2) etc.
Even PyTorch submodules such as [tensorpipe](https://github.com/pytorch/tensorpipe), etc.
