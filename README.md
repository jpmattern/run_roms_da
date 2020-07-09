# run_roms_da and run_roms

`run_roms_da` is a script to automate [ROMS](https://www.myroms.org/) data assimilation, `run_roms` starts non-assimilative simulations.
 * automated file renaming, starting of ROMS (via call to `mpirun` or via `sbatch` job submission), ROMS observation-file slicing, etc.
 * work with configuration files, storing the information of each data assimilation run in a single file
 * written in bash
 * requires [NCO toolkit](http://nco.sourceforge.net/) and a working ROMS data assimilation setup

### Note

`run_roms_da` and `run_roms` create new files and directories and remove old files, use at your own risk. Data files that are potentially modified by ROMS, such as the initial and observation files are copied by `run_roms_da`, so that the original is left unchanged.

## Installation

No installation required: After downloading, add the directory containing `run_roms_da` to the `PATH` variable or simply execute in a shell using its full path.

## How it works

The `run_roms_da` script requires a working ROMS data assimilation setup, including a ROMS executable and ROMS input and data files. 

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
    ├── ocean_ini_38832.nc
    ├── ocean_mod_38832.nc
    ├── ocean_obs_38832.nc
    ├── roms_38836.log 
    ├── ocean_fwd_38836_000.nc
    ├── ocean_fwd_38836_002.nc
    ├── ocean_ini_38836.nc
    ├── ocean_mod_38836.nc
    └── ocean_obs_38836.nc
```
Here, `run_roms_da.config` is the configuration file for the run and `run_roms_da.log` is the log file containing a copy of the output of `run_roms_da`. The directory `run` contains the current ROMS run (`38840` in each file name indicates the start time of the cycle in days since the ROMS reference date). The output of previous cycles is moved to and stored in the `output` directory; which files are saved can be configured in `run_roms_da.config`.

### Note

The `*.in` input files and the ROMS data files (`ocean_ini_*.nc`, `ocean_stdi_*.nc`) are copies of the files specified in `run_roms_da.config`, and `ocean_obs_38840.nc` is a sliced copy of the ROMS observation file specified in `run_roms_da.config`. The copying is meant to ensure that the original ROMS data files remain unchanged by the call to `run_roms_da` and subsequent ROMS calls. 

## The configuration file

Both `run_roms_da` and `run_roms` use a single configuration file to configure the ROMS simulations which allow modifications to the ROMS input file parameters (like `NAVG` in the `ocean.in` file). The philosophy behind this approach is to keep a small number of template input files containing parameters suitable for a wide range of ROMS simulations, and then modify copies of these templates for each individual simulation.

The configuration file for `run_roms_da` is considerably longer than that for `run_roms` because more information is required for data assimilation experiments than for non-assimilative simulations. All parameters that need to be set have descriptions in the configuration file.

Both configuration files permit modification of ROMS `ocean.in` file parameters via the `paramchanges_ocean` variable which is a bash array. To change the ROMS timesteps between writing time-averaged data (`NAVG`), change the ROMS boundary file (`BRYNAME`) and modify the output of temperature into history files (`Hout(idTvar)`), use for example:
```
paramchanges_ocean=(
NAVG=12
BRYNAME="myromsdir/bry/ocean_bry.nc"
"Hout(idTvar)"="T F"
)
```
Note the use of `"` to avoid syntax errors in the case of `"Hout(idTvar)"` and permit the setting of values with spaces in the case of `"T F"`.


## Authors

* Jann Paul Mattern [jpmattern](https://github.com/jpmattern)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

 

