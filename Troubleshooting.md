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

## CuPY error 
When I run my research code using Chainer with Cupy-cuda90, this error occured: 
```
   ls.add_ptr_data(ptx, u'cupy.ptx')
  File "cupy/cuda/function.pyx", line 212, in cupy.cuda.function.LinkState.add_ptr_data
  File "cupy/cuda/function.pyx", line 214, in cupy.cuda.function.LinkState.add_ptr_data
  File "cupy/cuda/driver.pyx", line 153, in cupy.cuda.driver.linkAddData
  File "cupy/cuda/driver.pyx", line 82, in cupy.cuda.driver.check_status
cupy.cuda.driver.CUDADriverError: CUDA_ERROR_INVALID_PTX: a PTX JIT compilation failed
```

### Current system enviroment: 

```
cupy-cuda90 
version 6.5.0
chainer version 6.5.0
```
```
(pytorchenv) [manhh@node18 fpl-fixed]$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2017 NVIDIA Corporation
Built on Fri_Sep__1_21:08:03_CDT_2017
Cuda compilation tools, release 9.0, V9.0.176
```
```
 echo $PATH
/usr/local/bin:/home/manhh/miniconda3/envs/pytorchenv/bin:/home/manhh/miniconda3/bin:/usr/local/torch/install/bin:/usr/lib64/qt-3.3/bin:/usr/local/mpi/intel/openmpi-1.10.4/bin:/usr/local/maven/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/local/cuda/bin:/usr/local/gcc-5.4.0/bin:/opt/ibutils/bin:/home/manhh/.local/bin:/home/manhh/bin:/home/manhh/bin
```
```
(pytorchenv) [manhh@node18 fpl-fixed]$ echo $LD_LIBRARY_PATH
/opt/boost/lib/:/usr/local/cuda-9.0/lib64:/usr/local/torch/install/lib:/usr/local/mpi/intel/openmpi-1.10.4/lib64
```

