## slurm.conf

- Slurm configuration file

This file should be consistent across all nodes in the cluster.

Changes to the configuration file take effect upon restart of Slurm daemons, daemon receipt of the SIGHUP signal, 
or execution of the command ```$ scontrol reconfigure``` unless otherwise noted.

***Slurm Configuration Tool*** [:link:](https://slurm.schedmd.com/configurator.html)

</br></br>
## slurmdbd.conf

- Slurm Database Daemon configuration file

This file shoud be only on the computer where SlurmDBD executes and should only be readable by the user which executes SlurmDBD (e.g. "slurm").

This should be protected from unauthorized access since it contains a database password.

</br></br>
## gres.conf

- Generic Resource management

If the GRES information in the slurm.conf file does not fully describe those resources, then a gres.conf file shoule be included on each compute node.

If the GRES information in the slurm.conf file fully describes those resources (i.e. no "Cores", "File" or "Links" specification is required for that GRES type or that information is automatically detected), that information may be omitted from the gres.conf file and only the configuration information in the slurm.conf file will be used. The gres.conf file may be omitted completely if the configuration information in the slurm.conf file fully describes all GRES.

**NOTE**: Slurm support for gres/mps requires the use of the select/cons_tres plugin. For more information on how to configure MPS, see [MPS_Management](https://slurm.schedmd.com/gres.html#MPS_Management)


