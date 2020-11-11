# exactSCIP-devel
A development version of the numerically exact variant of MIP solver SCIP

# Installation instructions

This development version of exact SCIP can only be built using CMake, the Makefile system is not functional.

Dependencies which are optional for the floating point version but mandatory for exact SCIP:
* GMP (https://gmplib.org/) for rational arithmetic in ZIMPL, SoPlex, SCIP, and PaPILO
* Boost multiprecision library (https://www.boost.org/) (Version should be 1.70 or newer) for rationals in SCIP and PaPILO
* MPFR (MPFR) for approximating rationals with floating-point numbers in SCIP.

Enabling/disabling the exact solving mode is done with parameter `exact/exact_enabled`

## Step by step guide:

### 1. Build Zimpl:

```
cd zimpl
mkdir build
cmake ..
make
cd ../..
```

### 2. Build SoPlex:

```
cd soplex
mkdir build
cmake ..
make
cd ../..
```

### 3. Build PaPILO:

```
cd papilo
mkdir build
cmake ..
make
cd ../..
```

### 4. Build SCIP:

```
cd scip
mkdir build
cmake .. -DSOPLEX_DIR=<PATH_TO_SOPLEX_BUILD_DIRECTORY> -DPAPILO_DIR=<PATH_TO_PAPILO_BUILD_DIRECTORY> -DZIMPL_DIR=<PATH_TO_ZIMPL_BUILD_DIRECTORY>
make
```
