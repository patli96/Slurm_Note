## Configuration

By default, no GRES are enabled in the cluster's configuration.  
You must explicitly specify which GRES are to be managed in the slurm.conf configuration file.  
The configuration parameters of interest are **GresTypes** and **Gres**.

***NOTE***  
The GRES specification for each node works in the same fashion as the other resources managed.  
Nodes which are found to have fewer resources than configured will be placed in a `DRAIN` state.

Snippet from an example slurm.conf file:
```bash
# Configure four GPUs (with MPS), plus bandwidth
GresTypes=gpu,mps,bandwidth
NodeName=tux[0-7] Gres=gpu:tesla:2,gpu:kepler:2,mps:400,bandwidth:lustre:no_consume:4G
```

There are cases where <ins>***you may want to define a Generic Resource on a node without specifying a quantity of that GRES***</ins>.  
For example, the filesystem type of a node doesn't decrease in value as jobs run on that node. You can use the `no_consume` flag to allow users to request a GRES without having a defined count that gets used as it is requested.  

If `AutoDetect=nvml` is set in `gres.conf`, and the ***NVIDIA Management Library (NVML)*** is installed on the node and was found during Slurm configuration, <ins>***configuration details will automatically be filled in for any system-detected NVIDIA GPU***</ins>.  
This removes the need to explicitly configure GPUs in `gres.conf`, though the `Gres=` line in slurm.conf is still required in order to tell slurmctld how many GRES to expect.  

By default, all system-detected devices are added to the node.  
However, if `Type` and `File` in `gres.conf` match a GPU on the system, any other properties explicitly specified (e.g. `Cores` or `Links`) can be double-checked against it.  
If the system-detected GPU differs from its matching GPU configuration, then the GPU is omitted from the node with an error.  
<ins>***This allows `gres.conf` to serve as an optional sanity check and notifies administrators of any unexpected changes in GPU properties.***</ins>

If `AutoDetect` is specified and a matching system GPU is not found, no validation takes place and the GPU is assumed to be as the configuration says.

For `Type` to match a system-detected device, it must either exactly match or be a substring of the GPU name reported by slurmd via the AutoDetect mechanism.  
This GPU name will have all spaces replaced with underscores.  
To see the GPU name, set `SlurmdDebug=debug2` in your `slurm.conf` and either restart or reconfigure slurmd and check the slurmd log.  
For example, with `AutoDetect=nvml`:
```bash
# Configure four GPUs (with MPS), plus bandwidth
AutoDetect=nvml
Name=gpu Type=gp100  File=/dev/nvidia0 Cores=0,1
Name=gpu Type=gp100  File=/dev/nvidia1 Cores=0,1
Name=gpu Type=p6000  File=/dev/nvidia2 Cores=2,3
Name=gpu Type=p6000  File=/dev/nvidia3 Cores=2,3
Name=mps Count=200  File=/dev/nvidia0
Name=mps Count=200  File=/dev/nvidia1
Name=mps Count=100  File=/dev/nvidia2
Name=mps Count=100  File=/dev/nvidia3
Name=bandwidth Type=lustre Count=4G Flags=CountOnly
```
```bash
debug:  gpu/nvml: init: init: GPU NVML plugin loaded
debug2: gpu/nvml: _nvml_init: Successfully initialized NVML
debug:  gpu/nvml: _get_system_gpu_list_nvml: Systems Graphics Driver Version: 450.36.06
debug:  gpu/nvml: _get_system_gpu_list_nvml: NVML Library Version: 11.450.36.06
debug2: gpu/nvml: _get_system_gpu_list_nvml: Total CPU count: 6
debug2: gpu/nvml: _get_system_gpu_list_nvml: Device count: 1
debug2: gpu/nvml: _get_system_gpu_list_nvml: GPU index 0:
debug2: gpu/nvml: _get_system_gpu_list_nvml:     Name: geforce_rtx_2060
debug2: gpu/nvml: _get_system_gpu_list_nvml:     UUID: GPU-g44ef22a-d954-c552-b5c4-7371354534b2
debug2: gpu/nvml: _get_system_gpu_list_nvml:     PCI Domain/Bus/Device: 0:1:0
debug2: gpu/nvml: _get_system_gpu_list_nvml:     PCI Bus ID: 00000000:01:00.0
debug2: gpu/nvml: _get_system_gpu_list_nvml:     NVLinks: -1
debug2: gpu/nvml: _get_system_gpu_list_nvml:     Device File (minor number): /dev/nvidia0
debug2: gpu/nvml: _get_system_gpu_list_nvml:     CPU Affinity Range - Machine: 0-5
debug2: gpu/nvml: _get_system_gpu_list_nvml:     Core Affinity Range - Abstract: 0-5
```
In this example, the GPU's name is reported as `geforce_rtx_2060`.  
So in your `slurm.conf` and `gres.conf`, the GPU Type can be set to `geforce`, `rtx`, `2060`, `geforce_rtx_2060`, or any other substring, and slurmd should be able to match it to the system-detected device `geforce_rtx_2060`.

<br></br>
## Runing Jobs

Jobs will not be allocated any generic resources unless specifically requested at job submit time using the options:

--gres  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Generic resources required per node  
--gpus  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GPUs required per job  
--gpus-per-node  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GPUs required per node. Equivalent to the --gres option for GPUs.  
--gpus-per-socket  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GPUs required per socket. Requires the job to specify a task socket.  
--gpus-per-task. 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GPUs required per task. Requires the job to specify a task count.  

<br></br>
## GPU Management


<br></br>
## MPS Management
