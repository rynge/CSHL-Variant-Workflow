CSHL-Variant-Workflow
=============
software.tar.gz contains
            bwa-0.7.12
            GenomeAnalysisTK-3.4.46
            picard-tools-1.119
            samtools-1.2
            seqtk

The workflow is controlled by a configuration file named
~/.irri-workflow.conf . Create the file with this content:

```
# local refers to the submit host. Specify paths to a directory
# which can be used by the workflow as work space, and locations
# for local software installs.
[local]

work_dir = /local-scratch/%(username)s/irri

irods_bin = /ccg/software/irods/3.2/bin

# tacc refers to configuration for the TACC Stampede 
# supercomputer. To use this machine, you need an allocation
# (start with TG-) and you also need to know your username
# and storage group name for the system. The easiest way to 
# obtain those is to log into the system, and run:
# cds; pwd
# This should return a path like: /scratch/00384/rynge. The
# storage group is the second level, and your username is 
# last level.
[tacc]

allocation = TG-BIO150041

username = lwang

storage_group = 01308

```


To access data from the iPlant iRods repository, you need a file in your
home directory named ~/irods.iplant.env, with 0600 permission and
content like:

```
irodsHost data.iplantcollaborative.org
irodsPort 1247
irodsUserName YOUR_IRODS_USERNAME
irodsZone iplant
# Pegasus requirement
irodspassword 'YOUR_IRODS_PASSWORD'
```

To generate the workflow:

```
$ ./workflow-generator --exec-env tacc-stampede
```

The workflow can also run in a distributed mode, using HTCondor to schedule
the jobs. Currently, the configuration is for the ISI submit node, and the
workflow can be generated with:

```
$ ./workflow-generator --exec-env distributed
```

