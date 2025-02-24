# COMP52315_GPU_Practicals

Please tackle the tasks yourself first, before looking at the solutions :-)

## Start an interactive session on a GPU on NCC:
Go to the [Learn Ultra page](https://blackboard.durham.ac.uk/ultra/courses/_54359_1/outline) to find out how to connect to NCC via the commandline. 

Once there, execute the following to access a GPU node:

`srun -c 2 --gres=gpu:1 --partition=tpg-gpu-small --pty /bin/bash`

## Compilation of CUDA code on NCC
`module load cuda/12.0`

`nvcc your_source_code.cu -o your_executable`

## Compilation of OpenMP code with CPU and GPU support
`module load nvidia-hpc`

`nvc++ -fopenmp -mp=gpu test.cpp -o test_executable`

### Environment variables to adjust the number of threads used by OpenMP
`OMP_NUM_THREADS`

`OMP_NUM_TEAMS`

`OMP_THREAD_LIMIT`

Pkease note that these are upper bounds and do not guarantee that the ocde is executed with this exact number of threads. For instance, slurm might overwrite these values dependent on the reservation parameters.

## Compilation of SYCL code on NCC
`module load llvm-clang`

`module load cuda/11.5`

`clang++ -fsycl -fsycl-targets=nvptx64-cuda my_source_code.cpp -o my_executable`
