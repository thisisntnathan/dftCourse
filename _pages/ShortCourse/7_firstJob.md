---
layout: page
title: My First <kbd>Gaussian</kbd> Job
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 7
permalink: /ShortCourse/firstJob.html
---

If you've been following along then you *almost* have all the pieces to run your first Gaussian job on our cluster.  

There's just a little bit of work we need to do tie everything together.  

### Our <kbd>g16</kbd> input file

We should tweak the `.com` file that we were working on last section. First, we'll get rid of the link 0 commands since we can easily (and more reliably) pass them to <kbd>Gaussian</kbd> as environment variables. Next, we'll run a geometry optimization and frequency analysis instead of just calculating the energy.

>A frequency analysis should always accompany geometry optimizations to verify that the final geometry is a global minimum on the potential energy surface (i.e., a true ground state), instead of a saddle point.

To do this we'll specify the following keywords:

<!-- markdownlint-disable MD040 -->
```
#N M062X/def2svp          ! use the m06-2x functional with def2svp basis set
OPT                       ! conduct a ground state geometry optimization
FREQ=NoRaman              ! conduct a frequency analysis of the optimized geometry
                ! do not calculate Raman stretches (reduces computation time by ~30%)
temperature=273.15        ! standard temperature
Integral(Grid=UltraFine)  ! use an ultrafine integration grid (99,590)
```
<!-- markdownlint-enable MD040 -->

So our full input should look like:

{% gist b20f36ab9304fde103b5a0ae7321fbe7 %}

Let's save it into a folder called `eqMeCyclohexane` and start working on our <kbd>SLURM</kbd> script.  

### <kbd>SLURM</kbd> job script

<kbd>Gaussian16</kbd> is slightly more resource intensive than our `hello.sh` script, so we'll need to be a bit better at requesting resources from <kbd>SLURM</kbd> than the minimal requests we made in the last exercise. In your `myFristG16Job` make a new submission script with the following resource allocation request:

```sh
#SBATCH -p chemq                         # submit to partition: chemq
#SBATCH -J eqMeCyhex                     # job name
#SBATCH -o %x_oe                         # name of stdout/stderr file
#SBATCH -N 1                             # total number of nodes requested
#SBATCH --ntasks-per-node=16             # total number of tasks requested
#SBATCH --mincpus=16                     # assign at least 16 CPUs from each node
#SBATCH --mem=0                          # allocate all of the node's available memory
#SBATCH -t 240:00:00                     # max run time (hhh:mm:ss)
#SBATCH --mail-type=ALL                  # send emails at job START/END/FAIL
#SBATCH --mail-user=<yourNetID>@cornell.edu
```

>n.b. The `%x` in the `-o` option line is a <kbd>SLURM</kbd> variable for the job name.  

Now, specify the job details:  

```sh
g16root=/software                      # the g16.profile file defines the g16
. $g16root/g16/bsd/g16.profile         # defaults; ask admin for its location
export GAUSS_SCRDIR=/scratch
export GAUSS_CDEF='0-15'               # see https://gaussian.com/link0/ for
export GAUSS_MDEF='28GB'               # environment variable definitions
export GAUSS_YDEF='eqMeCyhex.chk'
```

