# BurstBuffer


**Why we need Burst Buffer?**  
  - Many programs do I/O in bursts, such as read, compute, write, etc.
  - Want to have high bandwidith when doing I/O (Compute resources largely idle during I/O)
  - Disk-based PFS (Parrallel File System) bandwidth is expensive, capacity is cheap
  - SSD bandwidth is relatively inexpensive
  - Large I/O load at the beginning and end of jobs
  - Cray Aries network is faster, lower latency, shorter distance than path to PFS
  - ***Handle spikes in I/O***

**What is Burst Buffer?**  
- A "buffer" spcae with high bandwidth and lower capacity, backed by a disk based PFS.  
  It increased Burst Buffer bandwidth and decreased the time programs spend on I/O.
- Burst Buffer can interact with PFS before, during and after programs use data at "stage-in" and "stage-out" phases.

The Slurm burst buffer plugins call a script at different points during the lifetime of a job:

At job submission
While the job is pending after an estimated start time is established. This is called “stage-in.”
Once the job has been scheduled but has not started running yet. This is called “pre-run.”
Once the job has completed or been cancelled, but Slurm has not released resources for the job yet. This is called “stage-out.”
Once the job has completed, and Slurm has released resources for the job. This is called “teardown.”
This script runs on the slurmctld node. These are the supported plugins:
- DataWarp
- Lua
<br/></br>

## Slurm Configuration
- set `BurstBufferType` in `slurm.conf`
- may set `DebugFlags=BurstBuffer` in `slurm.conf` for detailed logging from the burst buffer plugin
- can add `bb/datawarp` or `bb/lua` to `AccountingStorageTres` in `slurm.conf` to make Slurm track burst buffer resources
- Burst-Buffer-specific configurations can be set in `burst_buffer.conf`
- The JSON-C library must be installed in order to build Slurm's `burst_buffer/datawarp` and `burst_buffer/lua` plugins, which must parse JSON format data
- The ability for normal users to create persistent burst buffers through slurm is disabled by default
  set `Flags=EnablePersisten` in `burst_buffer.conf`
<br/></br>

## DataWarp
DataWarp is Cray's impementation of the Burst Buffer concept.  
There are both hardware & software components:
- Hardware: 
  - XC40 Service node directly connected to Aries network
  - PCIe SSD Cards installed on the node

- Software:
  - DataWarp service daemons
  - DataWarp Filesystem (using DVS, LVM, XFS)
  - Integration with WoorkLoad Managers (Slurm, M/T, PBSpro)

### DataWarp on Slurm
`BurstBufferType=burst_buffer/datawarp` in `slurm.conf`

The datawarp plugin calls two scripts:
- dw_wlm_cli
- dwstat
<br/></br>


## LUA
<br/></br>

## Slurm Burst Buffer Resources
### DataWarp
- Pools are defined by `dw_wlm_cli`, and represent bytes
- If a job doesn't request a pool, then the pool defined by `DefaultPool` in `burst_buffer.conf` will be used
- If a job doesn't request a pool and `Default Pool` is not defined, then the job will be rejected

### LUA
<br/></br>

## Slurm Job Submission Commands
- For interactive jobs:
  - The `sun` and `salloc` commands can specify burst buffer requirements with the `--bb` and `--bbf` options:
    - `--bbf` to specify path of file with same directives as batch job
    - `--bb` to specify simple inline directives
- For batch jobs:
  - `#BB` prefix for generic plugin directives
  - `#DW` prefix for Cray DataWarp directives  
  ***Exception***: Use `#BB` to create/delete persistent burst buffers, operation not supported by `#DW` directives

### Example
- DataWarp
```bash

```
- Lua
```bash

```
- Persistent Burst Buffer
```bash

```
<br/></br>

## Failure Management


