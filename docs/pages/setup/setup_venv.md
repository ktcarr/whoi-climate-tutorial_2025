# Python / virtual environment
First, we'll create a "virtual environment" for the class, which contains python and associated software packages (if you're comfortable with python/virtual environments, feel free to skip ahead to [software dependencies](dependencies).

## (optional) Set up virtual environment
If you don't have a preferred package manager, I recommend using ```mamba``` (a faster version of the ```conda``` package manager). To use mamba:
1. Check if conda is installed by typing ```conda info``` at the command line. If not, download and install miniforge following the instructions here: [https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge).
2. Create a virtual environment for the project with: ```mamba create -n 12860_env``` and activate the environment with ```conda activate 12860_env```

(dependencies)=
## Install python packages
Next, we'll install python packages needed for the tutorials and class assignments:

**Required packages**
```
  - xarray  
  - jupyterlab
  - dask
  - netcdf4
  - cftime
  - cartopy
  - matplotlib
```

**Optional packages**
```
  - cmocean
  - seaborn
  - xesmf
  - scipy
  - bottleneck
  - h5netcdf
```

If using ```mamba```, the command line syntax to install the packages is:
```
## install required dependencies
mamba install -c conda-forge xarray jupyterlab dask netcdf4 cftime cartopy matplotlib

## install optional dependencies
mamba install -c conda-forge cmocean seaborn xesmf scipy bottleneck h5netcdf
```

(This should also work if you replace ```mamba``` with ```conda```, but will take longer). Note the "```-c conda-forge```" flag tells the package manager to download the packages located from the conda-forge channel. 

```{note}
If you're using conda and the ```conda install ...``` / ```conda env update ...``` commands are taking a long time, you could try [updating the solver to "libmamba"](https://www.anaconda.com/blog/a-faster-conda-for-a-growing-community). If this doesn't work, you could also try setting the channel priority to flexible, with ```conda config --set channel_priority flexible``` (thanks to Lilli Enders for suggesting this).
```

