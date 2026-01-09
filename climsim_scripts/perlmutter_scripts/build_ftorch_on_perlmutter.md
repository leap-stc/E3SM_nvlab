# Building FTorch on Perlmutter

This document describes how to install FTorch on Perlmutter for use with
PyTorch 2.8.0 and Python 3.12.

## Prerequisites

- CMake >= 3.15
- GCC compilers (gcc, g++, gfortran)
- CUDA-enabled PyTorch installation

## Step 1: Clone the FTorch Repository

```bash
git clone git@github.com:Cambridge-ICCS/FTorch.git
cd FTorch/
```

## Step 2: Verify PyTorch Path (Important!)

Before building, verify the PyTorch installation path on Perlmutter. The path
may change as NERSC updates their software stack:

```bash
module load pytorch
python -c "import torch; print(torch.__path__[0])"
```

This will print the path to use for `CMAKE_PREFIX_PATH`. As of this writing,
the expected path is:

```
/global/common/software/nersc9/pytorch/2.8.0/lib/python3.12/site-packages/torch
```

## Step 3: Build FTorch

Load the required modules and run CMake:

```bash
mkdir build
cd build

# Load pytorch module (automatically loads compatible cudatoolkit)
module load pytorch

# Configure the build (update CMAKE_PREFIX_PATH based on Step 2 output)
cmake .. \
    -DCMAKE_C_COMPILER=gcc \
    -DCMAKE_CXX_COMPILER=g++ \
    -DCMAKE_Fortran_COMPILER=gfortran \
    -DCMAKE_PREFIX_PATH=/global/common/software/nersc9/pytorch/2.8.0/lib/python3.12/site-packages/torch \
    -DCMAKE_INSTALL_PREFIX=../install/ \
    -DCMAKE_BUILD_TYPE=Release \
    -DGPU_DEVICE=CUDA
```

Then build and install:

```bash
cmake --build . --target install
```

This will install FTorch in the `FTorch/install/` directory.

## Changes from Previous Versions

### PyTorch 2.8.0 / Python 3.12 Updates

- **PyTorch path**: Updated from `pytorch/2.0.1/lib/python3.9/...` to
  `pytorch/2.8.0/lib/python3.12/...`
- **CUDA toolkit**: Updated from `cudatoolkit/11.7` to `cudatoolkit/12.9`
  (loaded automatically by the pytorch module)
- **Security**: PyTorch >= 2.6.0 addresses a vulnerability in earlier versions
  that could allow remote code execution when loading untrusted model files

### FTorch v1.0.0 Updates

The FTorch library (v1.0.0, released March 2025) has simplified the build
process:

- **GPU configuration**: The `-DENABLE_CUDA=TRUE` flag has been replaced with
  `-DGPU_DEVICE=CUDA`. This new option also supports other GPU backends:
  - `NONE` (default) - CPU only
  - `CUDA` - NVIDIA GPUs
  - `HIP` - AMD/ROCm
  - `XPU` - Intel
  - `MPS` - Apple Silicon

- **CUDA libraries**: Individual CUDA library paths are no longer required.
  The old flags (`-DCUDA_curand_LIBRARY`, `-DCUDA_cufft_LIBRARY`,
  `-DCUDA_cublas_LIBRARY`, `-DCUDA_cublasLt_LIBRARY`) have been removed as
  FTorch now handles CUDA library discovery automatically through the
  `GPU_DEVICE` option.

## Troubleshooting

If the build fails, check the following:

1. **PyTorch path**: Verify the path exists using `ls` and matches your
   `CMAKE_PREFIX_PATH`
2. **Module compatibility**: Ensure `cudatoolkit` version is compatible with
   the PyTorch installation
3. **CMake version**: Run `cmake --version` to verify CMake >= 3.15

To check available PyTorch versions on Perlmutter:

```bash
module avail pytorch
```

## References

- [FTorch Documentation](https://cambridge-iccs.github.io/FTorch/)
- [FTorch GitHub Repository](https://github.com/Cambridge-ICCS/FTorch)
- [NERSC PyTorch Documentation](https://docs.nersc.gov/machinelearning/pytorch/)
