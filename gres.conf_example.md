```bash
##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Define GPU devices with MPS support, with AutoDetect sanity checking
##################################################################
AutoDetect=nvml
Name=gpu Type=gtx560 File=/dev/nvidia0 COREs=0,1
Name=gpu Type=tesla File=/dev/nvidia1 COREs=2,3
Name=mps Count=100 File=/dev/nvidia0 COREs=0,1
Name=mps Count=100 File=/dev/nvidia1 COREs=2,3


##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Overwrite system defaults and explicitly configure three GPUs
##################################################################
Name=gpu Type=tesla File=/dev/nvidia[0-1] COREs=0,1
# Name=gpu Type=tesla File=/dev/nvidia[2-3] COREs=2,3
# NOTE: nvidia2 device is out of service
Name=gpu Type=tesla File=/dev/nvidia3 COREs=2,3

##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Use a single gres.conf file for all compute nodes - positive method
##################################################################
## Explicitly specify devices on nodes tux0-tux15
# NodeName=tux[0-15] Name=gpu File=/dev/nvidia[0-3]
# NOTE: tux3 nvidia1 device is out of service
NodeName=tux[0-2] Name=gpu File=/dev/nvidia[0-3]
NodeName=tux3 Name=gpu File=/dev/nvidia[0,2-3]
NodeName=tux[4-15] Name=gpu File=/dev/nvidia[0-3]

##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Use NVML to gather GPU configuration information
# for all nodes except one
##################################################################
AutoDetect=nvml
NodeName=tux3 AutoDetect=off Name=gpu File=/dev/nvidia[0-3]
##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Specify some nodes with NVML, some with RSMI, and some with no AutoDetect
##################################################################
NodeName=tux[0-7] AutoDetect=nvml
NodeName=tux[8-11] AutoDetect=rsmi
NodeName=tux[12-15] Name=gpu File=/dev/nvidia[0-3]

##################################################################
# Slurm's Generic Resource (GRES) configuration file
# Define 'bandwidth' GRES to use as a way to limit the
# resource use on these nodes for workflow purposes
##################################################################
NodeName=tux[0-7] Name=bandwidth Type=lustre Count=4G Flags=CountOnly
```
