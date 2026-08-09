# Quirks

A collection of problems or unexpected behaviour. Pure bugs in DALES should be filed as github issues instead.

## Various netCDF errors
these are often caused by netCDF files from a previous run being left in the case directory.
In particular if the domain or tile size has been changed since the previous run, it's best to delete any old netcdf output.
Also if DALES crashed, the netCDF files may be left in an unusable state.

## STOP FFTW plan creation failed

This error message is generally caused by linking to the Intel MKL library instead of FFTW.
MKL pretends to be compatible but is not, in particular it does not support the advanced plan creation interface used in DALES. 
To see the libraries linked to your DALES program, run `ldd path/to/your/dales`, and look for mkl libraries being mentioned.
The CMake files supplied with DALES should discourage the use of MKL as an FFTW substitute by default (from version 5.0 on).

## Internal compiler error with ifort 19.0.5.281 20190815
30.7.2020, FJ. On SuperMUC, when compiling with SYST=lisa-intel.
Compiling modheterostats.f90 causes an internal compiler error.
Changing `-O3` to `-O2` allows the compilation to finish.
```
/lrz/sys/intel/impi2019u6/impi/2019.6.154/intel64/bin/mpiifort \
-I/dss/dsshome1/lrz/sys/spack/release/19.2/opt/x86_avx512/netcdf-fortran/4.4.4-intel-ryxkupm/include \
-r8 -ftz -extend_source -g -traceback -O3 -xHost -module program_modules   \
-c /dss/dsshome1/0F/di67mow/dales/src/modheterostats.f90 -o CMakeFiles/dales4.dir/modheterostats.f90.o

20000_28030


catastrophic error: **Internal compiler error: internal abort** Please report 
this error along with the circumstances in which it occurred in a Software Problem Report.  
Note: File and line given may not be explicit cause of this error.
compilation aborted for /dss/dsshome1/0F/di67mow/dales/src/modheterostats.f90 (code 1)
```

## Crash with OpenMPI 3.1.1
On Lisa, runs with 16 cores on one node frequently crashed when compiled with the foss/2018b module set.
This seems to be a bug in OpenMPI. With OpenMPI 3.1.3 the problem disappeared. The crash usually happened in `transpose_b` or `transpose_binv` in modpois.
Such a bug is mentioned in the OpenMP [release notes](https://www.open-mpi.org/source/new.php), and 
[this bug report](https://github.com/open-mpi/ompi/issues/5375).


## Cray MPI and system() commands
Cray MPI is by default not compatible with programs that launch other programs with system().
In Dales, at least some versions, system() is used to symlink init*_latest_ files to the appropriate
init* file. This caused crashes. A possible workaround is to call a function for creating the symlink,
this is annoying because the syntax and support for this differs between Fortran compilers.

## Wrong wallclock time measurement
Some OpenMPI versions report an incorrect wallclock time - not taking CPU frequency variations into account.

## ifort: command line remark #10148: option '-i_dynamic' not supported
Cmake before version 3.0 is not compatible with the Intel Fortran compiler - it adds an obsolete flag -i_dynamic.
Using a newer cmake solves this issue.

## Odd domain size and Poisson solver

The (old-built-in) FFT Poisson solver may not be correct if the domain size is odd.
With odd sizes, there is a different element ordering in the output of the FFT,
and the code seems to not take this into account.
Test on the rico case, divergence diagnostics:
divmax, divtot =    7.31E-10   1.60E-11 (rico, 145x144)
divmax, divtot =    3.70E-17   2.28E-11 (rico, 144x144)

divmax for odd sizes is significantly larger, which is a sign that the solution is less accurate.
For now, odd domain sizes are not recommended. For future alternative Poisson solvers,
we will aim to handle odd sizes correctly, rather than reproducing the current behavior.

This problem should not be present for the FFTW-based solver, which is the default from DALES 5.0 on.
