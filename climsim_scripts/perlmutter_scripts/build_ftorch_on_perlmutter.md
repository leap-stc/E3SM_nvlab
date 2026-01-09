# Building FTorch on Perlmutter

Here I documented how to install fTorch on Perlmutter.

First git clone the fTorch repository:
```
git clone git@github.com:Cambridge-ICCS/FTorch.git
```

Then, nevigate to the FTorch directory:
```
cd FTorch/
```

Then, do the build process:
```
mkdir build
cd build
module load cudatoolkit/11.7
cmake .. \
    -DCMAKE_C_COMPILER=gcc \
    -DCMAKE_CXX_COMPILER=g++ \
    -DCMAKE_Fortran_COMPILER=gfortran \
    -DCMAKE_PREFIX_PATH=/global/common/software/nersc9/pytorch/2.0.1/lib/python3.9/site-packages/torch \
    -DCMAKE_INSTALL_PREFIX=../install/ \
    -DCMAKE_BUILD_TYPE=Release \
    -DENABLE_CUDA=TRUE \
    -DCUDA_curand_LIBRARY=/opt/nvidia/hpc_sdk/Linux_x86_64/22.7/math_libs/11.7/targets/x86_64-linux/lib/libcurand.so \
    -DCUDA_cufft_LIBRARY=/opt/nvidia/hpc_sdk/Linux_x86_64/22.7/math_libs/11.7/targets/x86_64-linux/lib/libcufft.so \
    -DCUDA_cublas_LIBRARY=/opt/nvidia/hpc_sdk/Linux_x86_64/22.7/math_libs/11.7/targets/x86_64-linux/lib/libcublas.so \
    -DCUDA_cublasLt_LIBRARY=/opt/nvidia/hpc_sdk/Linux_x86_64/22.7/math_libs/11.7/targets/x86_64-linux/lib/libcublasLt.so
```

Then, 
```
cmake --build . --target install
```

This should install FTorch in the `FTorch/src/install/` directory.
