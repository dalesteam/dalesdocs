# Setting up your system

This section aids you in preparing and setting up your system to run DALES. We assume you are using a UNIX-based system and are familiar with basic terminal commands. First, we will cover the basic requirements to use DALES, which are minimally required. Second, a list of optional librariers/packages is given that will enable optimal use of DALES. Finally, we will tell how you to obtain the latest, tested version of the code (i.e., the "main branch").

## Requirements
DALES has a few general requirements for installation and usage:
- Fortran compiler (e.g., gfortran)
- MPI implementation (for multi-cpu runs, OpemnMPI and MPICH have been tested)
- NetCDF4 libraries (for output/input)
- CMake
- Make (to build the code)
- FFTW library (for the Poisson solver. Optional but highly recommended.)
- HYPRE library (alternative Poisson solver. Optional.)

These requirements can be easily installed on most UNIX-based systems via a package manager. For example, on `ubuntu` via the standard package manager
```shell
sudo apt install cmake
```
On MacOS, a standard way to install packages is via `homebrew`
```shell
brew install cmake
```

When using high-performance conputing (HPC) infrastructures, these requirements may already be pre-installed. Many HPC infrastructures employ sets of software modules in which case the requiments often have to be loaded in. A list of available modules on the system is then given by the following command:
``` shell
module avail
```
Available modules can subsequently be loaded via the `load` command, for example, loading `cmake` via:
``` shell
module load cmake
```

## Optional libraries/packages
The folling list of libraries/packages can (probably: will) make your use of DALES easier:
- Python + numpy + matplotlib (to generate input files and/or analyse output)
- Python-netcdf4 library (to read in netcdf4 output)
- CDO (easily merge and operate on multiple netcdf4 files. Version >= 2.0.6.)
- ncview (interactive netCDF viewer)

To use ncview on windows you might need to download VcXsrv, and on OSX you need XQuartz.



