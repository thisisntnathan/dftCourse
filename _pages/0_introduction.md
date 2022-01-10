---
layout: page
title: A gentle introduction to computational chemistry and density functional theory
sub-title: The DFT Short Course
sidebar_link: false
permalink: /introduction.html
---
<!-- markdownlint-disable-file MD026 -->

<center>
    <h4>Written by <a href='https://thisisntnathan.github.io' target='_blank'>Nathan M. Lui</a> (2023) <br>
    With contributions from Dr. Ryan A. Woltornist (2021) <br>
    and many others; see the full list of contributors in the <a href='https://github.com/thisisntnathan/dftCourse#acknowledgements' target='_blank'>GitHub repo</a></h4>
</center>

<br />

## The Short Course

The *Short Course* is designed as a primer for advanced undergraduates and beginning graduate students. It is intended to give the completely inexperienced reader a step-by-step guide to running electronic structure calculations on the AS-CHEM computing cluster at Cornell University, but it is our hope that these instructions are easily generalizable to other computing clusters. The Short Course's main computational engine is <kbd>Gaussian16</kbd> since it was originally designed for Collum group members, however a *fully open-source* edition (utilizing [<kbd>Psi4</kbd>](https://psicode.org/)) is currently being written.  

[Software](/dftCourse/ShortCourse/software.html)  
[Linux basics](/dftCourse/ShortCourse/linuxBasics.html)  
[My first script](/dftCourse/ShortCourse/firstScript.html)  
[<kbd>SLURM</kbd> basics](/dftCourse/ShortCourse/slurm.html)  
[My first <kbd>SLURM</kbd> job](/dftCourse/ShortCourse/slurmScripts.html)  
[The <kbd>Gaussian</kbd> input file](/dftCourse/ShortCourse/gaussianInputs.html)  
[My first <kbd>Gaussian</kbd> job](/dftCourse/ShortCourse/firstJob.html)  
[Understanding the <kbd>Gaussian</kbd> output file](/dftCourse/ShortCourse/gaussianOutputs.html)  
[Putting it all together: calculating cyclohexane A-values](/dftCourse/ShortCourse/aValues.html)  

## The Long Course

#### The Long Course is coming soon!

The *Long Course* is a set of more advanced topics in scripting and computational chemistry. It was written for those who have, or would like to, incorporate more advanced calculations/models into their research. The topics start by streamlining computational workflow (with <kbd>bash</kbd> scripting) and then progress towards the more under-the-hood options of computational engines.  

Using <kbd>bash</kbd> to streamline computational workflow and data processing  
So what exactly is an optimization?  
Cost efficiency: Selecting basis sets and functionals without going overboard  
[Using custom basis sets and effective core potentials](/dftCourse/LongCourse/gen.html)  
[What is a transition state?](/dftCourse/LongCourse/transitionState.html)  
[The art of finding transition structures](/dftCourse/LongCourse/transitionStructureSearch.html)  
The implicit/explicit solvation war  
Electronic structure theory

<!-- TODO: These pages:
[(More) advanced job scripting with <kbd>bash</kbd>](/dftCourse/LongCourse/bashScripting.html)  
[What is an optimization?](/dftCourse/LongCourse/optimization.html)  
[Cost efficiency: Selecting basis sets and functionals without going overboard](/dftCourse/LongCourse/basisSets.html)   
[The implicit/explicit solvation war](/dftCourse/LongCourse/solvationModels.html)  
[Electronic structure theory](/dftCourse/Tutorials/CompChem/9_introToEST.html)   
-->

## Problems

My least favorite math teacher would always say that the only way to learn calculus is to solve lots of calculus problems. This is a collection of case studies and practice problems that you can use to try your own hand at computational chemistry. [Let me know](mailto:nml64@cornell.edu) if there's anything else you'd like to see here!  

[Cyclohexane A-values (from the Short Course)](/dftCourse/ShortCourse/aValues.html)  
[The Smelly Dimer Problem](/dftCourse/Problems/cpDimer.html)  
[E-Z isomers of 3-(4-nitrophenyl)but-2-en-2-yl triflate](/dftCourse/Problems/ezIsomers.html)

## Resources

[The Code Repo: Exercises and Problems](https://github.com/thisisntnathan/dftCourseCodeRepo)  
<!-- Best practices   -->
[g16 “cheat codes” (routing line templates)](/dftCourse/Resources/cheatCodes.html)  
<!-- Common error codes   -->
[A collection of papers/resources I've amassed over the years.](/dftCourse/Resources/links.html)

## Contributions

If there's something you'd like to see or add to the course check out the [GitHub repo](https://github.com/thisisntnathan/dftCourse#so-you-want-to-contribute)!

## Supplemental Readings

The *International Journal of Quantum Chemistry* has published an [excellent series of tutorial reviews](https://onlinelibrary.wiley.com/journal/1097461x) for novices and professionals alike. I highly recommend looking through them. Below are a few you may find particularly helpful.  

Fourteen Easy Lessons in Density Functional Theory  
by John P. Perdew and Adrienn Ruzsinszky  
[*Int. J. Quantum Chem.* **2012**, *110* (15), 2801](https://doi.org/10.1002/qua.22829)  
DFT in a nutshell  
by Kieron Burke and Lucas O. Wagner  
[*Int. J. Quantum Chem.* **2013**, 113 (2), 96](https://doi.org/10.1002/qua.24259)  
The devil in the details: A tutorial review on some undervalued aspects of density functional theory calculations by Pierpaolo Morgante and Roberto Peverati  
[*Int. J. Quantum Chem.* **2020**, *120* (18), e26332](https://doi.org/10.1002/qua.26332)
