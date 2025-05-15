# Downloading the code

The next step after installed these libraries and packages is to get the DALES code. The example below uses the `Git` software for this purpose. After going to the destination path on your system, download the code by using the command below
``` shell
<username> <currentpath> % git clone https://github.com/dalesteam/dales.git
```

Here, a new directory called `dales` should have appeared. By default, the latest main branch is checked out inside this directory (denoted by the `*`). This can be confirmed by doing the following
``` shell
cd dales
git branch
```
which should give as output
``` shell
 * main
```

Other available branches of the main repository can be viewed with the `-a` option
``` shell
<username> dales % git branch -a
* main
remotes/origin/3.1
remotes/origin/3.2
remotes/origin/4.1
remotes/origin/4.1_TROFFEEhack
remotes/origin/4.1_aero_m7
...
```

Checking out a specific branch (e.g., the `ruisdael` branch) is done via
``` shell
git checkout -b <newname> remotes/origin/ruisdael
```

### Initialize submodules
This step is required to fetch some external libraries, e.g. RRTMG and RRTMGP.

```shell
git submodule init
git submodule update
```



### Planning on coding yourself in DALES?
If you plan to change parts of the code and want to keep track of your changes, it may be worthwhile to use your personal Github account and first fork the main repository. The steps to get the code on your system are then slightly different.

``` shell
git remote add origin https://github.com/<your_account>/dales.git
git remote add upstream https://github.com/dalesteam/dales.git

git config --global user.name "YOUR NAME"
git config checkout.defaultRemote origin

git fetch --all

git checkout -b main origin/main
```
```{note}
This option will checkout the code in your current directory, _without_ first making a separate `dales` destination directory.
```




(sec:compilation)=
# Compilation of DALES

## Generic compilation
The next step after obtaining the code and installing required dependencies, is to build the code. As a starting point, we assume you are still in the main `dales` directory after checking out the main branch via git. It is advised the build the code in a different folder to better maintain overview. We therefore first move one directory up, make a new directory and move into that directory.
``` shell
cd ../
mkdir build
cd build
```

We then invoke `cmake` on the directory where the code is placed. This will configure the build of the code.
``` shell
cmake ../dales
```
Finally, the code can be build with the following command:
``` shell
make
```
After successfull compilation, the executable `dales` is located in the subdirectory `bin/`. Compilation of the code may be sped up using the `-j <nprocs>` specifier, with `nprocs` being the amount of parallel processes.

## Compilation options
It is possible to specify optional features at the compilation stage of the model. These optional features can be activated by adding them as specifyers to the cmake command. CMake options are specified as `-D<option>=<value>`. For example:
``` shell
cmake ../dales -DCMAKE_BUILD_TYPE=Debug
```
will produce a debug build. The debug build is much slower than the release build but contains more error checks. A list of commonly used options is given below.

| Option | Description | Allowed values | Default |
| ------ | ----------- | -------------- | ------- |
| `-DENABLE_FFTW` | Build with FFTW | True/False | True |
| `-DENABLE_HYPRE` | Build with HYPRE | True/False | False |
| `-DENABLE_FP32_FIELDS` | Use single precision floating-point numbers for prognostic fields (momentum, temperature, etc.) | True/False | False |
| `-DENABLE_FP32_POIS` | Use single precision floating-point numbers for the Poisson solver | True/False | False |
| `-DENABLE_ACC` | Build with GPU support through OpenACC | True/False | False |

```{caution}
To use HYPRE or FFTW, the library needs to both be enabled at compilation and selected at runtime by setting the &SOLVER section of the namoptions input file. See [Alternative Poisson solvers (Wiki)](https://github.com/dalesteam/dales/wiki/Alternative-Poisson-solvers). By default, FFTW is used if the library was found at compilation.

```
```{hint}
`-DPOIS_PRECISION=32` and `-DUSE_HYPRE=True` can be used together, but HYPRE is 64-bit only, so only use this, if it is really what you want.
```

## Compilation on specific clusters

### Delftblue (TUDelft HPC)
**Tested 20-11-2023**
``` shell
module load 2023r1-gcc11
module load openmpi/4.1.4
module load cmake/3.24.3
module load netcdf-fortran/4.6.0
module load fftw/3.3.10

git clone https://github.com/dalesteam/dales

# for dev branch:
git checkout dev
git submodule init
git submodule update


cd dales
mkdir build
cd build

export SYST=gnu-fast
cmake ..  -DUSE_FFTW=True

make -j 8
```

### Snellius (Dutch National system)
**Tested with module set 2023**
``` shell
module load 2023
module load foss/2023a
module load netCDF-Fortran/4.6.1-gompi-2023a
module load CMake/3.26.3-GCCcore-12.3.0
# module load Hypre/2.29.0-foss-2023a

mkdir build
cd build
export SYST=gnu-fast
cmake ..

# or, for single precision (options for version >= 5.0 or dev):
# cmake ../dales -DENABLE_FP32_FIELDS=ON -DENABLE_FP32_POIS=ON

make -j 8
```
