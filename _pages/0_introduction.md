---
layout: page
title: A gentle introduction to computational chemistry and density functional theory
sub-title: The DFT Short Course
sidebar_link: false
permalink: /introduction.html
---
<!-- markdownlint-disable-file MD026 -->

<center>
    <h4>Written by Nathan M. Lui (2023) <br>
    With contributions from Dr. Ryan A. Woltornist (2021), <br>
    Dr. Kyle Mack (2018), and Dr. Russell F. Algera (2017) </h4>
</center>

<br />

### The Short Course

The *Short Course* is designed as a primer for advanced undergraduates and beginning graduate students. It is intended to give the completely inexperienced reader a step-by-step guide to running electronic structure calculations on the AS-CHEM computing cluster at Cornell University, but it is our hope that these instructions are easily generalizable to other computing clusters.  

[Software](/dftCourse/ShortCourse/software.html)  
[Linux basics](/dftCourse/ShortCourse/linuxBasics.html)  
[My first script](/dftCourse/ShortCourse/firstScript.html)  
[<kbd>SLURM</kbd> basics](/dftCourse/ShortCourse/SLURM.html)  
[My first <kbd>SLURM</kbd> job](/dftCourse/ShortCourse/slurmScripts.html)  
[The <kbd>Gaussian</kbd> input file](/dftCourse/ShortCourse/gaussianInputs.html)  
[My first <kbd>Gaussian</kbd> job](/dftCourse/ShortCourse/firstJob.html)  
[Understanding the <kbd>Gaussian</kbd> output file](/dftCourse/ShortCourse/gaussianOutputs.html)  
[Putting it all together: calculating cyclohexane A-values](/dftCourse/ShortCourse/aValues.html)  

### The Long Course

#### The Long Course is coming soon!

The *Long Course* is a set of more advanced topics in scripting and computational chemistry. It was written for those who have, or would like to, incorporate more advanced calculations/models into their research. The topics start by the computational streamlining workflow (with <kbd>bash</kbd> scripting) and then progress towards the more under-the-hood options of computational engines.  

Using <kbd>bash</kbd> to streamline computational workflow and data processing  
Cost efficiency: Selecting basis sets and functionals without going overboard  
Using custom basis sets and pseudopotentials  
The ~~science~~ art of finding transition structures  
Transition structure search using QST methods  
The implicit/explicit solvation war  
Electronic structure theory
<!-- 
[(More) advanced job scripting with <kbd>bash</kbd>](/dftCourse/LongCourse/bashScripting.html)  
[Cost efficiency: Selecting basis sets and functionals without going overboard](/dftCourse/LongCourse/basisSets.html)  
[Transition structure search using QST methods](/dftCourse/LongCourse/QST.html)  
[The implicit/explicit solvation war](/dftCourse/LongCourse/solvationModels.html)  
[Electronic structure theory](/dftCourse/Tutorials/CompChem/9_introToEST.html)   
-->

### Problems

My least favorite math teacher would always say that the only way to learn calculus is to solve lots of calculus problems. This is a collection of case studies and practice problems that you can use to try your own hand at computational chemistry.  [Let me know](mailto:nml64@cornell.edu) if there's anything else you'd like to see here!  

[Cyclohexane A-values (from the Short Course)](/dftCourse/ShortCourse/aValues.html)  
[The Smelly Dimer Problem](/dftCourse/problems/cpDimer.html)  

### Resources

[The Code Repo: Exercises and Problems](https://github.com/thisisntnathan/dftShortCourseFiles)  
<!-- Best practices   -->
[g16 “cheat codes” (routing line templates)](/dftCourse/resources/cheatCodes.html)  
<!-- Common error codes   -->
[A collection of papers/resources I've amassed over the years.](/dftCourse/resources/links.html)

### Further Readings

The *International Journal of Quantum Chemistry* has published an [excellent series of tutorial reviews](https://onlinelibrary.wiley.com/journal/1097461x) for novices and professionals alike. I highly recommend looking through them. Below are a few you may find particularly helpful.  

Fourteen Easy Lessons in Density Functional Theory  
by John P. Perdew and Adrienn Ruzsinszky  
[*Int. J. Quantum Chem.* **2012**, *110* (15), 2801](https://doi.org/10.1002/qua.22829)  
DFT in a nutshell  
by Kieron Burke and Lucas O. Wagner  
[*Int. J. Quantum Chem.* **2013**, 113 (2), 96](https://doi.org/10.1002/qua.24259)  
The devil in the details: A tutorial review on some undervalued aspects of density functional theory calculations by Pierpaolo Morgante and Roberto Peverati  
[*Int. J. Quantum Chem.* **2020**, *120* (18), e26332](https://doi.org/10.1002/qua.26332)
