---
layout: page
title: Understanding the <kbd>Gaussian</kbd> output file
short-title: The <kbd>Gaussian</kbd> output file
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 8
permalink: /ShortCourse/gaussianOutputs.html
---

For easier analysis, let's drag all of our files back onto our personal laptop using FileZilla. (If you want you can try to read the <kbd>Gaussian</kbd> `.log` file in the terminal, but you'll soon see why that's not going to scale well.)

<center>
    <img src="/assets/7_1.png" width="641.5" height="462.5">
</center>

We won't use the `fort.7` file and `eqMeCyhex_oe` has a total size of 0 bytes (i.e., there's nothing written in it) so for the sake of cleanliness we can delete those. First, let's look at our optimized structure to make sure the final geometry makes chemical sense. Open `eqMeCyhex.log` in <kbd>GaussView</kbd> (download it from the [code repo](https://github.com/thisisntnathan/dftShortCourseFiles/blob/370b1efe3fbc332fc7beef72050ffe52b357fcf5/eqMeCyclohexane/eqMeCyhex.log) if you're not following along).  

<div class="message">
    If you're using <kbd>GaussView5</kbd> with <kbd>Gaussian16</kbd> you'll most likely run into this error:
        <center>
            <img src="/assets/GLOG_Parse_Error.png" width="300" height="124.5">
        </center>
    Don't panic, this issue occurs because <kbd>g16</kbd> writes some extra information in the output file that <kbd>GaussView5</kbd> doesn't know how to handle. Use <a href="https://gist.github.com/thisisntnathan/2aab4e51f1887c2e41feb71b081d5a7f">this script</a> provided by Dr. Davor Šakić from the University of Zagreb to generate a output file that <kbd>GaussView5</kbd> can read.
</div>

<kbd>GaussView</kbd> shows us a perfectly normal equatorial methylcyclohexane.

<center>
    <img src="/assets/eqMeCyhex.png" width="414.6" height="246.6">
</center>

You should always check your structures to make sure they are generally expected since not all *mathematical* solutions are *physical* ones. Sometimes our jobs will give us chemically nonsensical solutions simply because the algorithm found a particular energy well that it couldn't get out of.  

>*The computers are here to do your math, not your thinking.*

Fantastic, let's grab some numbers. One of the first things we noticed is that `eqMeCyhex.log` is just a *really* long text file that <kbd>GaussView5</kbd> is able to generate a picture from. So, open `eqMeCyhex.log` in a text editor (or follow along in another window using the link above) and we search the output for energies and vibrational/thermochemical data.  

First we'll want to check for imaginary (negative) vibrational frequencies which indicate saddle point structures. Search `eqMeCyhex.log` for `Harmonic frequencies`:

<!-- markdownlint-disable MD040 -->
```
...
Harmonic frequencies (cm**-1), IR intensities (KM/Mole), Raman scattering
 activities (A**4/AMU), depolarization ratios for plane and unpolarized
 incident light, reduced masses (AMU), force constants (mDyne/A),
 and normal coordinates:
                      1                      2                      3
                      A                      A                      A
 Frequencies --    159.1459               229.6806               248.3997
 Red. masses --      2.2917                 1.2669                 1.3457
 Frc consts  --      0.0342                 0.0394                 0.0489
 IR Inten    --      0.0025                 0.0069                 0.0006
...
```
<!-- markdownlint-enable MD040 -->

<kbd>Gaussian</kbd> prints all vibrational frequencies in the output in *ascending order* so we only need to check the first entry to ensure that all our vibrational frequencies are real.  

Next, we'll calculate the energy for our optimized structure. At this point, I **highly recommend** that you read [this technical document](https://gaussian.com/thermo/) from Dr. Joseph Ochterski about thermochemistry in <kbd>Gaussian</kbd>.<sup>1</sup> It describes how <kbd>Gaussian</kbd> calculates various thermochemical values and their proper usage in computing ΔG<sub>*rxn*</sub>. Searching `eqMeCyhex.log` for `correction` produces:

