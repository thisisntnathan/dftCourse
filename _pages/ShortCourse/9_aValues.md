---
layout: page
title: Calculating cyclohexane A-values
short-title: Calculating A-values
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 9
permalink: /ShortCourse/aValues.html
---
<!-- markdownlint-disable-file MD040 -->

The [A-value](https://goldbook.iupac.org/terms/view/A00012) of a substituent is the energy of the axial cyclohexane conformer relative to the equatorial conformer (i.e., the *isomerization energy*). In organic conformational analysis the A-value is used as the archetypal steric parameter.  

<center>
    <img src="/dftCourse/assets/a-value.png" width="396" height="77.9">
</center>

<br>

In [our last exercise](/dftCourse/ShortCourse/firstJob.html) we calculated the energy of equatorial methylcyclohexane:

```
eqMeCyhex:
SCF energy: E(RM062X)                           =     -274.841184 Eh
Thermal correction to Gibbs Free Energy         =        0.171472 Eh
--------------------------------------------------------------------------
E(RM062X) + Thermal correction                  =     -274.669748 Eh
Sum of electronic and thermal Free Energies     =     -274.669748 Eh
```

Let's see if you can do the same with the axial conformer. Take a quick break and see if you can set up and execute this calculation on your own.  
If you're just following along or get stuck feel free to grab the files from the [code repo](https://github.com/thisisntnathan/dftCourseCodeRepo).  

```
axMeCyhex:
SCF energy: E(RM062X)                           =     -274.838661 Eh
Thermal correction to Gibbs Free Energy         =        0.171734 Eh
--------------------------------------------------------------------------
E(RM062X) + Thermal correction                  =     -274.666928 Eh
Sum of electronic and thermal Free Energies     =     -274.666928 Eh
```

Now, you're probably not a physical chemists if you're on this page, so let's convert these numbers to a more common unit and calculate our A-value:

```
eqMeCyhex:
E(RM062X) + Thermal correction                  =     -172355.27 kcal/mol
axMeCyhex:
E(RM062X) + Thermal correction                  =     -172353.50 kcal/mol
--------------------------------------------------------------------------
A-value = deltaG = E(axMeCyhex) - E(eqMeCyHex)  =     1.77 kcal/mol
```

So we get a relative energy of 1.77 kcal mol<sup>-1</sup>, which is in excellent agreement with the [literature values](https://organicchemistrydata.org/hansreich/resources/fundamentals/?page=a_values/) for the A-value of a methyl group.<sup>2</sup>  

>These are the kinds of comparisons that underscore much of computational organic chemistry. Even computations of complex mechanistic pathways are reducible to calculations of relative energies.

For more practice, try calculating other A-values and checking them with their [experimental values](https://organicchemistrydata.org/hansreich/resources/fundamentals/?page=a_values/). Then, when you feel like you're ready, give [this problem](/dftCourse/Problems/cpDimer.html) a shot.

| <center>Previous<br><a href="/dftCourse/ShortCourse/firstJob.html">My First <kbd>Gaussian</kbd> Job</a></center> | <center><a href="/dftCourse/introduction.html">Home</a></center> |

<br>

#### References

(1) [IPUAC Gold Book: A-value](https://goldbook.iupac.org/terms/view/A00012)  
(2) [The Reich Collection: A-values](https://organicchemistrydata.org/hansreich/resources/fundamentals/?page=a_values/)
