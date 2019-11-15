# `run_roms_da`

A script to automate [ROMS](https://www.myroms.org/) data assimilation.
 * automates file renaming, starting of ROMS (via call to `mpirun` or via `sbatch` job submission), ROMS observation-file slicing, etc.
 * works with configuration files, storing the information of each data assimilation run in a single file
 * written in bash
 * requires [NCO toolkit](http://nco.sourceforge.net/) and a working ROMS data assimilation setup

## How it works

The `run_roms_da` script requires a working ROMS data assimilation setup, including a ROMS executable and ROMS input and data files. For the ROMS input files (`ocean.in`, `s4dvar.in` etc.) 

1. Copy the [template configuration file](templates/run_roms_da.config) `run_roms_da.config` into a new directory for the data assimilation run.
2. Edit the configuration file.
3. Start the assimilation via a call to `run_roms_da`, typically from inside the directory containing the `run_roms_da.config`.

If not modified via the configuration file (and depending on the ROMS setup used), `run_roms_da` creates a file structure like the following:
```
myrundir
├── run_roms_da.config
├── run_roms_da.log
├── run
│   ├── ocean_38840.in
│   ├── s4dvar_38840.in
│   ├── roms_38840.log
│   ├── ocean_dai_38840.nc
│   ├── ocean_fwd_38840_000.nc
│   ├── ocean_ini_38840.nc
│   ├── ocean_mod_38840.nc
│   ├── ocean_obs_38840.nc
│   └── ocean_stdi_38840.nc
└── output
    ├── roms_38832.log 
    ├── ocean_fwd_38832_000.nc
    ├── ocean_fwd_38832_002.nc
    ├── ocean_obs_38832.nc
    ├── roms_38836.log 
    ├── ocean_fwd_38836_000.nc
    ├── ocean_fwd_38836_002.nc
    └── ocean_obs_38836.nc
```
Here, `run_roms_da.config` is the configuration file for the run and `run_roms_da.log` is the log file containing a copy of the output of `run_roms_da`. The directory `run` contains the current ROMS run (`38840` in each file name indicates the start time of the run in days since the ROMS reference date). Note that the `*.in` input files are copies of the template input files, and `ocean_obs_38840.nc` is a sliced copy of the ROMS observation file specified in `run_roms_da.config`. The output of previous cycles is moved to and stored in the `output` directory; which files are saved can be configured in `run_roms_da.config`.


## Authors

* Jann Paul Mattern [jpmattern](https://github.com/jpmattern)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

 

