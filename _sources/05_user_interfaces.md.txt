# User Interfaces

There are several options users may take to work on the cluster:
- command line (remote terminal) interface
- graphical interface of application software (GUI)
- web interface

## Command line
That is a traditional manner of accessing the cluster. A user uses the Internet to connect to a remote terminal (through a secure shell (SSH) protocol usually) where commands can be executed in a text mode. It is possible to edit and compile a code, place a job (simulation) in the queue, and monitor the job's course of execution. Graphical tools can also be called from a command line. Instructions in this guide are mainly intended for command-line access.
 
![](images/command_line.png)
  
## GUI of Application Software
Some software packages (like Schrodinger, MATLAB, etc.) have in-built support for parallel computing on a cluster. A user can either perform a simulation locally or on a cluster, without leaving the regular software environment. In case of such software like Schrodinger, the cluster can be used by opening the graphical interface either locally on the personal computer or remotely on the cluster login node.
 
![](images/schrodinger.png)

## Web interface
Access to the cluster using an HTTPS protocol by opening a graphical interface (portal) on an Internet browser.

### Moab Viewpoint 
Viewpoint is a graphical interface for submitting and managing jobs in a similar manner as in command line but the graphical environment. Access to this interference should be asked by contacting hpc@rtu.lv.
 
![](images/viewpoint.png)
 
### Galaxy

```{include} ./includes/galaxy.md
```
