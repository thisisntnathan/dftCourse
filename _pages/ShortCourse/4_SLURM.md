---
layout: page
title: <kbd>SLURM</kbd>
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 4
permalink: /ShortCourse/slurm.html
---
<kbd>SLURM</kbd>, formerly known as the **S**imple **L**inux **U**tility for **R**esource **M**anagement, is a type of program called a workload manager.<sup>1</sup> On large, multi-user systems it can be advantageous and equitable for a program to control the allocation of computational resources, <kbd>SLURM</kbd> does just that.  

When you want to run a job on the CHEM cluster you have to ask the <kbd>SLURM</kbd> daemon for resources to allocate to your job. It then takes your script, figures out how much compute power you want, and, if the nodes/memory are available, runs your script on them.  If not, it places them in a queue until the requested resources become available to you.  

<!-- ### Commands specific to SLURM -->

<kbd>SLURM</kbd> has its own set of commands, and its full documentation can be found [here](https://slurm.schedmd.com/),<sup>2</sup> but here we'll go over only the most important ones: `sinfo`, `pestat`, `squeue`, `sbatch`, and `scancel`.  

### Gathering information

`sinfo` gives us information about the status of the cluster's computing nodes (a node is a single computer in the cluster).

```sh
nml64@as-chm-cluster | ~ $ sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST 
chemq        up   infinite      1  alloc chem001 
chemq        up   infinite      5   idle chem[002-006] 
collumq      up   infinite      4  down* dbc[001-003,005] 
collumq      up   infinite      1    mix dbc009 
collumq      up   infinite      1  alloc dbc007 
collumq      up   infinite      4   idle dbc[004,006,008,010] 
widomq       up   infinite      1  drain bw001 
widomq       up   infinite      1    mix bw007 
widomq       up   infinite      5   idle bw[002-006]
```

By default, `sinfo` lists the nodes by their partition; the highest level of cluster organization (a **partition** is a set of **compute nodes**). On our shared system nodes are partitioned by ownership, but other systems may have partitions based on usage (e.g. large jobs, small jobs, post-processing, data visualization, etc...) allowing the admin to install different programs on different partitions.

By default <kbd>SLURM</kbd> commands only show us nodes we have access to (more on how to change that below). So, for example, in the above snippet we have 3 partitions `chemq`, `collumq`, and `widdomq`. `collumq` has 10 total nodes, 4 of which (dbc1-3 and dbc5) are currently down, 1 of which (dbc7) is fully allocated to a running job, 1 of which (dob9) is mixed, which means it still has resources available, and 4 of which (dbc4,6,8,10) are idle. `sinfo` doesn't provide the most readable output, so sometimes its easier to use `pestat`.  

Notice how the terminal's command prompt has changed from `NathanLui@Local` to `nml64@as-chm-cluster`. This is because I'm now connected to the cluster, instead of working locally on my own computer (more on how to do this in the next section).  

`pestat` is quite similar to `sinfo -N` (`-N` provides a node-oriented view of the cluster), but I find the layout much easier to read. `pestat` also gives us data as to the CPU and memory capacities of each node which will be helpful later.

```sh
nml64@as-chm-cluster | ~ $ pestat
Hostname       Partition     Node Num_CPU  CPUload  Memsize  Freemem  Joblist
                            State Use/Tot              (MB)     (MB)  JobId User ...
   bw001          widomq    drain*  0  12    0.00     48277    37640   
   bw002          widomq     idle   0  12    0.00     64382    62151   
   bw003          widomq     idle   0  12    0.00     64382    62139   
   bw004          widomq     idle   0  12    0.00     64382    62144   
   bw005          widomq     idle   0  12    0.00     64382    62135   
   bw006          widomq     idle   0  12    0.00     64382    62135   
   bw007          widomq      mix   2  12    1.00*    64382    14752  8609 m----  
 chem001           chemq    alloc  16  16   15.98     31935    24991  8691 j-----  
 chem002           chemq     idle   0  16    0.00     31935    29712   
 chem003           chemq     idle   0  16    0.00     31935    29711   
 chem004           chemq     idle   0  16    0.00     31935    29721   
 chem005           chemq     idle   0  16    0.00     31935    29728   
 chem006           chemq     idle   0  16    0.00     31935    29730   
  dbc001         collumq    down*   0   8    0.00*    16032        0   
  dbc002         collumq    down*   0   8    0.00*     7968        0   
  dbc003         collumq    down*   0   8    0.00*    16032        0   
  dbc004         collumq     idle   0  16    0.00     24085    21791   
  dbc005         collumq    down*   0  16    0.00*    24085        0   
  dbc006         collumq     idle   0  16    0.00     24085    21805   
  dbc007         collumq    alloc  12  12   11.96     32126    26506  8652 nml64  
  dbc008         collumq     idle   0  12    0.00     32126    29959   
  dbc009         collumq      mix  12  40   11.82    192049   182642  8679 nml64  
  dbc010         collumq     idle   0  40    0.00    192049   189551   
```

`squeue` displays the current job queue:

```sh
nml64@as-chm-cluster | ~ $ squeue
    JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST 
     8609     chemq matlab_t    m----  R 5-13:38:51      1 bw007 
     8652   collumq trans-Na    nml64  R 3-20:58:56      1 dbc007 
     8679   collumq cis-NaTB    nml64  R 1-03:59:54      1 dbc009 
     8691     chemq A2HMPA3_   j-----  R      39:33      1 chem001 
```

Notice how `squeue` gives us a lot of information about the running jobs; it tells us the job number, who's running the job, the number of node(s), which node(s), their respective partitions, and how long the jobs have been running for.  

By default, `squeue` and `sinfo` only gives us data on the nodes we have permission to use, but if we wanted to check on other nodes we can use the `-all` switch.

```sh
nml64@as-chm-cluster | ~ $ sinfo -all
Tue Dec 21 15:29:47 2021
PARTITION AVAIL  TIMELIMIT   JOB_SIZE ROOT OVERSUBS     GROUPS  NODES       STATE NODELIST 
chemq        up   infinite 1-infinite   no       NO chemit,col      1   allocated chem001 
chemq        up   infinite 1-infinite   no       NO chemit,col      5        idle chem[002-006] 
slinq        up   infinite 1-infinite   no       NO slin,chemi      1   allocated sl001 
slinq        up   infinite 1-infinite   no       NO slin,chemi      1        idle sl002 
wilsonq      up   infinite 1-infinite   no       NO wilson,che      2        idle jjw[001-002] 
chenq        up   infinite 1-infinite   no       NO chen,chemi      1       mixed pc002 
chenq        up   infinite 1-infinite   no       NO chen,chemi      1        idle pc001 
loringq      up   infinite 1-infinite   no       NO loring,che      2       down* rl[001,003] 
loringq      up   infinite 1-infinite   no       NO loring,che      1     drained rl004 
loringq      up   infinite 1-infinite   no       NO loring,che      1        idle rl002 
collumq      up   infinite 1-infinite   no       NO collum,che      4       down* dbc[001-003,005] 
collumq      up   infinite 1-infinite   no       NO collum,che      1       mixed dbc009 
collumq      up   infinite 1-infinite   no       NO collum,che      1   allocated dbc007 
collumq      up   infinite 1-infinite   no       NO collum,che      4        idle dbc[004,006,008,010] 
widomq       up   infinite 1-infinite   no       NO chemit,col      1     drained bw001 
widomq       up   infinite 1-infinite   no       NO chemit,col      1       mixed bw007 
widomq       up   infinite 1-infinite   no       NO chemit,col      5        idle bw[002-006] 
lambertq     up   infinite 1-infinite   no       NO lambert,ch      2   allocated tl[001-002] 
nml64@as-chm-cluster | ~ $ squeue -all
Tue Dec 21 15:30:50 2021
JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMIT   NODES NODELIST(REASON) 
 8590     slinq B3LYP-D3    y----  RUNNING 6-01:40:46 UNLIMITED        1 sl001 
 8608     chenq matlab_t    m----  RUNNING 5-13:51:21 37-12:00:00      1 pc002 
 8609    widomq matlab_t    m----  RUNNING 5-13:48:40 37-12:00:00      1 bw007 
 8614     chenq matlab_t    m----  RUNNING 5-01:41:36 37-12:00:00      1 pc002 
 8652   collumq trans-Na    nml64  RUNNING 3-21:08:45 17-12:00:00      1 dbc007 
 8679   collumq cis-NaTB    nml64  RUNNING 1-04:09:43 17-12:00:00      1 dbc009 
 8686     slinq B3LYP-D3    y----  RUNNING    3:18:46 UNLIMITED        1 sl001 
 8688     slinq B3LYP-D3    y----  RUNNING      52:00 UNLIMITED        1 sl001 
 8691     chemq A2HMPA3_   j-----  RUNNING      49:22 17-12:00:00      1 chem001 
 8692  lambertq Dimer-6m     k---  RUNNING      19:07 5-00:00:00       1 tl001 
 8693  lambertq Dimer-6m     k---  RUNNING      19:07 5-00:00:00       1 tl002 
```

There are many switches you can use to filter the output of `squeue` and `sinfo` by user `--user`, partition `--partition`, node state `--state`, etc.

These are some of the most important commands we'll use in this tutorial. A short cheat sheet can be found [here](https://slurm.schedmd.com/pdfs/summary.pdf).<sup>3</sup>

### Submitting jobs

`sbatch` and `scancel` are mirror commands.  `sbatch <script>` submits the job script to the SLURM daemon for resource allocation, and returns a job ID number. `scancel <job ID>` cancels a job after allocation i.e., before or after a job starts running. Any files that have already been written will be preserved as they are when `scancel` is executed (keep this in mind if you choose to write any large scratch files to your job directory instead of `/scratch`).  In the next section, we'll learn about how to format submission scripts and submit our first <kbd>SLURM</kbd> job.

<br />

| <center>Previous<br><a href="/dftCourse/ShortCourse/firstScript.html">My First Script</a></center> | <center><a href="/dftCourse/Introduction.html">Home</a></center> | <center>Next<br><a href="/dftCourse/ShortCourse/slurmScripts.html">My First <kbd>SLURM</kbd> Job</a></center> |

<br />

#### References

(1) [SLURM Workload Manager](https://en.wikipedia.org/wiki/Slurm_Workload_Manager)  
(2) [SLURM Documentation](https://slurm.schedmd.com/)  
(3) [SLURM Cheat Sheet](https://slurm.schedmd.com/pdfs/summary.pdf)
