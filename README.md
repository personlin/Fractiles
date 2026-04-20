# Fractiles

A Fortran program that computes **fractiles** (percentile hazard curves and uniform hazard spectra) from probabilistic seismic hazard analysis (PSHA) runs via Monte Carlo simulation.

Compatible with **Haz45.3**, Haz45.2, and Haz45.1.

## Overview

`Fractiles` post-processes output from the seismic hazard code (Haz45) to propagate **epistemic uncertainty** — including uncertainty in ground-motion models, fault parameters, fault widths, dip angles, segment models, and fault types — into fractile hazard curves and uniform hazard spectra (UHS).

## Source Files

| File | Description |
|------|-------------|
| `fract_main.f` | Main program (`Fract_Haz45`); Monte Carlo sampling loop |
| `rd_input.f` | Reads PSHA run input and ground-motion model weights |
| `rd_flt.f` | Reads fault/source data for Haz45.1, 45.2, and 45.3 formats |
| `random.f` | Random number generator utilities |
| `fract.h` | Shared array-dimension parameters (included by all source files) |

## Building

Compile all Fortran source files together with a standard Fortran 77/90 compiler, e.g.:

```bash
gfortran -O2 -o Fractiles \
  "Fractiles Files/fract_main.f" \
  "Fractiles Files/rd_input.f" \
  "Fractiles Files/rd_flt.f" \
  "Fractiles Files/random.f"
```

> **Note:** `fract.h` must be in the same directory as the source files (or on the include path) when compiling.

## Usage

Run the executable and provide an input filename when prompted:

```
./Fractiles
Enter the input filename.
Run_Fractiles.txt
```

### Input File Format

The run-control file specifies (in order):

1. Random seed (`iseed`)
2. Number of Monte Carlo samples (`nSample`)
3. Number of spectral periods (`nPer`)
4. Number of hazard levels and their values
5. Path to the output file
6. For each period: period index, followed by PSHA run input file and source/fault file paths

Refer to the example input files in the `PEER_Verification_Test/` and `Reference_Test/` directories for a complete illustration.

## Test Cases

### PEER Verification Test (`PEER_Verification_Test/`)

Verification tests using PEER benchmark hazard results. Contains:
- `Fractiles/Input/` — run-control and PSHA source files
- `Fractiles/Output/` — expected fractile output
- `Hazard/Input/` & `Hazard/Output/` — corresponding hazard run files

### Reference Test (`Reference_Test/`)

An expanded test that includes **epistemic uncertainty in fault dip and fault width**. Contains the same `Input/` / `Output/` structure under `Fractiles/` and `Hazard/`.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
