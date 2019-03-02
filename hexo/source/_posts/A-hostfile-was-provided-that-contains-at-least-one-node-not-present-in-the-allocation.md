---
title: A hostfile was provided that contains at least one node not
date: 2017-05-23 21:06:53
categories: 集群管理
tags: [OpenMPI,高性能计算]
---

Re: [OMPI users] Problem running an MPI program through the PBS manager

On Mon, Sep 26, 2016 at 2:04 PM, Gilles Gouaillardet <
gilles.gouaillar...@gmail.com> wrote:
<!--more-->
> Mahmood,
	>
	> The node is defined in the PBS config, however it is not part of the
	> allocation (e.g. job) so it cannot be used, and hence the error message.
	>
	> In your PBS script, you do not need -np nor -host parameters to your
	> mpirun command.
	> Open MPI mpirun will automatically detect it is launched from a PBS job,
	> and get the needed information directly from PBS.
	>
	> FWIW, the list of allocated nodes is in the file $PBS_NODEFILE, but you
	> should not need that.
	>
	> Cheers,
	>
	> Gilles
	>
	>
	> On Monday, September 26, 2016, Mahmood Naderan <mahmood...@gmail.com>
	> wrote:
	>
	>> Hi,
	>> When I run an MPI command through the terminal the programs runs fine on
	>> the compute node specified in hosts.txt.
	>>
	>> However, when I put that command in a PBS script, if says that the
	>> compute node is not defined in the job manager's list. However, that node
	>> is actually defined in the job manager.
	>>
	>> Please see the output below
	>>
	>>
	>> mahmood@cluster:tran-bt-o-40$ cat submit.tor
	>> #!/bin/bash
	>> #PBS -V
	>> #PBS -q default
	>> #PBS -j oe
	>> #PBS -l nodes=1:ppn=15
	>> #PBS -N job-1
	>> #PBS -o /home/mahmood/tran-bt-o-40/cc-bt-cc-163-20.out
	>> cd $PBS_O_WORKDIR
	>> /share/apps/computer/openmpi-2.0.0/bin/mpirun -hostfile hosts.txt -np 15
	>> /share/apps/chemistry/siesta-4.0/tpar/transiesta <
	>> trans-cc-bt-cc-163-20.fdf
	>> mahmood@cluster:tran-bt-o-40$ cat cc-bt-cc-163-20.out
	>> ------------------------------------------------------------
	>> --------------
	>> A hostfile was provided that contains at least one node not
	>> present in the allocation:
	>>
	>>   hostfile:  hosts.txt
	>>   node:      compute-0-1
	>>
	>> If you are operating in a resource-managed environment, then only
	>> nodes that are in the allocation can be used in the hostfile. You
	>> may find relative node syntax to be a useful alternative to
	>> specifying absolute node names see the orte_hosts man page for
	>> further information.
	>> ------------------------------------------------------------
	>> --------------
	>> mahmood@cluster:tran-bt-o-40$ cat hosts.txt
	>> compute-0-1
	>> compute-0-2
	>> mahmood@cluster:tran-bt-o-40$ pbsnodes -l all
	>> compute-0-0          down
	>> compute-0-1          free
	>> compute-0-2          free
	>> compute-0-3          free
	>>
	>>
	>>
	>> As you can see, compute-0-1 has free cores and it is defined for the
	>> manager.
	>>
	>> Any idea?
	>> Regards,
	>> Mahmood
	>>
	>>
	>>
	> _______________________________________________
	> users mailing list
	> users@lists.open-mpi.org
	> https://rfd.newmexicoconsortium.org/mailman/listinfo/users
	>
	_______________________________________________
	users mailing list
	users@lists.open-mpi.org
	https://rfd.newmexicoconsortium.org/mailman/listinfo/users

