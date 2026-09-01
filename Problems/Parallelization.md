---
created: 2026-07-30
modified: 2026-08-31
---

## Introduction
#### Using in Athenak
Turn on when build, options:
- `-D Athena_ENABLE_OPENMP=ON`
- `-D Athena_ENABLE_MPI=ON`

#### Different options
###### OpenMP
- parallelization within a single node, processes share resources, most importantly their memory, eg: fishers within one lake
- "4 cores on 100%"
- Scaling?

###### MPI
- parallelization over distributed memory systems, different nodes, eg: fishers fish for fishes in different lakes
- "1 core 400%"
- scaling?
#### How large job and meshblocks for efficient parallelization?
TODO:

#### Miscellaneous
###### CPU structure
- Total cores –> should be the number of independent units, but seems misleading
	- Performance cores – "state of the art" capable of demanding tasks
	- Efficient cores – less powerful more optimized for power management, intended for simple ordinary tasks eg browsing internet.

- Threads – ordinarily available only for the performance cores, my case 4x P-cores, 8x E-cores and 16 threads => 2 threads per P-core and 1 per E-core, ie single thread may do up to 2 independent tasks on a P-core (not as efficient as two different cores, by far).
	- so in retrospect it seems (oversimplified) MPI coordinates threads over one core while OpenMP coordinates different cores. But! I'd expect one core to have a singular access to memory so, I possibly mixed effects of MPI and OpenMP from above.

###### Pinning
To effectively compare the efficiency of parallelization and perhaps to also attain control, it is better to pin the resources beforehand. Both OPM and MPI are controlled by local modules which athena 'only' makes use of. Before running athena, one should therefore specify the number of "ranks" (MPI) or "threads" (OPM) to use. These are 'workers', and their total number should not exceed that of number of threads offered by local CPU/GPU.

Example of a configuration:
```
#!usr/bin/bash
export OMP_NUM_THREADS=4    # number of threads
export OMP_PROC_BIND=true   # binding? to cpu
export OMP_PLACES=cores     # places each on different core

mpirun -np 2 ./athena -i input.in   # MPI 2 processes
```



## Tests

#### Test 0
MPI=OFF, OpenMP=OFF
3 meshblock 400x400
no parallelization enabled
cca 1h wall time

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 23841  
cpu time used  = 3.556369e+03  
zone-cycles/cpu_second = 1.072600e+06  
particle-updates/cpu_second = 0.000000e+00

#### Test 1
MPI=OFF, OpenMP=OFF
time sudo nice --10 ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_01_NoParall/    
  
