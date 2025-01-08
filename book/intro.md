# DALES documentation

Welcome to the documentation of the Dutch Atmospheric Large-Eddy Simulation (in short: DALES)!

```{note}
This documentation is a work-in-progress and for a future release of DALES.
```

The aim of this manual is to provide an up-to-date starting point for new users, explaining how to compile, setup and run the Dutch Atmospheric Large-Eddy Simulation. The manual will cover, among other things, general settings, and options (for example, statistics output), and instructions how to make more advanced cases (for example, simulations with tracers). The principles of the DALES model are described in [Heus et al., 2010](https://doi.org/10.5194/gmd-3-415-2010).

```{note}
This manual is written for the main-branch (v. X.Y) in mind. Other branches may contain new features that are not necessarily described in this manual.
```

At a later stage, this manual may be extended with in-depth information on code structure, variable naming schemes, and implementation details, better enabling people to become _new contributors_ to DALES.


## The structure of this manual

This manual is structured as follows. The items without link are not yet available as they are under construction.

Getting started with DALES!
- [Setting up your system](https://dalesteam.github.io/dalesdocs/running/settingup.html)
  - [Requirements](https://dalesteam.github.io/dalesdocs/running/settingup.html#requirements)
  - [Optional libraries/packages](https://dalesteam.github.io/dalesdocs/running/settingup.html#optional-libraries-packages)
  - [Downloading the code](https://dalesteam.github.io/dalesdocs/running/settingup.html#downloading-the-code)
- [Compilation of DALES](https://dalesteam.github.io/dalesdocs/running/compilation.html)
  - [Generic compilation](https://dalesteam.github.io/dalesdocs/running/compilation.html#generic-compilation)
  - [Compilation options](https://dalesteam.github.io/dalesdocs/running/compilation.html#compilation-options)
  - [Compilation on specific clusters](https://dalesteam.github.io/dalesdocs/running/compilation.html#compilation-on-specific-clusters)

The physics
- The governing equations
  - Anelastic assumption
- Subfilter-scale model
- Discretization and Numerical Scheme
- Condensation Scheme
- The pressure solver
- Cloud microphysics
- Radiation schemes

Running your first case in DALES
- Test case
- RICO example

Setting up your own case
- Input files
  - Run script
  - Warm start
- Output files
  - Merge output files
  - Statistics
- Basic Namoptions
- All Namoptions

Advanced DALES options
- Advection schemes
- Boundary conditions
  - The surface model
  - The sides and top
- Conditional sampling
- Dispersion and chemically reacting flows
- [Running a case with tracers](https://dalesteam.github.io/dalesdocs/running/tracers.html)
  - [Preparing the NetCDF file](https://dalesteam.github.io/dalesdocs/running/tracers.html#preparing-the-netcdf-file)
  - [Adding a tracer](https://dalesteam.github.io/dalesdocs/running/tracers.html#adding-a-tracer)
  - [Tracer attributes](https://dalesteam.github.io/dalesdocs/running/tracers.html#tracer-attributes)
- [Using the GPU](https://dalesteam.github.io/dalesdocs/running/gpu.html)
  -  [Compiling](https://dalesteam.github.io/dalesdocs/running/gpu.html#compiling)
  -  [Problems with NetCDF-Fortran](https://dalesteam.github.io/dalesdocs/running/gpu.html#problems-with-netcdf-fortran)
  -  [Running](https://dalesteam.github.io/dalesdocs/running/gpu.html#running)

Options outside main branch
- Immersed Boundary Method (IBM)
- Sampling tendencies

Applications and literature
- Example cases
  - RICO
  - ...
- [List of publications using DALES](https://dalesteam.github.io/dalesdocs/running/papers.html)

Contributing
- [User Manual](https://dalesteam.github.io/dalesdocs/developers/usermanual.html)
  - [Prerequisites](https://dalesteam.github.io/dalesdocs/developers/usermanual.html#prerequisites)
  - [Writing content](https://dalesteam.github.io/dalesdocs/developers/usermanual.html#writing-content)
  - [Building the documentation](https://dalesteam.github.io/dalesdocs/developers/usermanual.html#building-the-documentation)
- The code
