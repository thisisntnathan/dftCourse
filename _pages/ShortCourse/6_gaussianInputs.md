---
layout: page
title: The <kbd>Gaussian</kbd> Input File
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 6
permalink: /ShortCourse/gaussianInputs.html
---
<!-- markdownlint-disable-file MD040 -->

<kbd>Gaussian16</kbd> (<kbd>g16</kbd>) input files are plain text files that end in `.com` or `.gjf`. They can be generated using a molecular modeling program like <kbd>GaussView</kbd>  or <kbd>Avogadro</kbd>  or in a simple text editor (provided one has the atomic coordinates already). The general format of a <kbd>Gaussian</kbd>  input file is given below accompanied by a short description of each section.

```
Link 0 commands                 ! Specifies memory, CPU, and other job information
Routing information             ! Specifies job parameters
<<<BLANK LINE>>> 
Title line                      ! Free-format comment line
<<<BLANK LINE>>>
Charge and Multiplicity         ! Space delimited
Molecule specification(s)       ! Cartesian, Z-matrix, or Redundant Internals
<<<BLANK LINE>>>
Job/option specific input       ! See g16 manual
<<<BLANK LINE>>>
<<<BLANK LINE>>>
```

### Formatting and syntax

In general, <kbd>Gaussian</kbd>  input files follow simple and flexible syntax and grammar [rules](https://gaussian.com/input/):<sup>1</sup>

- Inputs are case-insensitive i.e., opt, OPT, and oPt request the same optimization job  
- Comments are made using the exclamation point (`!`)  
- External files can be read into an input using the syntax: `@path/to/file`  
- Keywords and keyword options can be specified using the following options:  
  - keyword=option  
  - keyword(option)  
  - keyword=(option1, option2, ...)  
  - keyword(option1, option2, ...)  

### Link 0

Link 0 command lines begin with the percent `%` symbol and set program control options such as resource limits, whether to save and what to name scratch files, where to access old scratch files, etc…  The link 0 commands are detailed [here](https://gaussian.com/link0/).<sup>2</sup>  Each link 0 command requires its own line.  Every link 0 option can also be called as a command line flag in your shell script or passed to <kbd>Gaussian</kbd> as an environmental variable.  Equivalent commands and the corresponding precedence are detailed under the “Equivalencies” tab in the link above.

> This section (and everything else in the input file) is an instruction to <kbd>Gaussian</kbd> , not <kbd>SLURM</kbd>. The memory and CPU allocations specified in the input file should, ideally, be equal to those requested from <kbd>SLURM</kbd> (so that there is no "wasted" computing power), but must not be greater than those requested from <kbd>SLURM</kbd>, otherwise <kbd>Gaussian</kbd> will attempt to allocate more resources than has been made available to it which will lead to job failure.

#### The `.chk` file

A common link 0 command is `%chk`, used to save the checkpoint file from the calculation (typically deleted with other scratch files when the job is completed). Every iteration of a calculation <kbd>Gaussian</kbd> saves an image of job in the checkpoint file (like a savepoint) allowing the user to go back and restart a failed job. In addition to various savepoints, the checkpoint file also allows the user to view the molecular orbitals in <kbd>GaussView</kbd>.  Additionally, specifications (e.g. basis set, functional, molecular geometries, etc...) can be read from the checkpoint file for future jobs; however, this means  the `.chk` file can get very large (on the order of 100s of mb). Saving `.chk` files indiscriminately is unadvisable unless you have ample storage space.

### Routing information

The routing line of the <kbd>Gaussian</kbd>  job file begins with the pound/hash `#` symbol.  The letter following the `#` determines the level of output. The options are `#P`, `#N`, & `#T` which provide verbose, normal, and terse levels of description in the job’s output file.  The default option is `#N` (which can shortened to `#`). The options that follow are referred to by <kbd>Gaussian</kbd> as “[keywords](http://gaussian.com/keywords/)” and are responsible for setting up the requested calculation.<sup>3</sup>

The routing line continues with a method and basis set declaration in single-slash notation.  If no options are specified the default method is a Hartree-Fock calculation (HF) using the minimal Slater-type orbital basis functions (STO-3G), given as HF/STO-3G. g16's built in [DFT methods](https://gaussian.com/dft/) and [basis sets](https://gaussian.com/basissets/) are available in the documentation. The choice of functional and basis set are discussed further in a later section.<sup>4,5</sup>  

> With the advancement of computational chemistry and the availability of computing resources there is no reason to perform the default HF/STO-3G computation.  The most commonly used theory/basis combination in computational organic chemistry is the infamous Becke, 3-parameter, Lee-Yang-Parr (B3LYP) hybrid-exchange correlated functional used in combination with the 6-31g* basis set.  

The final required keyword in the routing line is the type of calculation.  Typical calculations include geometry optimizations (Opt), vibrational frequency analysis (Freq), single-point molecular energy calculations (SP), NMR shift calculations (NMR), etc… A full list of <kbd>g16</kbd>'s computational capabilities can be found [here](https://gaussian.com/capabilities/?tabid=1).<sup>6</sup> These job keywords can be specified on their own or with various options.  

A calculation minimally requires a functional, basis set, and the type of calculation; however, it is usually necessary to specify customizable options such as implicit solvation, temperature (for frequency analysis), initial guesses, counterpoise correction, empirical dispersion correction, extra basis functions, effective core potentials (i.e., pseudopotentials), etc…  These options are specified from the routing line using their [respective keyword options](http://gaussian.com/keywords/).<sup>3</sup>  The full list of <kbd>Gaussian</kbd> keywords and their availbe options can be found in the [g16 documentation](http://gaussian.com/man/).<sup>7</sup>

Keywords can be specified in any order.  The routing section is fully reproduced in the output file and terminated by a blank line.

#### Integration grid size

All DFT methods implemented in <kbd>Gaussian</kbd> involve a grid-based numerical integration of the functional (or its derivatives). Consequently, the accuracy of a DFT calculation is dependent on the resolution of the integration grid. In <kbd>Gaussian16</kbd> the default integration for all DFT functionals is calculated over a pruned grid with 99 radial shells and 590 angular points, denoted (99,590), and specified by the keyword `Integral(grid=ultrafine)`.  This is sufficient for the vast majority of all calculations. Using a smaller grid, while faster, is [not recommended](https://gaussian.com/dft/) for most DFT calculations. Lastly, energy comparisons must be done using the **same grid size**.<sup>4</sup>  

### Job title

The title/comment line is plaintext that is reproduced in the <kbd>Gaussian</kbd> output file and terminated by a blank line.

### Charge and multiplicity

The charge and multiplicity of the system is given before the molecule specification in standard convention separated by a space.  For example, `-1 1` describes an anionic singlet state.

### Molecule specification

The molecule specification can be given in either standard cartesian (xyz) or internal (Z-matrix) coordinates. Note that specification in one coordinate system or another will not dictate what coordinates will be used to perform the optimization itself which defaults to <kbd>Gaussian</kbd>'s redundant internal coordinates, but can be changed by specifying an option in the Opt keyword.

This section may be omitted altogether if reading the start geometry from a pre-existing file (such as a checkpoint (`.chk`) file) using the `geom=checkpoint` option.

The molecule specification is terminated by a blank line.

### Job/keyword options

Some keyword options require additional input (such as specifying ECPs or additional basis functions). This additional information is placed after the molecule specification and each individual keyword’s specifications are terminated by blank lines.

The input file is terminated by two blank lines.

### A simple <kbd>g16</kbd>  input file

Below is a fully functional <kbd>Gaussian</kbd>  input file for a single point molecular energy calculation. Copy + paste it into a text editor and save it as `coolMolecule.com` and see if you can figure out what we're trying to do in the next section. (Hint: open it up in your favorite molecular editor/viewer)

```
%NPROCSHARED=16                        ! run on 16 parallel cores
%MEM=28GB                              ! run with 28 gb memory
%Chk=coolMolecule.chk                  ! save a checkpoint file
#N M062X/def2tzvp SP                   ! calculate single-point energy

My cool molecule

0 1                                    ! neutral singlet
C                                      ! starting geometry for
C 1 B1                                 ! a molecule specified in
C 2 B2 1 A1                            ! gaussian internal coordinates
C 3 B3 2 A2 1 D1 0
C 4 B4 3 A3 2 D2 0                     ! to save space bond lengths,
C 1 B5 2 A4 3 D3 0                     ! angles, and dihedrals are all
H 3 B6 2 A5 1 D4 0                     ! specified as variables
H 2 B7 1 A6 6 D5 0
H 1 B8 6 A7 5 D6 0
H 1 B9 6 A8 5 D7 0
H 4 B10 3 A9 2 D8 0
H 4 B11 3 A10 2 D9 0
H 5 B12 4 A11 3 D10 0                  ! g16 doesn't support comments
H 5 B13 4 A12 3 D11 0                  ! in the molecule specification
H 6 B14 1 A13 2 D12 0                  ! so get rid of these comments
H 6 B15 1 A14 2 D13 0                  ! before you try to run this job
H 3 B16 2 A15 1 D14 0
C 2 B17 1 A16 6 D15 0
H 18 B18 2 A17 1 D16 0
H 18 B19 2 A18 1 D17 0
H 18 B20 2 A19 1 D18 0

   B1             1.51510600
   B2             1.51517951
   B3             1.51543497
   B4             1.51512522
   B5             1.51498514
   B6             1.12087116
   B7             1.12176819
   B8             1.12177478
   B9             1.12093060
   B10            1.12176062
   B11            1.12091101
   B12            1.12095758
   B13            1.12181557
   B14            1.12176019
   B15            1.12097986
   B16            1.12168148
   B17            1.54000000
   B18            1.07000000
   B19            1.07000000
   B20            1.07000000
   A1           111.36252447
   A2           111.24127560
   A3           111.26574203
   A4           111.30936835
   A5           109.59001104
   A6           109.39681635
   A7           109.40740597
   A8           109.57492433
   A9           109.41106707
   A10          109.58676541
   A11          109.55885178
   A12          109.38713435
   A13          109.41082787
   A14          109.56859847
   A15          109.42519797
   A16          109.55953690
   A17          109.47120255
   A18          109.47120255
   A19          109.47123134
   D1            55.25714299
   D2           -55.23663791
   D3           -55.19282652
   D4           176.59526965
   D5            65.84971766
   D6           -65.94975705
   D7           176.44368868
   D8            65.79362957
   D9          -176.57422515
   D10          176.61579480
   D11          -65.80639557
   D12          -65.96332262
   D13          176.42532586
   D14          -65.75731388
   D15         -176.56184324
   D16         -178.76723016
   D17          -58.76721537
   D18           61.23277724


```

<br />

| <center>Previous<br><a href="/dftCourse/ShortCourse/slurmScripts.html"><kbd>SLURM</kbd> Basics</a></center> | <center><a href="/dftCourse/introduction.html">Home</a></center> | <center>Next<br><a href="/dftCourse/ShortCourse/firstJob.html">My First <kbd>Gaussian</kbd> Job</a></center> |

<br />

#### References

(1) [Gaussian input file syntax](https://gaussian.com/input/)  
(2) [Link 0 commands](https://gaussian.com/link0/)  
(3) [Gaussian keyword list](http://gaussian.com/keywords/)  
(4) [DFT methods](https://gaussian.com/dft/)  
(5) [Basis sets](https://gaussian.com/basissets/)  
(6) [Gaussian capabilities](https://gaussian.com/capabilities/?tabid=1)  
(7) [Gaussian16 documentation](http://gaussian.com/man/)
