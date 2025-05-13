# DALES Output

## Enabling output

To save cross sections or 3D fields the following settings are needed in the namelist file for Dales. Output is saved in netCDF format by default.

```
&NAMCROSSSECTION
lcross      = .true.
crossheight = 20,40,80
dtav        = 60
/
```
crossheights is a list of z-levels for which to save horizontal cross sections.

```
&NAMFIELDDUMP
lfielddump  = .true.
dtav        = 60
/
```


## Output with one directory per MPI row
When running with many cores the amount of output files in the run directory becomes annoying (and some supercomputer file systems don't like more than a few thousand files per directory).
With the option `loutdirs` set to true, the output from each row of MPI processes is placed in its own directory: 000, 001, 002, ... .
```
&RUN
loutdirs = .true.
/
```

## Merging netCDF tiles

For 2D and 3D output, each DALES process saves it's own  
The command `cdo` can be used to merge netCDF tiles.
Use cdo version >= 2.0.6.

```
cdo -O collgrid crossxy.0001.*.nc  crossxy.0001.nc
```

`-O` is for overwriting the target file if it exists.

### Selecting variables

```
cdo -O collgrid,vxy crossxy.0001.*.nc  crossxy.0001.nc
```
To include only selected variables in the output, add a comma-separated list after collgrid (no spaces).

### Compression
Optionally add flags to enable compressed netCDF4 output:
```
cdo -f nc4 -z zip_6 -O collgrid crossxy.0001.*.nc  crossxy.0001.nc
```

### NetCDF time units

If both `xyear` and `xday` are present in the DOMAIN namelist,
DALES outputs proper time units in the netCDF: `seconds since 2020-01-02T00:00:00.` Otherwise the unit is just `s`, meaning seconds since the start of the simulation.

### Handling large cases

If cdo runs out of memory, it may help to do the merging in two steps:
first merge each row of tiles into a stripe, then merge the stripes.
To save time, do not enable compression for the intermediate files.

