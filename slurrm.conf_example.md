```bash
#
# Sample /etc/slurm.conf for dev[0-25].llnl.gov
# Author: John Doe
# Date: 11/06/2001
#
SlurmctldHost=dev0(12.34.56.78) # Primary server
SlurmctldHost=dev1(12.34.56.79) # Backup server
#
AuthType=auth/munge
Epilog=/usr/local/slurm/epilog
Prolog=/usr/local/slurm/prolog
FirstJobId=65536
InactiveLimit=120
JobCompType=jobcomp/filetxt
JobCompLoc=/var/log/slurm/jobcomp
KillWait=30
MaxJobCount=10000
MinJobAge=3600
PluginDir=/usr/local/lib:/usr/local/slurm/lib
ReturnToService=0
SchedulerType=sched/backfill
SlurmctldLogFile=/var/log/slurm/slurmctld.log
SlurmdLogFile=/var/log/slurm/slurmd.log
SlurmctldPort=7002
SlurmdPort=7003
SlurmdSpoolDir=/var/spool/slurmd.spool
StateSaveLocation=/var/spool/slurm.state
SwitchType=switch/none
TmpFS=/tmp
WaitTime=30
JobCredentialPrivateKey=/usr/local/slurm/private.key
JobCredentialPublicCertificate=/usr/local/slurm/public.cert
#
# Node Configurations
#
NodeName=DEFAULT CPUs=2 RealMemory=2000 TmpDisk=64000
NodeName=DEFAULT State=UNKNOWN
NodeName=dev[0-25] NodeAddr=edev[0-25] Weight=16
# Update records for specific DOWN nodes
DownNodes=dev20 State=DOWN Reason="power,ETA=Dec25"
#
# Partition Configurations
#
PartitionName=DEFAULT MaxTime=30 MaxNodes=10 State=UP
PartitionName=debug Nodes=dev[0-8,18-25] Default=YES
PartitionName=batch Nodes=dev[9-17] MinNodes=4
PartitionName=long Nodes=dev[9-17] MaxTime=120 AllowGroups=admin


```
