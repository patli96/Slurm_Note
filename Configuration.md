## slurm.conf

- Slurm configuration file

This file should be consistent across all nodes in the cluster.

Changes to the configuration file take effect upon restart of Slurm daemons, daemon receipt of the SIGHUP signal, 
or execution of the command ```$ scontrol reconfigure``` unless otherwise noted.

</br></br>
## slurmdbd.conf

- Slurm Database Daemon configuration file

This file shoud be only on the computer where SlurmDBD executes and should only be readable by the user which executes SlurmDBD (e.g. "slurm").

This should be protected from unauthorized access since it contains a database password.

### Archive Info

ArchiveDir
> Where the file will be placed after a purge event has happened and archive for that element is set to true. 
> **/tmp by default**.
> The **format** for this files name is `$ArchiveDir/$ClusterName_$ArchiveObject_archive_$BeginTimeStamp_$endTimeStamp` 

ArchiveEvents
> Whether archive events when purging them.
> **No by default**.

ArchiveJobs
> Whether archive jobs when purging them.
> **No by default**.

ArchiveResvs
> Whether archive reservations when purging them.
> **No by default**.

ArchiveScript

