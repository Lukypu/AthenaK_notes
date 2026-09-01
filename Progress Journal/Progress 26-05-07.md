---
created: 2026-05-07
modified: 2026-05-14
type: journal
---


### Initial goal
- See Kokkos configuration
- Zingale: read sth 'bout the polynomial (more concretely piecewise polynomial PPM) reconstruction of states.
- try out parallelization – zapnout MPI a vyzkoušet na RT
- Compute different problems
	- Sod, Brio-Wu, Double reflection. Kelvin-Helmholtz
		- mind: Double reflection not found among Athenak problems, it is among athena++ problems as "dmr". As such the problem is not easily available in athenak.
	- Try different Riemann-solvers
	- Try different reconstructors

### Results and Conclusion
- Add Kokkos configuration:
	- use native-on (see [[AthenaK]]), otherwise need to look into documentation of Kokkos and know the archhitecture of the computer. How much speed up when explicitely stated compared to "native-on" remains unknown. How much lag when wrong arch.  supplemented also unknown. These unknowns are not going to be explored.
- Add Zingale:
	Done good enough, dunno what take out of it. The important part is that when solving a numerical problem, one first needs to reconstruct the state using some polynomial method (reminds "spline" methods) and then use an appropriate Riemann solver. Don't forget limiters that increase stability.
  
	These to freedoms determine the output. Riemann solver the most important i guess. What kind of Riemann solvers to choose from?

- Keywords:
	- PPM – piecewise polynomial method (of reconstruction of state)
	- Riemann solver
	- Limiter 
- Other keywords:
	- Flux approach respects conservation laws

- Parallelization:
	- Managed to execute the simulation with parallelization on
	- Both MPI and OpenMP:
		`-D Athena_ENABLE_MPI=ON`
		`-D Athena_ENABLE_OPENMP=ON`
	- tried different meshblocks
	- did not observe any particular speed up
	- see `my_python_scripts/test_notes/rt_parallelization_time.toml`
	- 

  - Ad Problems:
	  - Tried to locate viable options:
		  - For possible reconstructors see
			  `athena.hpp`, line 68, class ReconstructionMethod
		  - For use of reconstructor: see `hydro_fluxes.cpp`, line 84, switch
		  - For Riemann_solvers see:
			  `hydro.hpp` (list), or `hydro_fluxes.cpp`

### To Do

+ Known Bugs

+ To Do In the Future
	+ Be clear what is the difference between OpenMP and MPI, when is better one over the other. I know it is possible to use both at the time, but is it advisable? When or why not?
