---
type: manual
created_date: 2026-02-22
modified: 27-02-2026
created: 22-02-2026
---
source: [athenak](https://github.com/IAS-Astrophysics/athenak?tab=readme-ov-file)
## First impressions

- Based on [[Athena++]] -- Rewritten code for MHD, original Athena in C
- Uses [[Kokkos]] for hardware compatibility --> CPU, GPU, ARM
- Does not support all of Athena++ (not valid for our usecase)
- features adaptive mesh refinement (AMR)
- Use: enable and speed up exascale calculations
- Wiki's
	- [AthenaK_wiki](https://github.com/IAS-Astrophysics/athenak/wiki)
	- [Kokkos_overview](https://kokkos.org/about/overview/)
	- [Athena++\_wiki](https://github.com/PrincetonUniversity/athena/wiki)
> Additional compilers and software provided by the vendor may be required to build `AthenaK` on heterogeneous architectures such as GPU clusters. For example, the `CUDA` toolkit is needed to build on NVidia GPUs.

#### Download
Downloaded in Diplomka folder, thanks to recursive option, Kokkos framework should be already included.

## Dictionary
#### Problems
- Sod Tube
- Brio–Wu tube
- Double reflection mach
- Kelvin–Helmholtz
## Workflow
Basically 3 stages of simulation:
- Building the code, requires problem file
- Running the code, requires initialization file
- Analyzing the output files
#### Build
1. uses cmake  `$ cmake -B {build_dir} [options]`
	- Most options concerning Kokkos, examples:
		- `-D Kokkos_ARCH_NATIVE=ON` optimilizes problem for native infrastructure. That is the option for dummies, use it and pray for the best. More control using keywords from [Kokkos_website](https://kokkos.org/kokkos-core-wiki/get-started/configuration-guide.html#architectures). Eg.: `$ cmake -D CMAKE_CXX_COMPILER=icpc -D Kokkos_ARCH_SKX=On`
		- `-D PROBLEM="problem_file"` for user-made problems (in `src/pgen`)
		- `-j [N]` allow N jobs at once (uses more cores when being built)
		- `-D Athena_ENABLE_MPI=ON` enables parallelisation. For parallelization one needs to divide the problem into meshblocks which are then computed paralely. OpenMP not mentioned, try "ENABLE_OMP" or "\_OPENMP"
		- `-D Athena_ENABLE_OPENMP=ON` enables OpenMP parallelisation

	- Interesting thing, athena simply overwrites existing files and append new when built, no clearing, eg if once built for fluids/rt then new built again for fluids/rt unless new problem file specified
2. then second step: `cd build/src && make`
- default problems already compiled with the code? Initialization files in /src/pgen/tests
> A variety of other problem generators are also included in the code in the /src/pgen direcory. To compile the code with these, use the -D PROBLEM={name} option with cmake, where {name} is the name of the file containing the desired problem generator in /src/pgen/.

##### AthenaK directory
+ Text files:
	+ CMakeLists.txt -- I think this just tells cmake and kokkos how to build (IKEA)
	+ CODE_OF_CONDUCT.md
	+ CONTRIBUTING.md
	+ LICENSE
	+ README
	+ \*.cfg -- config files
+ Directories:
	+ inputs -- seems to be some initialization scripts for running with, ie.: "\*.athinput" files
	+ kokkos -- own git? repo of kokkos
	+ scripts -- just an example bash script of how to run on sume special clusters
	+ src -- source files
	+ tst -- ??? 
	+ vis -- some python scripts, prob. athena's own analyse and plot
+ My Directories:
	+ my_python_scripts – dir with my scripts for analysis and more
	+ visualization – save images and videos from processed results
	+ outputs – save binary (or csv) files from athenak computations
##### Problem Files
Default problems already "included", only need to specify input file when running.
 
There are exceptions, notably, hohlraum test does not run by default. A file exists, but it is not clear whether it is in a functional state.

#### Running
Build creates exe in /build/src --> run calling athena
- Needs an input file
- Outputs -- determined by parameter file
	-  Some `bin` vs `vtk` file, idk yet
	- "restart", defacto checkpoint files -- should the simulation be interrupted, the state is save
	- "History outputs", human readable, reductions of the grid
- run as: `path/to/athenak/build/src/athena -i path/to/inputfile/problem.athinput`
- possible to let it create directory with all the files of a problem with `-d <dir>` flag
- It is possible to make small tweaks to the input file as we are about to run it:
	  `./athena -i input_file <section>/<parameter>=<new_value>`
	eg: `./athena -i input_file radiation/nlevel=2`
	

##### Input file
- Sections as `<section>`
- Comments as  `# comment` 
- Constants of simulation and physical constants
- determines output format

- Option: `<meshblock>` is good to  have specified despite optional — `bin_convert.py` needs  info about meshblocks which is otherwise missing.

##### Ouput files
Generates output files in folders in the place where the simulation was called to run.
###### TAB
self explanatory
###### VTK
Supported, industrial standard. Eg. VisIt for visualization
###### BIN
Athenak own format, read through `vis/python/read_binary.py`.

Reading variables:
	`data_file = bin_convert.read_binary(file_path)`
    `dens = data_file["mb_data"]["dens"][0][0,:,:]`
where key refers where the variables are stored, second key to variable, third unwraps list of meshblock (first element) to np.array, fourth diminishes dimension 3D -> 2D.

A proper way of unwrapping is the need to glue different meshblocks together, either during data processing or when plotting.

Other important keys:
+ "header" – the whole header file with configurations --> all information
+ "time", "cycle" – time
+ "var_names" – list of variable names in "mb_data"
+ "x1min" (and variations min max) -> total length in that dimension
+ "Nx1" total number of cells in that dimension
+ "n_mbs" number of meshblocks
+ "nx1_mbs" number of meshblocks in dimension x1 per meshblock ?
+ "nx2_out_mb" number of cells per meshblocks ?
+ "mb_logical" dunno
+ "mb_geometry" boundaries of the meshblocks in problem units
+ "mb_index" starting and final index of individual meshblocks

*What does "mb" mean?*

###### HDF5
Zdá se, že tento formát není athenak podporován
###### ATHDF
Zdá se, že athena někde za nějakých okolnostech používá tento formát.  Měl by být podobný `hdf5` a efektivně jej nahrazovat. Jedna z možností jak se k němu dostat je skrze funkce v /vis/python/read_binary.py (bin->Python dic & Python dic -> athdf). 
Alternativně se snad nazývá i `xdmm`?? Nejsem si jistý. K čemu by tento formát měl být dobrý? Proč nepoužít jiné? Odpověď asi bude zpětná kompatibilita. 

Nelze generovat soubory přímo v tomto fformátu.


##### How to make Hohlraum work?
Provided that Hohlraum.cpp works, what changes need to be done to include it in the pre-defined problems of Athena?

From quick overview it seems:
1. One needs to tackle `pgen.cpp`  (line 936 or so) to include it as a method of *ProblemGenerator* class
2. include in `pgen.hpp` to let the program know it exists
3. improve CMake (prob. `src/CMakeLists.txt`) to let the compiler know to include the file in the compilation

Attempt:
- Vytvořil jsem copii AthenaK `expr_athenak`, modifikoval jsem soubory výše a pokusil se spustit.
- And it seems it might have worked!!
- I have managed to replicate results shown in the tutorial!!

Current state:
- Changes above tracked by git, branch *hohlraum*.


##### How to make RT2D work?
We are given rt2d.athinput, what do we need to change for it to be executable by athenak??

From previous Problem, it seems we need to 
- Include pgen file in athinput
- Compile athenak with the compile file

Questions:
- Is it possible to execute rt2d without the need for pgen file?? Could the solution be already hidden in the athenak options?
- What are the minimal changes to make it executable??

What we tried:
- Comparing athena++ file athinput.rt2d and athenak rt2d.athinput:
	- They are essentially the same (ie, it could be, that sth was forgotten)
	- Some sections were moved around

- configure = --prob=rt ?? (This is athena++ option, seems similar to "-D PROBLEM="problem_file" in case of athenak)
	- IDEA: zkompilovat athenak s pomocí -D rt.cpp

Solution:
It was necessary to rebuild athenak using `-D PROBLEM=rt` flag. Then it works. There seems to be an odd mishap: in original `rt.athinput` file is block `<hydro_srcterm>` (or similar), which is unrecognized. Moving its contents to `<hydro>` block seems to have fixed the issue. *not quite: see [[AthenaK#New probelm 02]]*

> Actually with newer version of athena, stating the fulll path seems to be mandatory, so the right flag is
> `-D PROBLEM=fluids/rt`

###### New problem 01
The result of simulation does not make quite sense. See [[Progress 26-04-23]] for details. Might have sth to do with removing original `<hydro_srcterm>`?

Solution:
The problem was data reading as revealed by athenak's native slice reader. Each meshblock has individual numpy array. Either glue the arrays together or simply use this knowledge when plotting.

*athenak's `plot_slice.py` unusable, a spaghetti code. Black box which probably spits out the right results.*

###### New probelm 02
The results still do not seem to make sense, although now we can at least see that the initial conditions are represented well, the numerical simulation still does not proceed to satisfaction.

Idea: Turn off meshblock or turn on MPI paralellism
—> didn't work

Solution:
The block `<hydro_srcterms>` is essential, the information in this block is loaded by the chain of calls `srcterms.cpp <-- hydro_tasks.cpp <-- hydro.cpp <--?? main.cpp` (and the same applies for MHD).  If nor `hydro_srcterms` provided the simulation uses some default values and won't work properly or at least not as expected. 

The confusion came from the fact that `rt.cpp` instead of reading the `const_accel_val` from the `<hydro_srcterms>` block where the information already is, it demanded the information to be restated in the `<hydro>` block. Perhaps bc `srcterms` block might not be defined or I dunno (this was initially believed superfluous and the `srcterms` block was deleted as it was not mentioned in the `rt.cpp` file). As far as I know it is simply a bug and the most straight-forward solution is to simply `rt.cpp` to read from the `srcterms` block instead.

Suggestion: change in:
`grav_acc = pin->GetReal("hydro","const_accel_val");`
"hydro" —> "hydro_srcterms" and rebuild.

Other soulution is to manually restate const_accel_val in the `hydro` block.

## Options

#### SMR/AMR

SMR (static mesh refinement), AMR (adaptive mesh refinement) zvyšují rozlišení ve vybraných oblastech zájmu. MSR jednou vygenerovaná, dále neměnná, AMR možnost úpravy on-the-fly, nutno definovat funkci spouštějící refinement (in problem gen I think, see docs).

Ad SMR (AMR analogicky):
- mesh.nx : defines number "zones", v podstatě základní diskrétní dělení prostoru. Můžeš nad tím přemýšlet jako nad počtem rysek v dané dimenzi.
- mesh.x#min/max: rozměry v dané dimenzi v přirozených jednotkách
- meshblock.nx# : afaik defines how many "zones" or cell per one block
- refinement#.x#min/max : defines regions of finer mesh 
- refinement#.level : defines how much refined a region should be. The scale is $2^{\mathrm{level}}$ 
- one can generate mesh file to see if the mesh was defined correctly, eg: ```~/athena/bin/athena -i athinput.example -m 1```  the `m` flag (dunno "1"  does)

#### Riemann solvers
See `hydro.hpp`, `mhd.hpp` (and a like) for options. As of now the default options are (listed for MHD):
- advect
- hlle
- hlld
- roe
- llf

#### Reconstructors of the state
See `athena.hpp` ln. 68, known to me are currently:
- dc - constant??,  order 1??
- plm – piecewise linear method, order 2
- ppm4 – order 3??? 
- ppmx – oder 3???
- wenoz – weno method (dont remember)

  For use of reconstructor: see `hydro_fluxes.cpp`, line 84, switch

*What is the characterization and difference between ppm4 and ppmx??*
#### Integrators
Athena mostly uses Runge-Kutta type derivative operators as integrators. "imex" means explicit-implicit method (vaguely do I remember that this should correspond or be equal to the predictor-corrector methods).

The only thing I found was in `driver.cpp`, ln. 200, switch:
- rk1, rk2, rk3, rk4
- imex2, imex2+, imex3

#### Python
v adresáři `/vis/python` sada pythonovských skriptů vhodných pro analýzu. Základní idea: převádějí data formáty na dictionaries. U tab asi zbytečné.
- bin_convert.py: bin --> Python dic
- read_athena: athdf, tab --> dic
- make_athdf.py: bin --> athdf (uses bin_convert)

=> Zdá se, že žádná podpora pro vtk (v tomto ohledu jsem na to sám)

Další skripty
- calculate_tori_\*.py --> Calculating properties of a torus
- cartgrid.py --> "reads cartesiangrid binary format"
- plot_\*.py --> self explanatory

###### Čtení BIN files
skript bin_convert udělá z data dictionary s dictionaries. Data jsou ve formě numpy_array, ale uvnitř listu a defaultně 3D.
Úplná cesta k datům:
`data_file = bin_convert.read_binary(f'{path}/bin/{files[0]}')`
`velx = data_file["mb_data"]["velx"][0][0,0,:]`
kde:  
- mb_data je název slovníku s konkrétními daty ze simuace
- první "0" rozbalí list – not good enough. The length of the `["velx"]` is determined by the number of meshblocks. Different meshblocks have their own numpy arrays.
- následné "0,0,:" sníží dimenzi numpy pole

-------------------------------------------------
## Ideas and Questions
- [X] What are "ghost cells"?
       Cells at the boundary to emulate boundary conditions
- [X] How is the initial state of a problem initialized? I imagine that I should come and say mass density here and there, star overhere and planet so so. It doesn't seem to work like that. IE problém zadání počátečních podmínek.
      Yes, there needs to  be a constructor class which takes the *problem* and its description and creates the initial state out of it.

## Problems
- [ ] I have trouble understanding how the problems are defined, where they are defined and what the solution should look like.  If I manage to run the simulation successfully, how do I recognize that?
- [ ] What are the limitations on input files imposed by a build without a *problem_file* and with one? ie out of all the number of input files available by default which one can be run?? All of them?
- [ ] Where to find documentation to individual input files and how to possibly create my own (as that I would know what it means)