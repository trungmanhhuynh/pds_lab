# Submit CUDA program using sbatch

1. ssh node18 to compile code 
```
>> ssh node18 nvcc ssh node18 nvcc /home/manhh/CSCI7551/Lab/Lab-4_Cuda/mm-cuda2.cu 
-o /home/manhh/CSCI7551/Lab/Lab-4_Cuda/mm-cuda2 
```
2. Write SLURM script gpu.slurm 
```
#!/bin/bash
#SBATCH -J mm-cuda2
#SBATCH --partition=short-gpu
#SBATCH --mem=20G                        # memory             
#SBATCH --output=output.log              # ouput file
#SBATCH --gres=gpu:P100_SXM:4            # max number of GPUs
         
./mm-cuda2 1000 
```

3. Run SLURM script **must** be executed from master node. 
```
sbatch gpu.slurm
```

4. Result is written in output.log
```
Program runs in 0.70 seconds
```
