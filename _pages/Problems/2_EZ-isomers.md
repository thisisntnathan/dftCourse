---
layout: page
title: E-Z isomers of <br> 3-(4-nitrophenyl)but-2-en-2-yl triflate
short-title: The E-Z isomer problem
sub-title: Problems
sidebar_link: true
sidebar_sort_order: 2
permalink: /Problems/ezIsomers.html
---
<!-- markdownlint-disable-file MD040 -->

Your advisor wants you to compute ground state energies for the two isomers of 3-(4-nitrophenyl)but-2-en-2-yl triflate. First, run a standard optimization/frequency calculation for the isomers at the `M062X/def2svp` level of theory. As always, you can check your work at [the code repo](https://github.com/thisisntnathan/dftCourseCodeRepo).  

<center>
    <img src="/dftCourse/assets/images/Problems/ez_triflate.svg" width="500" height="120.33">
</center>

### 1) Specifying different built-in basis sets for different atoms

You take your results back to your advisor who doesn't seem very satisfied. They tell you to make sure the substituents are right by beefing up the basis set on the heteroatoms.  

Modify your calculations to use the triple $\zeta$ basis set `def2tzvp` on all heteroatoms and `def2zvp` on C and H. Give it a shot before checking your input files against those at [the code repo](https://github.com/thisisntnathan/dftCourseCodeRepo).  

### 2) Assigning built-in basis sets to individual atoms

Still unhappy with the results, your advisor tells you to re-run the computations, but this time placing diffuse basis functions on **just the nitro group atoms**. The basis set `def2tzvpd` which has the diffuse functions added to `def2tzvp` isn't built in to <kbd>Gaussian</kbd> so you figure that another similarly large basis set with diffuse functions `aug-cc-pvtz` that is built-in would work just as well.  

Run your computations again, this time use `aug-cc-pvtz` to describe the N and *only the two O of the nitro group*. Keep everything else the same i.e.,  

- C H: `def2svp`  
- S F O(trifyl): `def2tzvp`  
- N O(nitro): `aug-cc-pvtz`  

### 3) Incorporating external basis sets into <kbd>Gaussian</kbd> calculations

After a few second guesses, you're unsure of whether or not `aug-cc-pvtz` is really a suitable substitute for `def2tzvpd`; you decide to try using `def2tzvpd` instead. Re-run your calculations, this time use `def2tzvpd` instead of `aug-cc-pvtz` to describe the N and *only the two O of the nitro group*. Keep everything else the same i.e.,  

- C H: `def2svp`  
- S F O(trifyl): `def2tzvp`  
- N O(nitro): `def2tzvpd`  

Remember that `def2tzvpd` is not built into <kbd>g16</kbd> so you'll have to get the basis set from the [Basis Set Exchange](https://www.basissetexchange.org/).$^1$ There are two ways to accomplish this task; see if you can figure them both out before going to [the code repo](https://github.com/thisisntnathan/dftCourseCodeRepo)!  

Hint: These are the diffuse functions from `def2tzvpd`  

```
N 0
S    1   1.00
      0.68441605847D-01      1.0000000
D    1   1.00
      0.12829642058          1.0000000
****
O 0
S    1   1.00
      0.70288026270D-01      1.0000000
P    1   1.00
      0.51112745706D-01      1.0000000
D    1   1.00
      0.14696477366          1.0000000
```

<!-- ### Key takeaways -->

<!-- TODO: Put in key takeaways once energies are done -->
<!-- TODO: Add compute time table here (once job finish)  -->

<br>

| <center><a href="/dftCourse/introduction.html">Home</a></center> |

<br>

#### Resources

(1) [Basis Set Exchange](https://www.basissetexchange.org/)  
(2) Vinyl cations. 12. Mechanism of reaction of cis- and trans-3-phenyl-2-buten-2-yl triflates. Evidence for vinylidene phenonium ions by Peter J. Stang and Thomas E. Dueber [*J. Am. Chem. Soc.* **1977**, *99* (8), 2602](https://pubs.acs.org/doi/abs/10.1021/ja00450a033)
