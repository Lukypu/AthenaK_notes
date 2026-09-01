---
created: 24-02-2026
modified: 2026-07-07
---
Note:

# Zingale: MAPPING INITIAL HYDROSTATIC MODELS IN GODUNOV CODES 

## Metadata
  @article{ Zingale_mapp_hydro ,
 author = { Zingale, Michael},
 title = { MAPPING INITIAL HYDROSTATIC MODELS IN GODUNOV CODES  },
 year =  { 2002 },
 language =  { en },
 ISBN =  { },
 url = { },
 doi = { },
 }
 - Added: 24-02-2026
 - links:
	 - url:
	 - doi:
- [x] Downloaded?
- [ ] Read?

## What and why read?
%%which sections to read and why%%
Pejcha. V mém případě není důležité mít hydrostatickou rovnováhu -- důležité zjistit, co je a jak funguje "Fluff" (reminds "swibble" from K Dicks' story)

## Contents
#### Notation
- PPM: piecewise parabolic method ("a highly used godunov method")
- EOS: equation(s) of state
- HSE: hydrostatic equilibrium
- "Fluff": zbytková atmosféra, která je tak řídká, že od ní už nadále nepožadujeme, aby byla v HSE
#### Introduction
>How to map an "Astrophysical initial model" from a stellar evolution code into the computational grid of an explicit Godunov-type code while maintaining **hydrostatic equilibrium**.

In other words, we have some longscale build-up code/situation follow by a short timescale period of interest. How do we connect the two simulations together? Eg.: white dwarf collapsing to a supernovae, then explosion of an interest.

> In the absence of any perturbations or external forces, the system should remain in hydrostatic equilibrium after the mapping.
eg different equations of state for the two phases => different 'numerical' equilibriums.

goal: create equilibrium in model2 that is as close as possible to the output of model1

Sources of errors as given by the paper:
- Different equations of motion, thermodynamical eqs.
- Different dimensionalities of the simulations
- Lagrangian vs Eulerian code -- need for variable conversion and grid difference
- Different discretization
- point-wise definitions vs cell averages
- Poor boundary conditions

#### Hydrodynamics
Definition pf hydrostaticc equilibrium (HSE). Problem mostly with gravity term: must hold $\nabla P = F_g$ , eg $F_g = \rho g$ . Problem bc preassure gradient and gravity computed in different ways. Recommendation: Couple preassure and gravity more tightly.

Also very brief presentation of numerical methods.

#### Initial models
>To better understand the effects of the initialization, boundary conditions, and hydrodynamics, we consider three different initial models. 

Ex. 3.1: Example of isothermal atmosphere  in HSE.
Ex. 3.2: Isothermal atmosphere with better EOS
Ex. 3.3: Model from stellar evolution code !(Fluff here)!
-> Some see notes from 10-03-2026
###### Fluff
Upper atmosphere so loosely densed that no longer dynamically important. 
> We use a cutoff value for the
density of $10^{-5}$ g cm-3 and affectionately refer to
the material above this as the ‘‘ fluff.’’ This cutoff is
needed since we cannot continue the HSE profile to
arbitrary heights since the densities would quickly
**underflow**.

#### Boundary conditions
May matter as much as initial condition. Especially in the case of elliptic problems.
*skipped*

#### Results
Mostly three models
- Isothermal atmosphere
- Realistic atmosphere
- Kepler model atmosphere

#### Conclusions
- Creating HSE is not dump proof
- Boundary condition important
	- Reflecting no good
	- Boundary should 'reflect physics'
-  Source term (gravity term!?) may strongly impact stability
- neglecting above mentioned problems may lead to unphysical behaviour of simulationn.

# shortcomings and questions
%%what didnt work and what was not understood though should be%%
#### Questions
- [ ] What does one-dimensional and multi-dimensional exactly mean here?
- [ ] Find the definition and use of the Riemann problem (describes shocks and other discontinuities). Mind the difference between Finite volume, Finite differences and finite elements methods/schemes
- [x] What does "density underflow" mean?? See Fluff
    - it (probably) refers to numerical underflow of values. $\rho\sim10^{-5} \; \mathrm{g/cm^{-3}}$ because of stability of difference schemes and also consider that $\rho$ might fall very quickly, eq. isothermal atmosphere $\rho$ fall exponentially with radius.

- [ ] What prevents us from just picking a point $(\rho, T, X_i)$ with a neighbourhood and integrating from there on with the new EOS? Thus completely disregarding values given/extracted from the previous simulation?

#### Shortcomings
-  Presumes operational knowledge of Godunov scheme of finite volume