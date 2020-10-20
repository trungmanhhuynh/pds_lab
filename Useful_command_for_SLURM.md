
To re-start/check status slurm service on master node:
```
$ sudo service slurmctld restart/status
```
To kill jobs that are PENDING/RUNNING/SUSPENDED
```
sudo scancel --state=PENDING/RUNNING/SUSPENDED
```
To set state for a node, 
Valid states are: NoResp DRAIN FAIL FUTURE RESUME POWER_DOWN POWER_UP UNDRAIN
Not all states are valid given a node's prior state
```
scontrol update nodename=node001 state=resume
```

To restart the slurmd deamon on the compute node
```
master$ ssh node2
node2$ sudo service slurmd restart
```
If the service fails to restart, with error "PID file /var/run/slurmd/slurmd.pid not readable (yet?) after start" 
Fix: 
```
$ ssh node2
$ sudo cp /var/run/slurmd.pid /var/run/slurmd/
$ sudo service slurmd restart 
```


scancel -u username   # cancel all jobs of username

slurmd -C print the node's current configuration

sinfo

scontrol show jobid -dd <jobid>
  
scontrol show node node18

squeue : list all submited jobs

## Resources: 
- https://docs.computecanada.ca/wiki/Using_GPUs_with_Slurm 
- https://www.rc.fas.harvard.edu/resources/documentation/convenient-slurm-commands/
- https://www.ch.cam.ac.uk/computing/slurm-usage
- https://github.com/accre/SLURM/blob/master/gpu-job/multi-gpu-job.slurm