Root grid = 1 x 3 x 1 MeshBlocks  
Total number of MeshBlocks = 3  
Number of logical  levels of refinement = 2 (3 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 1

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 23841  
cpu time used  = 3.537192e+03  
zone-cycles/cpu_second = 1.078415e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    58m57,301s  
user    0m0,134s  
sys     0m0,320s


#### Test 2
MPI=ON, OpenMP=OFF, 3 MPI
time mpirun -np 3 ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_02_NoParall/    
  
Root grid = 1 x 3 x 1 MeshBlocks  
Total number of MeshBlocks = 3  
Number of logical  levels of refinement = 2 (3 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 3  
 Rank = 0: 1 MeshBlocks, cost = 1  
 Rank = 1: 1 MeshBlocks, cost = 1  
 Rank = 2: 1 MeshBlocks, cost = 1  
Load Balancing:  
 Maximum normalized cost = 1, Average = 1

> I did not change niceness, too troublesome for mpi must not run as root, though I think it wont change much
> Athenak's processing is represented as three separate processes, each with it's core
> Also it seems that on my machine, the bottleneck is closer to the CPU side than the of memory.

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 23841  
cpu time used  = 1.465704e+03  
zone-cycles/cpu_second = 2.602544e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    24m26,985s  
user    73m14,733s  
sys     0m2,877s

> Attempt to run 6 MPI threads on 3 meshblocks terminated upon error. Athenak is not going to allow that.

#### Test 3
MPI=OFF, OPM=ON, 3 threads OPM (probably wrong)
time ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_04_OPM/    
  
Root grid = 1 x 3 x 1 MeshBlocks  
Total number of MeshBlocks = 3  
Number of logical  levels of refinement = 2 (3 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 1

> Seen as a single core running on 100%, though it is a performance core. Otherwise quite underwhelming.

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 23841  
cpu time used  = 3.430381e+03  
zone-cycles/cpu_second = 1.111993e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    57m10,476s  
user    57m9,896s  
sys     0m0,276s

> So, we can see that it didn't help much, why it should be so?
> we can look at the cpu time used, but there is but a negligible difference too and I think that it should correspond to the overall work done.
> The "improvement" might be due to strictly using one power core compared to the switching routines observed in the previous runs, therefore switching cost was saved.

> Further investigation uncovered probable problem. As per Athena++ Docs, it seems likely that one needs to specify \<mesh\> num_threads = 3 in the input file AND export OMP_NUM_THREADS = 3 before running in the terminal. Also "Each OMP thread must be assigned/correspond to at least one MeshBlock".


#### Test 4
MPI=ON, OPM=ON, 3 threads OPM, 1 MPI
time ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_05_OPM/    
  
Root grid = 1 x 3 x 1 MeshBlocks  
Total number of MeshBlocks = 3  
Number of logical  levels of refinement = 2 (3 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 1

> did not notice improvement over the last time, prob need to look into athena and also if OMP works for me

> OMP seems to work fine and the environment is kicked off when athena launched. It's just that Athena is not consuming more than ordinary number of resources.
> Tried 1 MPI and 3 or 6 OMP but it simply did not result in an increased cpu usage. I am at my wits end and also at the end of the docs as far as I could find. The only consolatory thing is that athena team recommends using MPI only anyway.

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 23841  
cpu time used  = 3.413068e+03  
zone-cycles/cpu_second = 1.117634e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    56m54,351s  
user    56m52,779s  
sys     0m0,768s

#### Test 5
MPI 6x, OMP=OFF
time mpirun -np 6 ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_05_MPI/    
Authorization required, but no authorization protocol specified  
  
Authorization required, but no authorization protocol specified  
  
  
Root grid = 2 x 3 x 1 MeshBlocks  
Total number of MeshBlocks = 6  
Number of logical  levels of refinement = 2 (3 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 6  
 Rank = 0: 1 MeshBlocks, cost = 1  
 Rank = 1: 1 MeshBlocks, cost = 1  
 Rank = 2: 1 MeshBlocks, cost = 1  
 Rank = 3: 1 MeshBlocks, cost = 1  
 Rank = 4: 1 MeshBlocks, cost = 1  
 Rank = 5: 1 MeshBlocks, cost = 1  
Load Balancing:  
 Maximum normalized cost = 1, Average = 1

Terminating on time limit
time=5.500000e+00 cycle=7947
tlim=5.500000e+00 nlim=30000

MeshBlock-cycles = 47682
cpu time used  = 1.274387e+03
zone-cycles/cpu_second = 2.993251e+06
particle-updates/cpu_second = 0.000000e+00

real    21m15,729s
user    127m19,384s
sys     0m8,177s

> We already observe quite a saturation

#### Test 6
MPI 6x, OMP OFF, 12 mb
time mpirun -np 6 ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_06_MPI/    
Authorization required, but no authorization protocol specified  
  
Authorization required, but no authorization protocol specified  
  
  
Root grid = 2 x 6 x 1 MeshBlocks  
Total number of MeshBlocks = 12  
Number of logical  levels of refinement = 3 (4 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 6  
 Rank = 0: 2 MeshBlocks, cost = 2  
 Rank = 1: 2 MeshBlocks, cost = 2  
 Rank = 2: 2 MeshBlocks, cost = 2  
 Rank = 3: 2 MeshBlocks, cost = 2  
 Rank = 4: 2 MeshBlocks, cost = 2  
 Rank = 5: 2 MeshBlocks, cost = 2  
Load Balancing:  
 Maximum normalized cost = 1, Average = 1

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 95364  
cpu time used  = 1.342185e+03  
zone-cycles/cpu_second = 2.842052e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    22m23,458s  
user    134m8,345s  
sys     0m6,091s

> slightly worse than 1-1 division of work, but only within few percent. Might be worse for smaller meshblocks.

#### Test 7
MPI 12x, OMP OFF
time mpirun -np 12 ./athena -i ../inputs/rt_par/rt2d_parallel.athinput -d ../outputs/rt_par/rt_par_07_MPI/    
Authorization required, but no authorization protocol specified  
  
Authorization required, but no authorization protocol specified  
  
  
Root grid = 2 x 6 x 1 MeshBlocks  
Total number of MeshBlocks = 12  
Number of logical  levels of refinement = 3 (4 levels total)  
Number of physical levels of refinement = 0 (1 levels total)  
Number of parallel ranks = 12  
 Rank = 0: 1 MeshBlocks, cost = 1  
 Rank = 1: 1 MeshBlocks, cost = 1  
 Rank = 2: 1 MeshBlocks, cost = 1  
 Rank = 3: 1 MeshBlocks, cost = 1  
 Rank = 4: 1 MeshBlocks, cost = 1  
 Rank = 5: 1 MeshBlocks, cost = 1  
 Rank = 6: 1 MeshBlocks, cost = 1  
 Rank = 7: 1 MeshBlocks, cost = 1  
 Rank = 8: 1 MeshBlocks, cost = 1  
 Rank = 9: 1 MeshBlocks, cost = 1  
 Rank = 10: 1 MeshBlocks, cost = 1  
 Rank = 11: 1 MeshBlocks, cost = 1  
Load Balancing:  
 Maximum normalized cost = 1, Average = 1  
  
Setup complete, executing task list(s)...

Terminating on time limit  
time=5.500000e+00 cycle=7947  
tlim=5.500000e+00 nlim=30000  
  
MeshBlock-cycles = 95364  
cpu time used  = 8.260871e+02  
zone-cycles/cpu_second = 4.617624e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    13m47,401s  
user    164m57,565s  
sys     0m18,598s

> Effectively, twice the speed-up compared to the previous computation, ie, limited overhead/no saturation

#### Test 8
MPI=OFF, OMP=ON
time ./athena -i ../inputs/orszang_tang/orszag_tang.athinput -d ../outputs/orszang_tang/

Root grid = 1 x 1 x 1 MeshBlocks
Total number of MeshBlocks = 1
Number of logical  levels of refinement = 0 (1 levels total)
Number of physical levels of refinement = 0 (1 levels total)
Number of parallel ranks = 1

Terminating on time limit  
time=2.000000e+00 cycle=5644  
tlim=2.000000e+00 nlim=-1  
  
MeshBlock-cycles = 5644  
cpu time used  = 2.556868e+02  
zone-cycles/cpu_second = 3.531822e+06  
particle-updates/cpu_second = 0.000000e+00  
  
real    4m15,738s  
user    17m2,330s  
sys     0m0,533s



Trying one of the defaults problems with OMP. This time it seems to work.

I observe 1 process going 400% and then three 'shadow' process 100% each.
First four CPU units are being used, I believe this means 2 physical core and 2 
virtual ones. Not 100% sure. Needed not specify \<mesh\> num_threads = 3.
Upon testing this command is not even recognized by athenak, it is ignored. As far as I am concerned, no way to control OMP in input files (makes kinda sense though).
