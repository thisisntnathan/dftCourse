---
layout: page
title: <kbd>g16</kbd> routing line templates
sub-title: Resources
sidebar_link: True
permalink: /resources/cheatCodes.html
---
<!-- markdownlint-disable-file MD040 MD024 -->

This section provides general templates for the most common Gaussian jobs. In all cases a split basis set has been utilized to reduce computational costs as our group typically works on relatively large systems. In the following “Basis Set (HL/LL)” refer to the high- and low-level basis sets, respectively. If your systems are small enough or computational resources are considerable enough to treat the entirety of the system with a single basis set then that approach is preferable. As we've covered already, a final single point energy calculation is redundant in this case.  

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

## Transition State Optimizations/Energy Calculations

### Step 1:  Optimization around the active atoms

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,EstmFC,ModRedundant,NoEigentest) 
Geom=Connectivity  
```

Then, at the end of the input file, add:  
B [Atom 1 number] [Atom 2 number] F  
Where atoms 1 and 2 will be frozen in the geometry optimization  
e.g. `B 74 94 F — Note: there are spaces between each parameter and the next.

#### If optimizing using Generalized Internal Coordinates (GIC)

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,EstmFC,AddGIC,NoEigentest) 
Geom=Connectivity
```

Then, at the end of the preview, add:  
CoordinateName(freeze)=R(Atom 1 number,atom 2 number)  
Where atoms 1 and 2 will be frozen in the geometry optimization  
e.g. `BrC(freeze)=R(54,46)`

### Step 2: Geometry optimization of the active atoms

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,CalcFC,NoEigentest) 
freq=NoRaman temperature=Temperature Geom=Connectivity Integral(Grid=UltraFine)
```

#### If optimizing using Generalized Internal Coordinates (GIC)

```
#N Level of Theory/Basis Set (LL) OPT=(TS,gediis,CalcFC,GIC,NoEigentest) 
freq=NoRaman temperature=Temperature Geom=Connectivity Integral(Grid=UltraFine)
```

### Step 3: Calculation of single point transition state energies

```
#N Level of Theory/Basis Set (HL) SP Integral(Grid=UltraFine)
```

## QST Transition State Optimizations/Energy Calculations

### Step 1: Optimize ground state geometries for reactant ensemble and product ensemble

```
#N Level of Theory/Basis Set (LL) OPT=gediis FREQ=NoRaman 
temperature=Temperature Integral(Grid=UltraFine)
```
