## Running jobs on clusters

The command `sbatch`, which is shown below, is used to submit jobs to the queue. Additional commands to work with and monitor the queue/scheduler include those shown in the table below.

|Command|Description|
|--|--|
|squeue|reports the state of jobs or job steps.|
|sinfo|reports the state of partitions and nodes managed by Slurm|
|scancel|used to cancel a pending or running job or job step. It can also be used to send an arbitrary signal to all processes associated with a running job or job step.|
|strigger|used to set, get or view event triggers.|

!>The listed queue resource limitations are based on planned limitations. Queue resource limitations may vary for specific users and user groups according to node buy-in, project needs, current resource use, and/or administrative needs. Contact the system administration team for clarification or to request a variance from standard limitations.

Typical use of the queue system begins by writing a submission script that will be handed to the queue and scheduling system. The submission script is comprised of two sections: (1) a list of SGE options and resource requirements to guide the scheduler’s assignment of the job to one or more production nodes; and (2) a standard shell script that carries out the user desired calculation. To submit a submission script call `my_job.sub` to the queue, one uses the following command:
```bash
sbatch my_job.sub
```
The `my_job.sub` sample script for MERCED is provided below
```bash
#!/bin/bash  
#SBATCH --mail-user=UCMercedNetID@ucmerced.edu  
#SBATCH --mail-type=ALL  
#SBATCH --nodes=1  
#SBATCH --ntasks=20
#SBATCH --partition std.q  
#SBATCH --mem=96G  
#SBATCH --time=0-00:15:00 # 15 minutes  
#SBATCH --output=test1.qlog  
#SBATCH --job-name=test1  
#SBATCH --export=ALL

# This submission file will run a simple set of commands. All stdout will
# be captured in test1.qlog (as specified in the Slurm command --output above).
# This job file uses a shared-memory parallel environment and requests 20
# cores (--ntasks option) on a single node(--nodes option). This job will also run a global #script called
# run. For more info on this script, cat /usr/local/bin/merced_node_print.
#  

whoami
```
The `my_job.sub` sample script for Pinnacles is provided below, the job scripts for the two machines look quite similar, however, they have different partition names and time limits:
```bash
#!/bin/bash
#SBATCH --mail-user=UCMercedNetID@ucmerced.edu  
#SBATCH --mail-type=ALL  
#SBATCH --nodes=1    # request only 1 node
#SBATCH --partition test      # this job will be submitted to test queue
#SBATCH --time=0-00:15:00 # 15 minute
#SBATCH --ntasks-per-node=56 # this job requests for 56 cores on a node
#SBATCH --output=my_%j.stdout    # standard output will be redirected to this file
# #SBATCH --constraint=bigmem   #uncomment this line if you need the access to the bigmem node for Pinnacles
# #SBATCH --constraint=gpu #uncomment this line if you need the access to GPU
# #SBATCH --gres=gpu:2   #uncomment this line if you need GPU access (2 GPUs)
#SBATCH --job-name=my_job    # this is your job’s name
#SBATCH --export=ALL

#  type 'man sbatch' for more information and options
#  this job will ask for 1 full CPU node (56 cores) for 10 min

# run your job
```