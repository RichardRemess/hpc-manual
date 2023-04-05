# Running containers in the cluster
[Singularity](https://www.sylabs.io/singularity/) is a containerization solution designed for high-performance computing cluster environments. Besides the native singularity containers it can run also docker containers.

Accessing Singularity:
```
module load singularity
module list
```
Main online container registries and commands to load
||||
| --- |--- | --- |
| Singularity Library |	[https://cloud.sylabs.io/library](https://cloud.sylabs.io/library) | $ singularity pull library:// |
| Docker Hub |	[https://cloud.sylabs.io/library ](https://hub.docker.com)| $ singularity pull docker:// |
| Singularity Hub |	[https://singularity-hub.org](https://singularity-hub.org) | $ singularity pull shub:// |
| NVIDIA GPU Cloud |	https://ngc.nvidia.com | $ singularity pull docker://nvcr.io/ |
|Biocontainers| [https://biocontainers.pro/#/registry](https://biocontainers.pro/#/registry) | |

Examples:
```
singularity pull docker://gcc:5.3.0
singularity pull library://godlovedc/demo/lolcow
```
Containers execution on the HPC cluster. Start interactive session in the fast queue:
```
qsub -I -X  -q fast
```
or start interactive session on 1 node with Tesla K40m GPU, wall-time 1hr in “fast” queue
```
qsub -I -l nodes=1:gpus=1,feature=K40,walltime=01:00:00 -q fast   
cd $PBS_O_WORKDIR			# change to working directory
module load singularity		# Load Singularity module
```
Run command executes a defined set of commands from a definition file's runscript. It is only available when using an image that was built from a definition file that specified a runscript.
```
# Running  container’s runscript from library
singularity run library://godlovedc/demo/lolcow	 
# Execute runscript of loaded container
singularity run  ./lolcow_latest.sif
```
Exec command executes command from container:
```
singularity exec  ./lolcow_latest.sif  fortune   
singularity exec  library://godlovedc/demo/lolcow  fortune
```
Shell command allows you to spawn a new shell within your container and interact with it as though it were a small virtual machine
```
singularity shell library://godlovedc/demo/lolcow
singularity shell container.sif
```
User-defined bind paths:
```
singularity shell --bind /scratch  container.sif
```
GPU use:  If your host system has an NVIDIA GPU card and a driver installed, you can leverage the card with the --nv option : 
```
singularity run --nv  container.sif
```
Example using Tensorflow Deep Leraning Framwork from Docker container Library. Full output has been omitted from the example for brevity.
```
singularity pull docker://tensorflow/tensorflow:2.3.1-gpu
qsub -I -l nodes=1:ppn=4:gpus=1 
sub: waiting for job 1004934.rudens to start
qsub: job 1004934.rudens ready
module load singularity 
cd $PBS_O_WORKDIR
@wn58 tensorflow]$ singularity exec --nv tensorflow_2.3.1-gpu.sif \
 python -c 'import tensorflow as tf; print(tf.__version__)'
2.3.1
@wn58 tensorflow]$ singularity exec --nv tensorflow_latest-gpu.sif \
python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
…
Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 10610 MB memory) -> physical GPU (device: 0, name: Tesla K40m, pci bus id: 0000:04:00.0, compute capability: 3.5)
tf.Tensor(654.7006, shape=(), dtype=float32)
@wn58 tensorflow]$ singularity shell --nv tensorflow_2.3.1-gpu.sif  
Singularity tensorflow_2.3.1-gpu.sif:~> python /usr/local/lib/python3.6/dist-packages/tensorflow/python/debug/examples/debug_mnist.py
Accuracy at step 0: 0.216
Accuracy at step 1: 0.098
```
For Future reading:  [Singularity documentation](https://www.sylabs.io/docs/), [User Guide](https://sylabs.io/guides/3.2/user-guide/), [Quick Start](https://sylabs.io/guides/3.2/user-guide/quick_start.html)
