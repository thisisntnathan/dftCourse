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

>Do not wait for these calculations to finish. Even running on the cluster, they will take a while. The best thing to do is to submit them and then go set up a reaction or something. If you're running on the `chem` nodes you can expect these jobs to take around 1 to 2 hours.  

### 1) Specifying different built-in basis sets for different atoms

You take your results back to your advisor who doesn't seem very satisfied. They tell you to make sure the substituents are right by beefing up the basis set on the heteroatoms.  

Modify your calculations to use the triple $\zeta$ basis set `def2tzvp` on all heteroatoms and `def2svp` on C and H. Give it a shot before checking your input files against those at [the code repo](https://github.com/thisisntnathan/dftCourseCodeRepo).  

### 2) Assigning built-in basis sets to individual atoms

Still unhappy with the results, your advisor tells you to re-run the computations, but this time placing diffuse basis functions on **just the nitro group atoms**. The basis set `def2tzvpd` which has the diffuse functions added to `def2tzvp` isn't built in to <kbd>Gaussian</kbd> so you figure that another similarly large basis set with diffuse functions `aug-cc-pvtz` that is built-in would work just as well.  

Run your computations again, this time use `aug-cc-pvtz` to describe the N and *only the two O of the nitro group*. Keep everything else the same i.e.,  

- C H: `def2svp`  
- S F O(trifyl): `def2tzvp`  
- N O(nitro): `aug-cc-pvtz`  

### 3) Incorporating external basis sets into <kbd>Gaussian</kbd> calculations

After a few second guesses, you're unsure of whether or not `aug-cc-pvtz` is really a suitable substitute for `def2tzvpd`; you also realize that your computations are taking quite a while, and the other group members are starting to get upset that you're hogging the new compute nodes. You decide to try using `def2tzvpd` instead. Re-run your calculations, this time use `def2tzvpd` instead of `aug-cc-pvtz` to describe the N and *only the two O of the nitro group*. Keep everything else the same i.e.,  

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

### Key takeaways

<!-- TODO: Put in key takeaways once energies are done -->

Once your jobs have finished, extract the corrected energies from your results. I've placed mine in the table below if you're just following along (the input/output files are available in [the code repo](https://github.com/thisisntnathan/dftCourseCodeRepo)).  

Using these energies can you justify the product distribution observed in the triflation of 3-(*p*-nitro)phenyl-2-butanone (products 5g/6g) in [this paper](https://pubs.acs.org/doi/abs/10.1021/ja00450a033)?<sup>2</sup>  

| Basis Set | Isomer | Energy<br>/ kcal mol<sup>-1</sup> | ΔG(E<->Z)<br>/ kcal mol<sup>-1</sup> | Total Computation Time<br>/ min |
|:---:|:---:|:---:|:---:|:---:|
| def2svp(all) | *E*<br>*Z* | -973863.597<br>-973862.450 | 1.147 | 37<br>47 |
| def2tzvp (SNOF)<br>def2svp (CH) | *E*<br>*Z* | -974540.402<br>-974539.491 | 0.911 | 87<br>92 |
| aug-cc-pvtz (nitro)<br>def2tzvp (trifyl)<br>def2svp (CH) | *E*<br>*Z* | -974540.579<br>-974539.693 | 0.886 | 120<br>134 |
| def2tzvpd (nitro)<br>def2tzvp (trifyl)<br>def2svp (CH) | *E*<br>*Z* | -974542.096<br>-974541.205 | 0.891 | 99<br>110 |

<br>

There are some key takeaway from the data above:

1. The answer never formally changes. In all cases the *E* isomer, as we expect, is more stable than the *Z* isomer.  
2. The caveat is that depending on our choice of basis set,  we **do** see changes in the relative energies of the two species; namely, the relative energies tend converge with increasing basis set size.  
3. Nevertheless, its important to not lose sight of the forrest in the trees. Look again in the predicted relative energies. In the "worst" case we there is a 1.15 kcal mol<sup>-1</sup> difference between the isomers; in the "best" case, only 0.89 kcal mol<sup>-1</sup>.  The difference in these two predictions is a mere 0.25 kcal mol<sup>-1</sup>; it is simple to use this as justification for more computationally intensive calculations, however consider for a second the *experimental* implications of this value.  
A reaction under control of a 1.15 kcal mol<sup>-1</sup> ΔΔG would predict 11% minor product formation, while one with a 0.89 kcal mol<sup>-1</sup> ΔΔG would predict 16% of the minor product; barely something to split hairs over.  
4. Computational time, while relatively cheap, is not free. The difference in relative energy that comes from using the diffuse augmented basis sets is a whopping 0.025 kcal mol<sup>-1</sup> (or *25 thousandths of a kcal*). If this number seems small to you now, consider it in the context of the computational time.  
Augmenting *just three atoms* in our molecule with the diffuse functions of `aug-cc-pvtz` increased our total computational time from 87 min to 120 min, a 40% increase in resources. All for 0.025 kcal mol<sup>-1</sup>. Realizing that a ΔΔG of 0.025 kcal mol<sup>-1</sup> erodes a selectivity by less than 1%, it seems a little silly. Note that DFT scales in cubic time ($\mathcal{O}(n_e^3) $) with respect to the number of electrons in your system so as the size of your molecule increases this issue will only get much worse.<sup>3</sup> Take a look at [this paper](https://pubs.acs.org/doi/10.1021/jp312755z) for a discussion on the necessity of diffuse functions.<sup>4</sup>  

<br>

>Computational chemistry is all about choosing which assumptions to make because all models must make assumptions, i.e., **there is no free lunch**. In the most rigorous sense we can, we are always searching for the *good enough* method that balances *chemical accuracy* with *computational cost*.

<br>

| <center><a href="/dftCourse/introduction.html">Home</a></center> |

<br>

#### Resources

(1) [Basis Set Exchange](https://www.basissetexchange.org/)  
(2) Vinyl cations. 12. Mechanism of reaction of cis- and trans-3-phenyl-2-buten-2-yl triflates. Evidence for vinylidene phenonium ions by Peter J. Stang and Thomas E. Dueber [*J. Am. Chem. Soc.* **1977**, *99* (8), 2602](https://pubs.acs.org/doi/abs/10.1021/ja00450a033)  
(3) [Max Hutchinson on CompSci Stack Exchange](https://scicomp.stackexchange.com/questions/5515/how-does-density-functional-theory-scale-with-system-size)  
(4) Is the Use of Diffuse Functions Essential for the Properly Description of Noncovalent Interactions Involving Anions? by Antonio Bauzá, David Quiñonero, Pere M. Deyà, and Antonio Frontera [*J. Phys. Chem. A* **2013**, *117* (12), 2651](https://pubs.acs.org/doi/10.1021/jp312755z)  
