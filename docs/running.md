
## Requirements

GARD has the following dependencies:

1. A Fortran compiler. GARD has been used with the following compilers:
  - gfortran (version >=4.9)
  - ifort (version >=15)

  GARD has also been compiled using the PGI compiler in the past but PGI does not support newer fortran constructs and is not recommended.  The Cray compiler is also likely to work though compiler options may need to be modified.

1. LAPACK — Linear Algebra PACKage.
1. netCDF4 - Network Common Data Form.

*Note: GARD compiled with intel allocates memory to the stack. Users should set the "The maximum stack size." to "unlimited" prior to building/running GARD. `ulimit -s unlimited`*

## Building GARD

GARD is built using a standard `makefile`.  From the command line, simply run the following command:

```
cd GARD/src/
make FC=gfortran NETCDF=/path/to/netcdf LAPACK=/path/to/lapack
```

Depending on which computer you are running GARD on, you may need to edit one or more of the following variables:

- `FC`: Fortran compiler (e.g. `gfortran`)
- `NCDF_PATH`: path to netCDF installation (see `nc-config --prefix` )
- `LAPACK_PATH`: path to lapack installation (e.g. `/usr/local`)
- `INSTALL_DIR`: path to install GARD executable
- `RM`: path to unix `rm` command
- `CP`: path to unix `cp` command

## Running GARD

After building GARD, it is run on the command line following this syntax:

```
./gard downscale_options.txt
```

A sample config file is provided in the `GARD/run` directory and in the documentation : [downscale_options.txt](downscale_options.txt).

Many options in this file can be left at their default values; however it is necessary to edit the names of the files to be used and the names of the variables in the netcdf files.  The config file specifies the name of text files containing a list of netcdf filenames that will be read.  One file list should be specified for each variable to be read, even if these variables are in the same files. File lists should just be text files with a single file listed per line; it is recommended to add  quotations around the file names. 

## Useful commands
Use the following to generate a list of e.g. GEFS precipitation files for input.

    ls -1 gefs/2010/*/apcp_sfc_*_mean.nc | sed 's/*//g;s/$/"/g;s/^/"/g'>gefs_pr_file.txt

## Common Errors

1. Segmentation Fault
    - GARD allocates memory to the stack. Users should set the "The maximum stack size" to "unlimited" prior to building/running GARD. `ulimit -s unlimited`
2. Random errors (e.g. debug not staying set at False)
    - Make sure all filenames in the namelist are in quotations.
