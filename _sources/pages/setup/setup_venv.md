# Python / virtual environment
First, we'll create a "virtual environment" for the class, which contains python and associated software packages (if you're comfortable with python/virtual environments, feel free to skip ahead to [software dependencies](dependencies)). If you have an account on Poseidon (WHOI's high-performance computing system), and feel comfortable with HPC, you could [set up the virtual environment on Poseidon](../resources/setup_hpc.md) (not recommended if you're not familiar with HPC).

## (optional) Set up virtual environment
If you don't have a preferred package manager, we recommend using ```mamba``` (a faster version of the ```conda``` package manager). To use mamba:
1. Check if conda is installed by typing ```conda info``` at the command line. Also check if mamba is installed (if you have conda but not mamba installed you can run ```conda install -n base -c conda-forge mamba```). If you have neither, download and install miniforge following the instructions here: [https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge).

2. Create a virtual environment for the project with: ```mamba create -n my_new_env``` and activate your environment with ```conda activate my_new_env```

Using a package manager is benefitial because it:
- Allows you to install packages in an isolated environment, preventing clutter in your base Python environment with packages needed only for specific projects.
- Manages dependencies (as some packages rely on others) and resolves version conflicts (since some packages are only compatible with specific versions of others) for you.
- Keeps your system organized.

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
  - cmocean
  - seaborn
  - tqdm
```

**Optional packages**
```
  - xesmf
  - scipy
  - bottleneck
  - h5netcdf
```

If using ```mamba```, the command line syntax to install the packages is:
```
## activate your environment if it is not already
mamba activate my_new_env

## install required dependencies
mamba install -c conda-forge xarray jupyterlab dask netcdf4 cftime cartopy matplotlib cmocean seaborn tqdm

## install optional dependencies
mamba install -c conda-forge xesmf scipy bottleneck h5netcdf
```

(This should also work if you replace ```mamba``` with ```conda```, but will take longer). Note the "```-c conda-forge```" flag tells the package manager to download the packages located from the conda-forge channel. 

```{note}
If you're using conda and the ```conda install ...``` / ```conda env update ...``` commands are taking a long time, you could try [updating the solver to "libmamba"](https://www.anaconda.com/blog/a-faster-conda-for-a-growing-community). If this doesn't work, you could also try setting the channel priority to flexible, with ```conda config --set channel_priority flexible``` (thanks to Lilli Enders for suggesting this).
```