<!-- markdownlint-disable MD040 -->
```
...
 Zero-point correction=                           0.198783 (Hartree/Particle)
 Thermal correction to Energy=                    0.204789
 Thermal correction to Enthalpy=                  0.205654
 Thermal correction to Gibbs Free Energy=         0.171472
 Sum of electronic and zero-point Energies=           -274.642438
 Sum of electronic and thermal Energies=              -274.636431
 Sum of electronic and thermal Enthalpies=            -274.635566
 Sum of electronic and thermal Free Energies=         -274.669748
...
```
<!-- markdownlint-enable MD040 -->

By default, <kbd>Gaussian</kbd> reports energies in Hartree atomic units:  
<center>
    <math>
        1 E<sub>H</sub> =  ħ<sup>2</sup> / m<sub>e</sub>ɑ<sub>0</sub><sup>2</sup> ≈ 627.5 kcal mol<sup>-1</sup>
    </math>
</center>

<br>

The values we're interested in are:

<!-- markdownlint-disable MD040 -->
```
 Thermal correction to Gibbs Free Energy=         0.171472
 Sum of electronic and thermal Free Energies=         -274.669748
```
<!-- markdownlint-enable MD040 -->

The `Thermal correction to Gibbs Free Energy` is calculated by:
<center>
    <math>
        G<sub>corr</sub> = E<sub>thermal</sub> + <i>k</i><sub>B</sub>T - TS<sub>total</sub>
    </math>
</center>
  
The `Sum of electronic and thermal Free Energies` is the sum of the above `Thermal correction` and the `electronic energy` (also known as the `single point energy` since its the energy at a [single point on the potential energy surface](https://en.wikipedia.org/wiki/Potential_energy_surface)).<sup>2</sup> This `thermally-corrected single point energy` is the value that should be used to calculate free energies of reaction (ΔG<sub>*rxn*</sub>).

<kbd>Gaussian</kbd> calculates the `single point energy` of each intermediate geometry it generates during optimization as well as at the start of a vibrational frequency analysis. We can exploit this fact to save us from having to set up another calculation. To find the `single point energy` search `eqMeCyhex.log` for **the last occurrence** of `SCF Done`:

<!-- markdownlint-disable MD040 -->
```
 SCF Done:  E(RM062X) =  -274.841184603     A.U. after    9 cycles
```
<!-- markdownlint-enable MD040 -->

With this value the relationship between these three quantities becomes clear.

<!-- markdownlint-disable MD040 -->
```
SCF energy: E(RM062X)                           =     -274.841184
Thermal correction to Gibbs Free Energy         =        0.171472
--------------------------------------------------------------------------
E(RM062X) + Thermal correction                  =     -274.669748
Sum of electronic and thermal Free Energies     =     -274.669748
```
<!-- markdownlint-enable MD040 -->

### <center> 🎉🎉 That's all folks!!! 🍾🍾 <br> You know everything you need to run your own <kbd>Gaussian</kbd> jobs! </center>

In our final lesson we'll see how we can use <kbd>Gaussian</kbd> to calculate relative conformational energies.  

#### A note on split basis calculations

It is common in large systems to use a smaller set of basis functions to find the optimized geometry (this is part of the Long Course) and then use a larger basis set to recalculate the single point energy. In this case the calculated `Sum of electronic and thermal Free Energies` and the `thermally-corrected single point energy` derived from the larger basis set **will not** be the same.  

>You must manually correct single point energies when running split-basis calculations.  

<br />

| <center>Previous<br><a href="/dftCourse/ShortCourse/firstJob.html">My First <kbd>Gaussian</kbd> Job</a></center> | <center><a href="/dftCourse/Introduction.html">Home</a></center> | <center>Next<br><a href="/dftCourse/ShortCourse/aValues.html">Calculating A-values</a></center> |

<br>

#### References

(1) [Thermochemistry in <kbd>Gaussian</kbd>](https://gaussian.com/thermo/)  
(2) [Potential energy surface](https://en.wikipedia.org/wiki/Potential_energy_surface)  
