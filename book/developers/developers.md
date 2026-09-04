# For new developers
 
* git and GitHub are used for DALES development. See this [Software Carpentry course](http://swcarpentry.github.io/git-novice/) for git basics. Especially, learn about
  - git clone (download DALES from github to your computer)
  - branches (different "versions" of the DALES code, for example for developing a specific function)
  - forking (make a copy of the DALES repository to your own GitHub account)

* Avoid distributing DALES source code outside git, e.g. by email. It's hard to get the
  code back into git. If you want to send a version to someone else, make a new
  branch (in your own fork) for it, and send the link.
    
* When changing the official version: we don't want surprise changes
  to the default behavior (exceptions: fixing bugs). New features
  should in general be introduced with namelist flags to turn them on,
  and the default should be the old behavior.

* Prefer netCDF output over ASCII (netCDF is standardized, more robust, better
  tools available, easier for others to process).

* DALES should work with multiple Fortran compilers. gfortran and intel are most
  used at the moment. Automatic tests with GitHub actions, which verify that the code
  compiles with multiple compilers are in use.

## Coding patterns

* DALES can be run in parallel using MPI. The domain is split in `nprocx * nprocy` tiles in the horizontal direction.
Each tile contains `imax * jmax * kmax` grid points. The whole domain contains `itot * jtot * kmax` grid points.
* index ranges for the 3D fields are `(2:i1, 2:j1, 1:kmax)` for historical reasons.
  `i1 = imax+1` and `j1 = jmax+1`. 
* Ghost cells are added at the edges of the 3D fields of each process, and contain values copied from neighboring tiles. `ih` and `jh` are the number of ghost cells in the x and y directions. Generally 1 <= ih,jh <= 3, depending on the advection schemes used.
A 3D field including ghost cells is indexed like this: `allocate(thl0 (2-ih:i1+ih,2-jh:j1+jh,k1))`. 
The "proper" grid points are in the range (2:i1, 2:j1, 1:kmax), and ih or jh ghost cells are added on each side in the horizontal directions.
* warm starts should be bitwise exact - after a restart the model should continue as if the restart never happened. But see the note of reproducibility with MPI above.
* field naming: `thl0` is the thl value at the current (sub)time step,
  `thlm` is the value at the beginning of the full step, and `thlp` is the tendency
  calculated for the current time step. 

* the top level file calls procedures for different physical processes and statistics, each procedure keeps track
  of when it should run, `tnext_...` 

## Reproducibility

* DALES runs that are even slightly different will diverge and become
  more and more different over time. This is typical for simulating chaotic systems,
  and means testing can be difficult.
  
* Parallel runs, especially with more than one node are not bitwise
  reproducible, because MPI does not guarantee bitwise reproducibility
  (probably the reduce operations which can happen in different
  order).


## Programming quirks

* the startup order is fragile, be careful if re-ordering
* Fortran doesn't allow circular module references. This can
  create confusing errors, where compilation stops working olny when
  one does a clean build.
* time is counted both with floating point number of seconds, and integer number of milliseconds.

