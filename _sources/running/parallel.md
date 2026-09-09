# Parallelization

DALES can be run in parallel, using MPI. The domain can be decomposed in one or two horizontal dimensions.
The basic steps for a parallel run are:

* set `nprocx` and `nprocy` in the `&DOMAIN` namelist to the desired
  number of parallel domains in the x and y directions. The total
  number of processes will then be `nproc = nprocx * nprocy`.

* start DALES with
  ```
  mpirun -n <nproc> /path/to/dales namoptions.001
  ```
  where you fill in the requested number of tasks instead of `<nproc>`.
  On a cluster you may want submit a job script instead.


The best configuration (choice of `itot`, `jtot`, `nprocx`, `nprocy`) for a given job depends on the machine the job is running on. Below are some constraints to keep in mind and some general recommendations.

* `nprocx` must divide `itot`
  
* `nprocy` must divide `jtot`

* a reasonable number of grid rows and columns per task (`imax = itot/nprocx` and `jmax = jtot/nprocy`) is around 32-64.
  
* the Fourier-transform-based Poisson solver works best (fastest) when the total grid sizes (`itot`, `jtot`) factorize into powers of small numbers, e.g. 2**n * 3**m.

* it seems square tiles are efficient, or tiles that are
longer along x than y, perhaps up to two times longer.

For runs that span multiple nodes:

* if you want to use an integer number of nodes,
the `total_number_of_cores = number_of_nodes * cores_per_node` should match the number of DALES tasks = `nprocx * nprocy`.

* it's often efficient if `nprocx` divides the number of cores per node,
i.e. an integer number of tile rows fit on one node. This means that the
communication in the x direction is always within a node.

* often the number of cores in a node is of the form 2**n * 3**m, where
m is small, e.g. 48, 128, 192.
We have had good experiences with domain sizes (`itot`, `jtot`) that are 2**n or 3*2**n.

To increase speed and to save memory, also consider running in
single-precision mode, see [](sec:compilation:single)=
