# Set-up changes

## 1. CMIP server workarounds
### The problem
The CMIP server seems unable to handle multiple users access the same file at once (e.g., during the actual tutorial session). This seemed to manifest in two types of mysterious error messages:
1. ```NetCDF: HDF error```
2. Something along the lines of ```backend engine not found```

Until we figure out how to fix this, we suggest using one of the work-arounds suggested below during the tutorial sessions (outside of the tutorial sessions, when less users are trying to access a given file simultaneously, we expect these errors to be less common). 

### Two workarounds
1. **Use a different variable from the example**: e.g., rather than using sea surface temperature, use 2-meter atmospheric temperature, sea level pressure, or precipitation. Here are filepaths to several variables on the CMIP server (click for downdown):
````{dropdown} ERA5 filepaths on CMIP server
```python
from pathlib import Path

## filepath to the CMIP server on your PC 
## TO-DO: update this!
server_fp = Path(...)

## Filepath to the ERA5 reanalysis
era5_fp = Path("cmip6/data/era5/reanalysis/single-levels/monthly-means")

## sea surface temperature (SST)
era5_fp_sst = server_fp / era5_fp / Path("sea_surface_temperature")

## 2m-temperature (T2m)
era5_fp_t2m = server_fp / era5_fp / Path("2m_temperature")

## sea level pressure (SLP)
era5_fp_slp = server_fp / era5_fp / Path("mean_sea_level_pressure")

## precipitation
era5_fp_pre = server_fp / era5_fp / Path("total_precipitation")
```
````

2. **Use data from "the cloud"**: both Google and Amazon host lots of climate data in the cloud. Example scripts will include an option to ```LOAD_FROM_CLOUD```. To use data from the cloud, you'll have to install the following packages: ```gcsfs```, ```s3fs```, and ```intake-esm```. You can install them with ```mamba``` using:
```
mamba install -n my_new_env -c conda-forge gcsfs s3fs intake-esm
```
where ```my_new_env``` is the name of your virtual environment.

## 2. Install regridding package
To make it easier to switch between data from the CMIP server and from the cloud, we've updated the examples to regrid data onto a fixed grid (data on the CMIP server and the cloud servers use different longitude/latitude grids). Running the examples will now require the ```xesmf``` python package, which can be installed into an environment called ```my_new_env``` with:  
```
mamba install -n my_new_env -c conda-forge xesmf
```