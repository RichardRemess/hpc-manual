# Using GPUs and CUDA on the cluster
This section describes best practices for using GPUs and CUDA framework on the HPC cluster.
## Requesting GPUs for a job
GPUs can be requested in a similar way as other cluster resources.
```
qsub -l nodes=1:ppn=1:gpus=1:shared
```
Additionally, it is possible to specify compute mode for a GPU: 
- `shared`: GPU available for multiple processes 
- `exclusive_process`: only one compute process is allowed to run on the GPU
To request specific GPU model additionally specify the feature parameter: `-l feature=v100`

GPUs available on the cluster “Rudens”:
| GPU model | Arch | CUDA  | FP64 power |	Tensor power |	Memory | NVLink| qsub feature | 
| --------- | ---- | ----- | ---------- | ------------ | ------- | ------|------------- |
| Tesla K40 |	Kepler | 3.5 | 1.3 Tflops |	N/A |	12 GB |	N/A | k40 |
| Tesla V100 |	Volta |	7.0 |	7.5 Tflops |	112  Tflops |	16 GB | N/A |	v100 |
| A100 | Ampere | 8.0 | 9.7 Tflops | 312 Tflops | 40 GB | Bridge 600 GB/s | a100 |

For more details, please see the section “HPC hardware specifications”.

## Development using CUDA Toolkit
CUDA (Compute Unified Device Architecture) is a parallel computing platform and application programming interface (API) model created by NVIDIA. It allows software developers to use a CUDA-enabled graphics processing unit (GPU) for general purpose processing, an approach known as General Purpose GPU (GPGPU) computing. Find more information on the availability of GPUs in the previous section.
### Preparing the Working Environment
You need to load a version of the CUDA library (and compiler). Several versions of the CUDA library are available, list them with `module avail cuda` command. Different versions of CUDA are activated via the ‘module load’ command:
```
module load cuda/cuda-<version>
```

```
module load cuda
```

 CUDA tools/libraries installed on the cluster:
- C++ extensions for CUDA programming
- CuDNN library for neural networks
- CuBLAS
- NCCL

### Usage examples for the cluster
Job examples available here: `/opt/exp_soft/user_info/cuda`.
The following Linux-related graphical tools can be used for programming and debugging: 
`emacs` and `ddd`.
```
module load cuda
emacs hello-world.cu
nvcc -g -G hello-world.cu –o hello-world.out
ddd –debugger cuda-gdb hello-world.out
```
NVIDIA Nsight, a development environment based on Eclipse and development by NVIDIA, can be used.
```
module load cuda
nsight
```
Note. To be able to use GUI tools, X11 forwarding must be performed to provide a remote graphical interface. 

### GPU Code Generation Options
To achieve the best possible performance whilst being portable, GPU code should be generated for the architecture(s) it will be executed upon.

That is controlled by specifying `-gencode` arguments to NVCC which, unlike the `-arch` and `-code` arguments, allows for ‘fatbinary’ executables that are optimised for multiple device architectures.

Each `-gencode` argument requires two values, the virtual architecture and real architecture, for use in NVCC’s two-stage compilation. I.e. `-gencode=arch=compute_60, code=sm_60` specifies a virtual architecture of compute_60 and real architecture `sm_60`.

The minimum specified virtual architecture must be less than or equal to the GPU's Compute Capability used to execute the code.
To build a CUDA application which targets any GPU on HPC cluster “Rudens”, use the following -gencode arguments (for CUDA 8.0):
```
nvcc filename.cu \
   -gencode=arch=compute_35,code=sm_35 \
```

