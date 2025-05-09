# Changes between DALES 4.x and 5.0

This page documents the recent changes to DALES, in particular those that require adapting existing cases. For more details, see [CHANGELOG](https://github.com/dalesteam/dales/blob/main/CHANGELOG.md) in the DALES repository.

## Incompatibilities

The following changes require user adaptation, changeing the installation procedure and adapting old case files.


### Installation

The installation steps have changed: DALES now uses git submodules, and the options to `cmake` have been modified. See  [](sec:compilation) for the current compilation procedure.

### Scalar variables

Scalar variables are now accessed by name, instead of just index.
Scalars variables are automatically added as required by the modules that are active.

* nsv in the namelist is now ignored
* iadv_sv is no longer an array but a single value for all scalars
* `scalar.inp.NNN` is no longer mandatory. Scalars for which no initial value is provided are initialized to 0. If `scalar.inp.NNN` is provided, it must contain a header line with the names of the scalars in the columns.
The header must be two lines (as before). The second line must now
list the scalar names and nothing else (for example an extra `#`, if followed by a space will cause problems).
This is a valid scalar input file:
```
input file Scalar Profiles - RICO Trade Cu Period (12/16-01/08)
height         nr      qr
2.0000E+001     0       0
6.0000E+001     0       0
1.0000E+002     0       0
...
```

## Other major changes

* GPU support with OpenACC (see section [](sec:GPU))
* Immersed boundary conditions enabling simulation of obstacles e.g. buildings (see section IBM) 
