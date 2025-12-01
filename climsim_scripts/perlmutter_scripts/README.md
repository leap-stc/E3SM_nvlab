This folder contains example scripts to launch E3SM-MMF or E3SM-NN jobs on perlmutter.

```example_job_submit_mmf.py``` will launch usual E3SM-MMF jobs.

```example_job_submit_hybrid_pytorch-fortran.py``` will launch E3SM-NN job using the pytorch-fortran library to couple the NN and the E3SM components. To use this version, you must checkout to the main branch of this repo.

```example_job_submit_hybrid_ftorch_withcuda.py``` will launch E3SM-NN job using the ftorch library to couple the NN and the E3SM components. To use this version, you must checkout to the 'ftorch' branch (the current branch) of this repo. Setting ```cb_use_cuda``` to True will use the GPU for the NN inference, which could give a ~4x speedup in model throughputs. ```/pscratch/sd/z/zeyuanhu/share_for_jerry/v2rh_mc_torchscript_wrapper-unet-cuda.ipynb``` is a notebook that demonstrates how to prepare a torchscript model that can be used on GPU.