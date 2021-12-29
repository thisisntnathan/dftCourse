---
layout: page
title: <kbd>g16</kbd> routing line templates
sub-title: Resources
sidebar_link: True
permalink: /Resources/cheatCodes.html
---
<!-- markdownlint-disable-file MD040 MD024 -->

This section provides general templates for the most common Gaussian jobs. Minimal explanation is provided and **it is strongly advised** that you read the respective sections for these calculations.  

>To run any of the optimizations below using Gaussian's generalized internal coordinates give OPT the `GIC`keyword.

#### Split-basis calculations

In all cases a split basis set has been utilized to reduce computational costs as our group typically works on relatively large systems. In the following “Basis Set (HL/LL)” refer to the high- and low-level basis sets, respectively. If your systems are small enough or computational resources are considerable enough to treat the entirety of the system with a single basis set then that approach is preferable. As we've covered already, a final single point energy calculation is redundant in this case.  

>N.b. SCF energies computed using two different basis sets are incomparable.

## Ground State Geometry Optimizations/Energy Calculations

### Step 1: Optimize a ground state geometry

```
#N Level of Theory/Basis Set (LL) OPT=gediis FREQ=NoRaman 
temperature=Temperature Integral(Grid=UltraFine)
```

### Step 2: Calculate ground state single point energy

```
#N Level of Theory/Basis Set (HL) SP Integral(Grid=UltraFine)
```

<br>

## Transition State Optimizations/Energy Calculations

### Step 1:  Optimization around the active atoms

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,EstmFC,ModRedundant,NoEigentest) 
Geom=Connectivity  
```

Then, at the end of the input file, add: `B [Atom 1 number] [Atom 2 number] F`  
Where atoms 1 and 2 will be frozen in the geometry optimization  
e.g. `B 74 94 F`  
>N.b. there are spaces between each parameter and the next.

#### If optimizing using Generalized Internal Coordinates (GIC)

At the end of the input file, add: `CoordinateName(freeze)=R(Atom 1 number,atom 2 number)`  
Where atoms 1 and 2 will be frozen in the geometry optimization  
e.g. `BrC(freeze)=R(54,46)`
>N.b. all coordinates must have a unique name

### Step 2: Geometry optimization of the active atoms

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,CalcFC,NoEigentest) 
freq=NoRaman temperature=Temperature Geom=Connectivity Integral(Grid=UltraFine)
```

### Step 3: Calculation of single point transition state energies

```
#N Level of Theory/Basis Set (HL) SP Integral(Grid=UltraFine)
```

<br>

## QST Transition State Optimizations/Energy Calculations

### Step 1: Optimize ground state geometries for reactant ensemble and product ensemble

If using `QST3`, also optimize the best guess for the transition structure.  

```
#N Level of Theory/Basis Set (LL) OPT=gediis FREQ=NoRaman 
temperature=Temperature Integral(Grid=UltraFine)
```

### Step 2: Quasi-Newton Transition Structure Search

It is strongly advised to save a checkpoint file for these calculations as you'll need it for the intrinsic reaction coordinate calculation to verify the optimized structure.  

```
#N Level of Theory/Basis Set (LL) OPT=(QST2/QST3) FREQ=NoRaman 
temperature=Temperature Integral(Grid=UltraFine)
```

### Step 3: Calculation of single point transition state energies

```
#N Level of Theory/Basis Set (HL) SP Integral(Grid=UltraFine)
```

<br>

## Intrinsic Reaction Coordinate (IRC) calculation for verification of transition structures

An IRC requires initial force constants to proceed. The easiest way to do this is to use the ones in the checkpoint file from the previous frequency calculation using option `rcfc`, but if you didn't save the checkpoint file from the TS optimization then pass the option `calcfc` to calculate force constants at the beginning of the calculation.  

```
#N Level of Theory/Basis Set (LL) IRC=(rcFC/calcFC) 
temperature=Temperature Integral(Grid=UltraFine)
```
