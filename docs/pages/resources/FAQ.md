# FAQ

## How to handle large datasets?
If the dataset is small enough to fit into your PC's RAM (data size $< ~\sim 8 GB$ for a laptop), I'd suggest loading it directly into memory. If not, use [Dask](https://docs.dask.org/en/latest/array-best-practices.html) (```xr.open_mfdataset``` defaults to using Dask if Dask is installed). Here are some tips to speed up your code if you're using Dask: [https://docs.xarray.dev/en/latest/user-guide/dask.html#optimization-tips](https://docs.xarray.dev/en/latest/user-guide/dask.html#optimization-tips).

While I'm not qualified to answer this question, one thing that's helpful in practice is saving intermediate results, so that you only have to do the "heavy" computations once. For example, if I wanted to compute an index which depended on a 3-D ocean variable (e.g, meridional overturning circulation), I would (i) test the index on a small subset of the data (e.g., two timeslices instead of the full dataset), (ii) apply it to the full dataset (e.g., 1950-present), and (iii) save the result. Then you could load in the saved result for data analysis.

As a special case for dealing with data which is saved in multiple files (e.g., each year of ERA5 is saved in a different file), one trick is to use the ```preprocess``` argument of [```xr.open_mfdataset```](https://docs.xarray.dev/en/stable/generated/xarray.open_mfdataset.html). For example, if you want to compute an index for each time slice using a function ```compute_idx```, you can pass this as an argument when opening the data: ```xr.open_mfdataset(..., preprocess=compute_idx)```. Then ```xarray``` will apply the function to each file separately (in theory this sounds nice, but it hasn't led to huge speed-ups when I've tried to use it).

## How to resolve ```NetCDF: HDF``` error?
(typically seen when trying to open data on the CMIP6 server using xarray or NetCDF packages)

### On Mac
Add the following line to your .bashrc, .bash_profile, or .zshrc file {sup}`1`
```bash
export HDF5_USE_FILE_LOCKING=FALSE
```
{sup}`1`These files should be located in your home folder (you can navigate to your home folder by entering ```cd ~``` in the terminal).

### On Windows (untested solution)
*(Note: this solution was suggested by IS at WHOI but hasn't been tested yet)*. On Windows, we're going to do the same thing as on Mac (set the ```HDF5_USE_FILE_LOCKING``` variable to ```FALSE```), but the process for setting environment variables is different. [This page from Microsoft](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.4#set-environment-variables-in-the-system-control-panel) describes how to set the environment variables ([this blog post](https://www.architectryan.com/2018/08/31/how-to-change-environment-variables-on-windows-10/) shows a more detailed example). To try this potential solution, follow the instructions at the links above for setting an environment variable, and add a new variable called ```HDF5_USE_FILE_LOCKING``` with a value of ```FALSE```.

__Note__: for both solutions, you may need to restart your shell/terminal and Jupyter session for changes to take effect.
