# General guidelines for using HPC
## Terms of Use
Terms of use are available online here: [Terms of Use](https://hpc.rtu.lv/hpc/hpc-services/using-hpc/?lang=en)    

## Best practice
If possible, avoid the execution of jobs locally on the login nodes. Use it only to submit a job to a queue, to compile programs, and test short jobs.  

Try to indicate the necessary walltime (`qsub -l walltime=01:00:00`) so that other users can forecast their queue time.  

The maximum execution time of one job (simulation) is two weeks. After this period, the job will be automatically cancelled. If more time is needed for a job, the user can make a separate agreement with HPC Centre.  

If a job requires intensive disk operation (I/O), it is advised to use a local SSD disk instead of the network directory (`/home` or `/home_beegfs`). The local disk is mounted to the directory /scratch. When the job is completed, the files created in this directory should be removed by a user manually.  

Evaluate the performance of parallel (MPI) jobs by using a various number of nodes/cores to select the most suitable one for your job. A larger number of cores does not always ensure faster execution of a job.  

## Technical Support
Technical support is provided to all HPC cluster users. The support includes assistance in preparing jobs, installing application software, briefing on the work with the cluster, consulting.  

In some areas (such as computational chemistry), we can also provide scientific support by helping to choose the most appropriate simulation tool for your problem or to create a simulation model.  
- HPC user support e-mail: hpc@rtu.lv.
- Security incidents should be reported to e-mail: hpc@rtu.lv.