The last four lines define environment variables for <kbd>g16</kbd>; these are equivalent to setting [link 0](https://gaussian.com/link0/) commands directly in the `.com` file, but any link 0 commands specified in the `.com` file will override those passed to <kbd>Gaussian</kbd> as environmental variables or command-line options.<sup>1</sup>  

>Recall that the memory and CPU assignments requested by <kbd>g16</kbd> must be at most those requested from the <kbd>SLURM</kbd> resource daemon, otherwise the job will fail due it insufficient resources.

The last thing left to do is to call the program. To run <kbd>g16</kbd> call the executable with `/path/to/g16 <inputFile> <outputFileName>` (by convention, <kbd>Gaussian</kbd> output files have the ending `.log`):

```sh
$g16root/g16/g16 eqMeCyhex.com eqMeCyhex.log
# on AS-CHEM the g16 program is located at /software/g16/g16 on other clusters
# it may not be in the same place. Ask your system admin for its location!
```

At this point we have everything we need. The rest of our job details will be read into g16 from the input file. Our full job script should look like:

{% gist 30ea71341d21b924bb1d73c44b071f22 %}

Save it into the same folder as `eqMeCyhex.com` and now let's go submit it!

### Submitting our first <kbd>Gaussian</kbd> job

In FileZilla connect to the CHEM cluster and drag your whole `eqMeCyclohexane` folder from your computer to the cluster's home directory. `cd` into that folder and make sure everything's where it should be.

```sh
nml64@as-chm-cluster | ~ $ cd eqMeCyclohexane/
nml64@as-chm-cluster | ~/eqMeCyclohexane $ ls
eqMeCyclohex.com  g16run.sh
```

Submit the job with `sbatch`. Its usually a good idea to make sure that the job has started properly. When <kbd>Gaussian</kbd> jobs fail they typically fail in the first 20 seconds (usually due to a FileIO issue, syntax errors, or insufficient resources) so checking that the job is running smoothly prevents you from coming back later just to realize that your job was killed after 7 seconds because you spelled the input filename incorrectly. If we look at the files in our directory while the job is running, we see three new files our checkpoint file `eqMeCyhex.chk`, our <kbd>Gaussian</kbd> output file `eqMeCyhex.log`, and our <kbd>SLURM</kbd> output file `eqMeCyhex_oe`.  

```sh
nml64@as-chm-cluster | ~/eqMeCyclohexane $ sbatch g16run.sh
Submitted batch job 8721
nml64@as-chm-cluster | ~/eqMeCyclohexane $ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
              8721     chemq eqMeCyhe    nml64  R       0:36      1 chem005 
nml64@as-chm-cluster | ~/eqMeCyclohexane $ ls
eqMeCyhex.chk  eqMeCyhex.com  eqMeCyhex.log  eqMeCyhex_oe  g16run.sh
nml64@as-chm-cluster | ~/eqMeCyclohexane $ ls
eqMeCyhex.chk  eqMeCyhex.com  eqMeCyhex.log  eqMeCyhex_oe  fort.7  g16run.sh
```

#### `fort.7`

After the job is completed we see another file `fort.7`. After a job finishes <kbd>Gaussian</kbd> "punches out" (the parlance comes from a time [when computers used punched cards as fileIO](en.wikipedia.org/wiki/Punched_card))<sup>2</sup> a separate output for the results of that calculation. The output can be specified using the [`Punch` keyword](https://gaussian.com/punch/), however <kbd>Gaussian</kbd> punches a new `fort.7` file at each termination. In our combined optimization/frequency calculation the program terminates twice: once following the optimization and again after the vibrational frequency analysis. So, in a combined job you get only the punch out for the final calculation.

### Checking for successful termination

We can do a quick check to make sure our job finished correctly using `tail`.

```sh
nml64@as-chm-cluster | ~/eqMeCyclohexane $ tail eqMeCyhex.log


 ... THE UNIVERSE IS NOT ONLY QUEERER THAN WE SUPPOSE,
 BUT QUEERER THAN WE CAN SUPPOSE ... 

                                   -- J. B. S. HALDANE
 Job cpu time:       0 days  0 hours 27 minutes 52.9 seconds.
 Elapsed time:       0 days  0 hours  1 minutes 46.1 seconds.
 File lengths (MBytes):  RWF=     66 Int=      0 D2E=      0 Chk=      5 Scr=      1
 Normal termination of Gaussian 16 at Mon Dec 27 13:06:58 2021.
```

`tail -n` prints the last `n` lines (by default `n` is 10) of the file to the console. The first two lines tell us how long the job took, the third tells us the size of the job's scratch files, and the final line informs us that <kbd>Gaussian</kbd> terminated without error. Additionally, if your job terminates successfully <kbd>Gaussian</kbd> will print you a quote. If the job had failed the output tail would look something like the code bloc below. Different reasons for failure will produce different looking outputs but all failed jobs will produce an `Error termination` message.  

```sh
nml64@as-chm-cluster | ~/.../NaTMIPS/A3_eee_tol $ tail eqMeCyhex.log
 Error on total polarization charges = ********
 SCF Done:  E(RM062X) =  -8076.68013868     A.U. after  129 cycles
            NFock=128  Conv=0.45D-02     -V/T= 7.5222
 SMD-CDS (non-electrostatic) energy       (kcal/mol) =      -2.87
 (included in total energy above)
 Convergence failure -- run terminated.
 Error termination via Lnk1e in /software/g16/l502.exe at Sat Dec 18 17:05:50 2021.
 Job cpu time:      14 days  5 hours  4 minutes 13.4 seconds.
 Elapsed time:       1 days  5 hours 16 minutes 45.2 seconds.
 File lengths (MBytes):  RWF=    958 Int=      0 D2E=      0 Chk=     45 Scr=      1
```

### <center>🎉🎉 Great job! You just ran your first <kbd>Gaussian</kbd> optimization! 👏👏 </center>

<br>

In the next section, we'll go over how to examine the output file and parse it for the important thermochemical data.

<br />

| <center>Previous<br><a href="/dftCourse/ShortCourse/gaussianInputs.html"><kbd>Gaussian</kbd> Input Files</a></center> | <center><a href="/dftCourse/Introduction.html">Home</a></center> | <center>Next<br><a href="/dftCourse/ShortCourse/gaussianOutputs.html"><kbd>Gaussian</kbd> Output Files</a></center> |

<br>

#### References

(1) [<kbd>Gaussian</kbd> link 0](https://gaussian.com/link0/)  
(2) [Punched Card](en.wikipedia.org/wiki/Punched_card)  
(3) [The <kbd>Gaussian</kbd> `Punch` keyword](https://gaussian.com/punch/)  
