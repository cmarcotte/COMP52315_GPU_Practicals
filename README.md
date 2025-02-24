# COMP52315: GPU Computing Practicals

Please tackle the tasks yourself first, before looking at the solutions :-)

## Start an interactive session on a GPU on NCC:
Go to the [Learn Ultra page](https://blackboard.durham.ac.uk/ultra/courses/_54359_1/outline) to find out how to connect to NCC via the commandline. 

Once logged in to NCC, execute the following to access a GPU node (live), with immediate `bash` access:

`srun -c 1 --gres=gpu:pascal:1 --partition=tpg-gpu-small --pty /bin/bash`

**Note**: You may need to wait for live access to a GPU node due to a significant backlog of jobs on NCC[^1]. Choosing `gpu:pascal` (instead of `gpu:turing` or `gpu:ampere`) should get your job running much sooner, as the older hardware is underutilized.

## Significant Differences from Hamilton:
NCC uses the same scheduler and batch submission system as Hamilton, which you are familiar with. There are two significant differences which will affect how you use NCC compared to Hamilton:
1. Hamilton hardware is uniform; i.e. the login nodes have the same hardware as the compute nodes, and thus compiling and testing your code on the login node is viable. On NCC, the login nodes do not provide GPU compute access _at all_. If you attempt to run GPU code on the login node, it will fail. Further, NCC is much more strict about running tests on the login node compared to Hamilton -- if you run CPU-code on the login node, you are likely to face consequences.
2. [NCC additionally specifies a *Q*uality *O*f *S*ervice (QoS) parameter to the batch script](https://ncc1.clients.dur.ac.uk/#running-jobs). It is recommended you use the `debug` QoS when debugging, as this is typically sufficient for the work we will do in this module.

## Compiler Modules in use for this module:

This module will introduce CUDA, OpenMP, and SyCL programming models for GPU computation. For each, you will need a distinct stack of software for compilation. On NCC, this is handled using modules.

### Compilation of CUDA code on NCC
```
module load cuda/12.0
nvcc your_source_code.cu -o your_executable
```

### Compilation of OpenMP code with CPU and GPU support
```
module load nvidia-hpc
nvc++ -fopenmp -mp=gpu test.cpp -o test_executable
```

#### Environment variables to adjust the number of threads used by OpenMP
```
OMP_NUM_THREADS
OMP_NUM_TEAMS
OMP_THREAD_LIMIT
```

**Note**: These are upper bounds and do not guarantee that the code is executed with this exact number of threads. For instance, slurm might overwrite these values dependent on the reservation parameters.

### Compilation of SYCL code on NCC
```
module load llvm-clang/15.0.0
module load cuda/11.5
clang++ -fsycl -fsycl-targets=nvptx64-cuda my_source_code.cpp -o my_executable
```
**Note**: The specific versions for the `clang` and `cuda` modules.

[^1]: And a glut of students using the cluster ineffectively due to a lack of understanding that "gpu ≠ fast".
