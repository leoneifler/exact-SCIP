# Exact SCIP
This is a development version of the *numerically exact* variant of MIP solver SCIP, based on SCIP 7.0.

The exact solving mode is based on the framework described in
> William J. Cook, Thorsten Koch, Daniel E. Steffy, Kati Wolter: [A hybrid branch-and-bound approach for exact rational mixed-integer programming.](https://doi.org/10.1007/s12532-013-0055-6) Math. Program. Comput. 5(3): 305-344 (2013)

That framework has been revised and extended by symbolic presolving, using the parallel presolving library [PaPILO](https://github.com/lgottwald/PaPILO), as well as an exact repair heuristic.
The results of this revision are described in
> Leon Eifler, Ambros Gleixner: [A computational status update for exact rational mixed integer programming](https://doi.org/10.1007/s10107-021-01749-5)  Math. Program. (2022).

Furthermore, the most recent version features separation and verification of safe Gomory mixed integer cuts.

Certificate printing is supported (although presolving cannot yet be verified), and can be checked using [VIPR](https://github.com/lgottwald/PaPILO).

The only input formats that are currently supported for the exact solving mode are [ZIMPL](https://zimpl.zib.de/) and MPS (only in the most recent version).

## Versions

* `scipopt-exact-0.1.tgz` the version used for the IPCO 2021 paper
* `scipopt-exact-0.2.tgz` the version used for the full length 2022 Math. Programming paper
* `scipopt-exact-0.3.tgz` the most recent version, featuring separation of safe GMI cuts
# Installation instructions

This development version of exact SCIP can only be built using CMake, the Makefile system is not functional.

Dependencies which are optional for the floating point version but mandatory for exact SCIP:
* [GMP](https://gmplib.org/) for rational arithmetic in ZIMPL, SoPlex, SCIP, and PaPILO
* [Boost multiprecision library](https://www.boost.org/) (Version should be 1.70 or newer) for rationals in SCIP and PaPILO
* [MPFR](https://www.mpfr.org/) for approximating rationals with floating-point numbers in SCIP.

## Step by step guide:

### 1. Extract the tarball

```
tar -xzvf scipopt-exact-0.1.tgz
cd scipopt-exact-0.1
```

### 1. Build Zimpl, SoPlex, PaPILO:

```
# go into the directory
cd <one of zimpl/soplex/papilo>
mkdir build
cmake ..
make
cd ../..
```

### 2. Build SCIP:

```
cd scip
mkdir build
cmake .. -DSOPLEX_DIR=<PATH_TO_SOPLEX_BUILD_DIRECTORY> -DPAPILO_DIR=<PATH_TO_PAPILO_BUILD_DIRECTORY> -DZIMPL_DIR=<PATH_TO_ZIMPL_BUILD_DIRECTORY>
make
```

Test if everything works correctly with the MIPEX ctest (vipr needs to be installed for the certificate checking to work)

```
ctest -R MIPEX
```

# USAGE

Enabling/disabling the exact solving mode is done with parameter `exact/exact_enabled`, note that this has to be done before reading a problem instance. Further advanced parameters for exact solving can be set in the `exact` submenu.
Alternatively, the provided settings file `exactsolve.set` can be used.

Certificate printing can be enabled by setting `certificate/filename` to a non-default value. If cutting planes are enabled (only version 0.3), then the certificate file needs to be completed using the `viprcomp` script prior to verifiaction.


