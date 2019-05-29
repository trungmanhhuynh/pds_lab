# Basic Use C/C++ using Sbatch Command on Heracles. 

## Submit job for a simple C/C++ code

1. Write code hello.c 
```
#include<stdio.h>
int main(){
 printf("Hello World\n");
 return 0;
}
```
2. Compile the code
```
>> gcc hello.c -o hello
```
3. Prepare SLURM script c_slurm in the same directory.
```
#!/bin/bash
#SBATCH --partition=interactive-cpu        ### Partition 
#SBATCH --job-name=HiWorld       ### Job Name
#SBATCH --output=Hi.out         ### File in which to store job output
#SBATCH --error=Hi.err          ### File in which to store job error messages
#SBATCH --time=0-00:01:00       ### Wall clock time limit in Days-HH:MM:SS
#SBATCH --nodes=1               ### Node count required for the job, default = 1
#SBATCH --ntasks-per-node=1     ### Nuber of tasks to be launched per Node, default = 1
#SBATCH --exclusive             ### no shared resources within a node
#SBATCH --mail-type=ALL                   ### email alert at start, end and abortion of execution
#SBATCH --mail-user=manh.huynh@ucdenver.edu ### send mail to this address


./hello
```

4. Run SLURM script
```
>> sbatch c_slurm
```
The results will be stored in Hi.out.

## FAQs for C/C++ slurm jobs

**1. What is partition and how do I know which partition I should specify to run my program?**

Partition is group of nodes/resources are configured with pre-defined the limit of resources. Each partition is 
considered as job queue, where all jobs submited into the same partition will be queued. 
To see the list of partitions run command **sinfo**
```
(pytorchenv) [manhh@heracles C]$ sinfo
PARTITION       AVAIL  TIMELIMIT  NODES  STATE NODELIST
month-long-cpu     up 31-00:00:0     16   idle node[2-17]
week-long-cpu      up 7-00:00:00     16   idle node[2-17]
day-long-cpu       up 1-00:00:00     16   idle node[2-17]
short-cpu*         up      30:00     16   idle node[2-17]
interactive-cpu    up 2-00:00:00     16   idle node[2-17]
month-long-gpu     up 31-00:00:0      1   idle node18
week-long-gpu      up 7-00:00:00      1   idle node18
day-long-gpu       up 1-00:00:00      1   idle node18
short-gpu          up      30:00      1   idle node18
interactive-gpu    up 2-00:00:00      1   idle node18
```
Users **MUST** specify one of above partition to run their programs. 






## References
