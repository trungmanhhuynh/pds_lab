# Troubleshooting Heracles

#### Error 1: 
When running mpi codes with slurm job. We encountered this errors when using many nodes. It works fine for small number of nodes.
This happened when running mpi in Lab 1 and Lab 4 of Parallel Class.  

```
[mpiexec@node2] check_exit_codes (../../../../../src/pm/i_hydra/libhydra/demux/hydra_demux_poll.c:114): unable to run proxy on node2 (pid 809)
[mpiexec@node2] poll_for_event (../../../../../src/pm/i_hydra/libhydra/demux/hydra_demux_poll.c:152): check exit codes error
[mpiexec@node2] HYD_dmx_poll_wait_for_proxy_event (../../../../../src/pm/i_hydra/libhydra/demux/hydra_demux_poll.c:205): poll for event error
[mpiexec@node2] HYD_bstrap_setup (../../../../../src/pm/i_hydra/libhydra/bstrap/src/intel/i_hydra_bstrap.c:650): error waiting for event
[mpiexec@node2] main (../../../../../src/pm/i_hydra/mpiexec/mpiexec.c:1881): error setting up the boostrap proxies
```
###### Solution: 
This is because slurm versions on node9, node11, node12, node16 are older (16.05) than other nodes (17.11), so whener slurm request resources
on these nodes, the errors occured. We can resolve this error by exclude these nodes from slurm jobs:
```
--exclude node9,node10,node11,node12,node16 ( I also exclude node10 because there is no intel installed in this node). 
```



