---
layout: page
title: What is a transition state?
sub-title: The DFT Long Course
sidebar_link: true
sidebar_sort_order: 5
permalink: /LongCourse/transitionState.html
---

Transition state theory was first developed by Erying (Princeton), Evans (Manchester), and Polyani (Manchster) in the early 20th century.<sup>1</sup> Their early conceptions unified classical thermodynamics, statistical mechanics, and early kinetic theory. The theory is comprised of three major tenets:<sup>2</sup>

1. Rates of reaction can be studied by examining activated complexes near the saddle point of a potential energy surface. The details of how these complexes are formed are not important. The saddle point itself is called the transition state.  
2. The activated complexes are in a quasi-equilibrium with the reactant molecules.  
3. The activated complexes can convert into products, and kinetic theory can be used to calculate the rate of this conversion.  

Of course, this isn't a kinetics course so take a look at [ref. 2](https://en.wikipedia.org/wiki/Transition_state_theory) if you'd like to learn more about TST and how its relation to modern day kinetics. In computational organic chemistry transition states are commonly used to understand how reactions proceed and rationalize selectivities.  

<div class='message'>
A particularly elegant description by Dr. Kalju Kahn (UCSB):<sup>3</sup><br>
The simplest versions of the transition state theory assume that reactants behave like very tired mountain climbers who are trying to get from one valley to another and have to cross a mountain range. Such mountain climbers will seek out the easiest path, one that avoids steep climbs and raises minimally in altitude. They will seek out a gorge that takes them over the ridge. Like a group of tired mountain climbers, the reactant molecules in the transition state theory will follow a unique path that connects the reactant basin and the product basin. The highest point on this path is the <a href='https://en.wikipedia.org/wiki/Col' target='_blank'>col</a>, or saddle point that separates the reactant basin from the product basin. The saddle point is the point of highest energy along the reaction path and is also the point of lowest energy in the direction perpendicular to the reaction path (lowest point of the ridge that separates reactants and products.
</div>

So what is a transition state? Drs. Eyring, Evans, and Polyani have clarified for us that we're looking for a saddle point on the potential energy surface.  

>Now if you need a quick refresher from Calc III, recall that a saddle point (*a.k.a.* a minimax point) is a point on a function where the derivative in all directions is zero.<sup>4</sup>
><center>
>   <img src='/dftCourse/assets/images/LC/minmaxsaddle.png'>
>   <b>Figure 1.</b> Different types of stationary points. Reproduced from <a href='https://www.offconvex.org/2016/03/22/saddlepoints/' target='_blank'>ref 5</a>.
></center>

Further, the saddle point must be a *first-order* saddle point. First-order saddle points are characterized by a single imaginary vibrational frequency (this implies a negative force constant). Physically, this means that in one direction the saddle point is a local maximum while in all others it is a minimum (verify that you understand this visually using the figure above). The imaginary vibrational frequency *typically* represents the molecular vibration along which the reactants convert into the products. A typical potential energy surface for a reaction looks like:

<center>
   <img src='/dftCourse/assets/images/LC/symmetrysmall.png'>
   <b>Figure 2.</b> A surface with two symmetric local minima (ground states) and a saddle point (transition state). Reproduced from <a href='https://www.offconvex.org/2016/03/22/saddlepoints/' target='_blank'>ref 5</a>.
</center>

<br />

Taking a cross-sectional slice of the surface, *along the axis formed by the two minima* produces the familiar *reaction coordinate diagram* that we all know and love:

<center>
   <img src='/dftCourse/assets/images/LC/PES_RC.png' width="400" height='288'>
   <b>Figure 3.</b> Potential Energy Surface and Corresponding 2-D Reaction Coordinate Diagram derived from the plane passing through the minimum energy pathway between A and C and passing through B. Reproduced from <a href='https://commons.wikimedia.org/wiki/File:Potential_Energy_Surface_and_Corresponding_Reaction_Coordinate_Diagram.png' target='_blank'>ref 6</a>.
</center>

<br />

Recall of course that this is a vast oversimplification. The reaction coordinate space is 3N-6 dimensional (where N is the number of atoms) so when we draw 3D energy surfaces or 2D reaction coordinate diagrams we're only looking at slices of a much more complex hypersurface. So now that we know *what* we're looking for, we can start to figure out *how* we're going to do it. In the next lesson we'll cover some of the available methods of identifying transition structures.

<!-- TODO: This feels incomplete, maybe we return to it later and add some more discussion? Maybe this will feel better once we write the next section  -->

### A tiny bit of pedantry

Strictly speaking, a state is described by its thermodynamic properties, not its structure. In what follows we'll talk about methods that identify a structure corresponding to the transition state *sans vibration*. The wide use of the term "**transition state**" to describe a structure is technically wrong, which is why we will refer to these methods as **transition structure** search algorithms.

<br />

#### References

(1) K.J. Laidler & M.C. King Development of transition-state theory [*J. Phys. Chem.* **1983**, *87* (15), 2657](https://pubs.acs.org/doi/10.1021/j100238a002)  
(2) [Transition State Theory](https://en.wikipedia.org/wiki/Transition_state_theory)  
(3) [Transition States by Dr. Kalju Kahn](https://people.chem.ucsb.edu/kahn/kalju/chem126/public/qm_ts_optim.html)  
(4) [Saddle point](https://en.wikipedia.org/wiki/Saddle_point)  
(5) [Escaping from Saddle Points by Rong Ge from *Off the Convex Path*](https://www.offconvex.org/2016/03/22/saddlepoints/)  
(6) [PES and Reaction Coordinate on Wikimedia](https://commons.wikimedia.org/wiki/File:Potential_Energy_Surface_and_Corresponding_Reaction_Coordinate_Diagram.png)  
