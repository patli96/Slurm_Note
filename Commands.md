## Commands

#### Gather Clusters Information

```bash
# Gather all simple information
$ sinfo

# Display node and other information
$ sinfo -Nl

# Display required information
$ sinfo -o "%20N %10c %10m %25f %20G"
# N = node name
# c = number of corers
# m = memory
# f = features, often it will be the architecture or type of associated gpu
# G = gres type and number
```

#### Update node states

```bash
# drain nodes to maintain mode. ex: nodes=worker[01-02],worker08
$ scontrol update NodeName=${nodes} State=DOWN Reason=”Maintain”

# resume nodes
$ scontrol update NodeName=worker[01-02] State=Resume
```

#### Run jobs

```bash
# run a job via srun on 2 nodes (using dd to simulate a high CPU consume job)
$ srun -N2 dd if=/dev/zero of=/dev/null

# run a job with a time constrain. Format: 
# - minute
# - minute:second
# - hours:minutes:seconds
# - days-hours
# - days-hours:minutes 
# - days-hours:minutes:seconds)
$ srun -N2 --time=01:00 dd if=/dev/zero of=/dev/null
```

#### Reservation

```bash
# reserve nodes for a user to test
$ scontrol create reservation ReservationName=maintain \
  starttime=now duration=120 user=root flags=maint,ignore_jobs nodes=ALL
# must specify reservation; otherwise, the job will not run
$ srun --reservation=maintain ping 8.8.8.8 2>&1 > /dev/null &

# show reservations
$ scontrol show res

# delete a reservation
$ scontrol delete ReservationName=maintain
```

#### Cancel jobs

```bash
# cancel a job
$ scancel "${jobid}"

# cancel a job and disable warnings
$ scancel -q "${jobid}"

# cancel all jobs which are belong to an account
$ scancel --account="${account}"

# cancel all jobs which are belong to a partition
$ scancel --partition="${partition}"

# cancel all pending jobs
$ scancel --state="PENDING"

# cancel all running jobs
$ scancel --state="RUNNING"

# cancel all jobs
$ squeue -l | awk '{ print $ 1}' | grep '[[:digit:]].*' | xargs scancel

# cancel all jobs (using state option)
$ for s in "RUNNING" "PENDING" "SUSPAND"; do scancel --state="$s"; done
```

#### Account & User

```bash
# create a cluster (the clustername should be identical to ClusterName in slurm.conf)
$ sacctmgr add cluster clustername

# create an account
$ sacctmgr -i add account worker description="worker account" Organization="your.org"

# create an user and add to an account
$ sacctmgr create user name=worker DefaultAccount=default

# create an user and add to additional accounts
$ sacctmgr -i create user "worker" account="worker" adminlevel="None"

# modify user fairshare configuration
$ sacctmgr modify user where name="worker" account="worker" set fairshare=0

# remove an user from an account
$ sacctmgr remove user "worker" where account="worker"

# show all users
$ sacctmgr show account

# show all users with associations
$ sacctmgr show account -s
```
