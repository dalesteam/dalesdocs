# Using the GPU
DALES now has the option to use graphics processing units (GPU's) to accelerate computations. 

## Prerequisites

- One or more NVIDIA GPU's
- An OpenACC compatible compiler.
- cuFFT
- A GPU-aware MPI library

The easiest and recommended way to get started on the GPU is by downloading the NVIDIA HPC SDK. This SDK contains the nvfortran compiler (which supports OpenACC), cuFFT, and a version of the OpenMPI library that is GPU-compatible. You can download it [here](https://developer.nvidia.com/hpc-sdk-downloads).

## Compiling

To compile DALES for the GPU, OpenACC has to be enabled when configuring the build with CMake. Here, it's important to make sure that CMake uses the correct compiler such that the compilation flags are setup correctly. Using the `which` command, check that the `mpif90` command points to the MPI Fortran compiler wrapper that is bundled with the HPC SDK:

```{code} bash
$ which mpif90
/opt/nvidia/hpc_sdk/Linux_x86_64/2023/comm_libs/mpi/bin/mpif90
```

If the output looks similar, you're all set. The next step is to configure the GPU build using CMake:

```{code} bash
cd dales
mkdir build
cd build
export FC=mpif90
cmake -DENABLE_ACC=On ..
make -j
```

## Problems with NetCDF-Fortran

As mentioned before, the NVIDIA compiler might complain about your installation of NetCDF-Fortran. If this is the case, you will see an error mentioning something about "Old or corrupt module files" during the compilation phase. The solution is to compile the NetCDF-Fortran bindings yourself, using the NVIDIA compiler. In this section, we will briefly explain how to do this.

You can download the NetCDF-Fortran source code from GitHub using `wget`:

```{code} bash
wget https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v4.6.1.tar.gz
tar xvf v4.6.1.tar.gz
```

Next, we need to export some variables such that NetCDF-Fortran compiler correctly. You can use `nc-config`, which comes bundled with NetCDF-C to automatically figure out some compiler flags:
```{code} bash
cd netcdf-fortran-4.6.1
export CC=nvcc
export FC=nvfortran
export CPPFLAGS=$(nc-config --cflags)
export LDFLAGS=$(nc-config --libs)
```

Finally, we can compile and install NetCDF-Fortran. Here, we use `~/software/netcdf-fortran` as the installation location:
```{code} bash
mkdir -p $HOME/software/netcdf-fortran
./configure --prefix=$HOME/software/netcdf-fortran --disable-shared
make install
```

To then compile DALES with this new library, we need to point CMake to its location:

```{code} bash
export FC=mpif90
cmake -DENABLE_ACC=On -DNetCDF_Fortran_ROOT=$HOME/software/netcdf-fortran ..
make -j
```

## Running

If you have compiled DALES succesfully with OpenACC enabled, running it is not very different from the CPU version. DALES can be launched using the `mpirun` command:

```{code} bash
mpirun -np N <path-to-DALES> <path-to-namoptions>
```

where N should match the number of GPU's you want to use. 