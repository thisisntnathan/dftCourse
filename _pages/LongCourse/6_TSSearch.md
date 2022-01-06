---
layout: page
title: The art of finding transition structures
short-title: Transition Structure Search
sub-title: The DFT Long Course
sidebar_link: true
sidebar_sort_order: 6
permalink: /LongCourse/transitionStructureSearch.html
---
<!-- markdownlint-disable-file MD040 -->

This lesson is not done yet

Finding transition structures is one of the most challenging tasks in computational organic chemistry; moreso than ground state optimizations since the resulting structures are, by definition, unstable ones. The process requires both careful setup of the problem as well as the dreaded chemical intuition.  

This lesson will focus on four methods of searching for transition structures. They will be presented in descending order of utility (i.e. the most likely to work to the least likely to work). Which means that you, dear user, should try this list from *top* to *bottom*.  

1. 2-Structure Synchronous Transit-Guided Quasi-Newton Method (QST2)  
2. TS guess with frozen bond approximations  
3. 3-Structure Synchronous Transit-Guided Quasi-Newton Method (QST3)  
4. Potential energy surface scan  

<br />

## 2-Structure Synchronous Transit-Guided Quasi-Newton Method (QST2)

The Synchronous Transit-Guided Quasi-Newton methods were developed by [Peng and Schlegel](http://dx.doi.org/10.1002/ijch.199300051) in the 90s.<sup>1</sup> These methods employ a linear or quatradic synchronous transit (L/Q-ST) approach to approximate the saddle region and then switch to a *quasi*-Newton search for the true transition structure. In a the synchronous transit approach a (linear or quadratic) path along the reaction coordinates is plotted and the energies along the path are sampled (**Fgiure 1a**). The highest energy structure along the path is then efficiently optimized to the transition structure (**Figure 1b**) using an approximate quadratic search (quasi-Newton).  

<center>
<div class="row">
  <div class="column">
    <img src='/dftCourse/assets/images/LC/qstlst.png' width='280'>
  </div>
  <div class="column">
    <img src='/dftCourse/assets/images/LC/qst2.png' width='310'>
  </div>
</div>
<b>Figure 1. a</b>| Linear (LST) and quadratic (QST) synchronous transit paths. <br><b>b</b>| Eigenvector search from QST maximum to the saddle point. Reproduced from <a href='hhttps://www.tau.ac.il/~ephraim/TranState.pdf' target='_blank'>ref 2</a>.
</center>

<br />

To set up a QST2 calculation you'll need two structures:

1. The reactant ensemble  
2. The product ensemble  

I call these *ensembles* because (unless you're studying a unimolecular to unimolecular reaction) these starting geometries will comprise of multiple fragments. They should be fully optimized; sometimes this requires a bit of fine tuning since <kbd>Gaussian</kbd> may want to kick off one of the reactants to make a more favorable "molecule", but persistence is key in this step. After optimizing the reactant and product ensembles you'll submit a <kbd>Gaussian</kbd> input with the following format:

```
%chk=jobName.chk      ! you want to save a checkpoint, can also do this with GAUSS_YDEF
#N M062X/def2svp OPT=QST2 Freq=(NoRaman) ...    ! all optimizations need a freq
<<Blank line>>        ! QST2 tells g16 that we want a transition structure search
First title section
<<Blank line>>
Molecule specification for the reactants
<<Blank line>>
Second title section
<<Blank line>>
Molecule specification for the products
...
```

>The atom numbers in both molecule specifications **must be the same** so that <kbd>Gaussian</kbd> can map the reaction coordinate atom by atom.

The starting guess for the transition structure is midway between the reactant and product geometries (which is why they must be optimized beforehand). Additionally, this allows you to calculate a $\Delta G^{\ddagger}_{rxn}$. In <kbd>Gaussian</kbd> the QST2 is the quickest path to a saddle point and typically works well. However, this method fails if the "average" geometry between reactants and products is chemically illogical (e.g., complex stereoinversions and isomerizations). If your calculation converges to a logical transition structure skip down to the [verification section](#verification).

## TS guess with frozen bond approximations



## 3-Structure Synchronous Transit-Guided Quasi-Newton Method (QST3)

The QST3 method 

## Potential energy surface scan  

## Verification of transition structures <a name="verification"></a>

<!-- TODO: add instructions for TS verification (freq and irc). 
    N.b. Must do vibrational frequency calc at the SAME level of theory as optimization -->

#### References

(1) C. Peng and H. B. Schlegel, “Combining Synchronous Transit and Quasi-Newton Methods for Finding Transition States,” [*Israel J. Chem.*, **1993**, *33*, 449.](http://dx.doi.org/10.1002/ijch.199300051)  
(2) [Transition States and Reaction Paths *for* Computational Chemistry Course *by* Prof. Ephraim Eliav](https://www.tau.ac.il/~ephraim/TranState.pdf)  
