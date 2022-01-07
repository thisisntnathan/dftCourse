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
%chk=jobName.chk     ! you want to save a checkpoint, can also do this with GAUSS_YDEF
#N M062X/def2svp OPT=QST2 Freq=(NoRaman) ...    ! all optimizations need a freq
<<Blank line>>       ! QST2 tells g16 that we want a transition structure search
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

The starting guess for the transition structure is midway between the reactant and product geometries (which is why they must be optimized beforehand). Additionally, this allows you to calculate a $\Delta G^{\ddagger}_{rxn}$. In <kbd>Gaussian</kbd> the QST2 is the quickest path to a saddle point and typically works well. However, this method fails if the "average" geometry between reactants and products is chemically illogical (e.g., complex stereoinversions and isomerizations). If your calculation converges to a logical transition structure skip down to the [verification section](#verification). Otherwise, try a TS guess!  

## TS guess with frozen bond approximations

So the QST2 didn't work (either because it failed or the algorithm didn't find the correct structure). This next method has two parts:

1. Build a model of the most likely transition structure (this is where you chemical intuition is needed) and optimize all the atoms that don't participate in the reaction.  
2. Re-optimize the transition structure now with all atoms free.  

>One could ask, why we're not trying the QST3 method mentioned below. QST3 requires a "best guess" on the transition structure, which is what we'll make in the first step of this method. So if we'll need to partially optimize a best guess anyways, we might as well attempt a direct optimization of the transition structure.

Keep in mind, designing a transition structure by hand is no trivial task; bond lengths, angles, and dihedrals are all significant factors. Furthermore, **the saddle point you find is *sensitive* to the initial guess**. Whether or not you correctly identify the true saddle structure is wholely dependent on whether or not you start with the correct geometry.  

A transition structure search is called using the keyword `OPT=TS`, we'll also use the additional options to help guide the calculation to convergence:

- `CalcFC`: Calculates the force constants for the first structure  
- `NoEigenTest`: Suppresses testing of the eigenvalues during the optimization (common cause of failure)  
- `ModRedundant`/`AddGIC`: Allows us to freeze bond lengths/positions during the optimization

```
%chk=jobName.chk     ! you want to save a checkpoint, can also do this with GAUSS_YDEF
#N M062X/def2svp OPT=(TS,CalcFC,NoEigenTest,AddGIC/ModRedundant) Freq=(NoRaman) ...
<<Blank line>>       ! don't forget: all optimizations need a freq
Title section
<<Blank line>>
Molecule specification
<<Blank line>>
Coordinates of active atoms
...
```

At the end of this first optimization you'll need to tell <kbd>Gaussian</kbd> which bonds to freeze during the optimization. If you're optimizing using <kbd>Gaussian</kbd>'s **redundant internal coordinates** (the default) then this last section will take the form: `B [Atom 1 number] [Atom 2 number] F` where atoms 1 and 2 will be frozen in the geometry optimization. For example, `B 74 94 F` freezes the bond (length and position) between atom 74 and 94.  

If you're optimizing using the new **Generalized Internal Coordinates (GICs)** the section will look like: `CoordinateName(freeze)=R(Atom 1 number,atom 2 number)` where atoms 1 and 2 will be frozen in the geometry optimization. For example, `BrC(freeze)=R(74,94)` freezes the coordinate between atoms 74 and 94. In  this method each frozen coordinate must have its own unique name.  

Once the optimization has converged (if it does converges, see note below), reoptimize the structure using the same routing line, but without `AddGIC/ModRedundant` and the coordinate section at the end.

>N.b. This optimization may not converge, sometimes the first step doesn't end up finding a true minimum because the frozen coordinates are frozen in such a way that they prevent the convergence criteria from being met. You have to keep an eye on these calculations in case they end up oscillating between two similar structures. If this happens, just cancel the job and use one of these structures as the starting point for the next optimization.  

If this calculation converges then skip down to the [verification section](#verification). Of course, there's always the possibility that the molecule relaxes down to the product or reactant geometries at this stage (this happens more often than you'd like). This is typically an indicator that your initial transition state guess is incorrect. In this case, start from the result of the first optimization step and tweak the active atoms (bond lengths, angles, etc...) in the starting geometry for the second optimization. This method can be fickle, but when it does work it's quite elegant. If you've tried several times and still have no luck, proceed to the QST3 method in the next section.  

## 3-Structure Synchronous Transit-Guided Quasi-Newton Method (QST3)

<!-- The QST3 method uses the same search algorithm as QST2 but  -->

## Potential energy surface scan  

## Verification of transition structures <a name="verification"></a>

<!-- TODO: add instructions for TS verification (freq and irc). 
    N.b. Must do vibrational frequency calc at the SAME level of theory as optimization -->

#### References

(1) C. Peng and H. B. Schlegel, “Combining Synchronous Transit and Quasi-Newton Methods for Finding Transition States,” [*Israel J. Chem.*, **1993**, *33*, 449.](http://dx.doi.org/10.1002/ijch.199300051)  
(2) [Transition States and Reaction Paths *for* Computational Chemistry Course *by* Prof. Ephraim Eliav](https://www.tau.ac.il/~ephraim/TranState.pdf)  
