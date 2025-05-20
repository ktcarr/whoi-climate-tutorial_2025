# Assignment 1: model validation
**Due date**: Sep 30, 2024

Imagine you've been tasked with making a regional climate change projection based on output from a global climate model (for example, perhaps the Boston city council is interested in the risk of flooding 30 years in the future). Before looking at the model's projected changes, you need to evaluate how well the model simulates the region's current climate. This is what you'll do in this assignment (we'll get to the projected changes later on in the course).

In particular, you're going to valid the CESM2 climate model's ability to represent regional climate variability. You'll start by choosing a region and defining a climate index for that region. Then you'll compute statistics for the region/variable in both a reanalysis product and the CESM2 climate model. Finally, you'll compare the statistics and assess the model's performance.

The assignment is broken down into 4 parts, detailed below (some [hints](hints) are included as well). For each part, we've highlighted what to include in your submission in a "To-do" box (you can turn in a jupyter notebook or .pdf/.doc file with solutions). A [marking guide](marking_guide) is provided at the end of the assignment, showing the number of points for each part (note 3/10 of total points come from labeling plots!).

Feel free to copy and paste code from the [in-class example](../tutorial_9-19/example.ipynb) and please reach out if you run into coding issues (the intent of this assignment is not to assess your programming ability!).

## 1) Choose a region and a climate "index"
Start by defining a regional "climate index". The index should be a scalar metric which can be evaluated at every timestep. For example, in class we defined the Woods Hole sea surface temperature (SST) metric as the SST averaged along the coastline close to Woods Hole. For the assignment, choose a different location and/or a different variable (e.g., sea level pressure in the North Pacific). Filepaths to several variables are provided [below](filepaths). You may also want to change how the index is computed (e.g., averaging over a larger/smaller area, or defining an index as the difference between two area averages).


```{admonition} To-do
In your submission, include a plot of your region (e.g., showing the first timestep of the reanalysis data) and description of your index. Make sure your plots are labeled (e.g. label axes, labels, colorbars, etc.).
```

## 2) Compute statistics for the reanalysis
After defining a regional climate index, make a few diagnostic plots of the region and index. The goal is to get a sense for the climate in the region. E.g., some questions to think about may include:
- what is the mean state in your region and how big are typical fluctuations?
- how big are these fluctuations relative to the seasonal cycle?
- what is the timescale of these fluctuations?
- are fluctuations of your index correlated with larger-scale fluctuations?

To address these questions, some possible diagnostic plots include:
- spatial plots of mean, variance
- line/bar plot of index's seasonal cycle (e.g., showing mean and variance for each month)
- histogram of index anomalies (Does the distribution look normal?)
- power spectral density of the index
- spatial plot showing correlation between your index and data at each gridpoint 

```{admonition} To-do
In your submission, include a few (3-4) diagnostic plots of the climate in your region.
```

## 3) Compare statistics between model and reanalysis
### 3a) Compute statistics for CESM2 model
Your next task is to evaluate how well the CESM2 model simulates the climate in your region. To do this, start by computing the same statistics as above – e.g. mean, variance, power spectral density – but for the model instead of the reanalysis (note: [the model's spatial coordinates may be labeled differently than the reanalysis's](relabel_hint)). *You don't need to include anything from this part in your write-up.*

```{warning}
Make sure you compute the statistics for each dataset over an overlapping time period (e.g., if the reanalysis spans 1979–present and the model spans 1950–2014, you would compute the statistics for each dataset over the period 1979–2014).
```

### 3b) Comparison plots
Next, compare the model's statistics to the reanalysis's statistics by making a few plots. One suggestion is to overlay the index-related statistics from both reanalysis and model onto the same plot (e.g., seasonal cycle, histogram of anomalies, power spectral density). Another is to plot the *difference* between the statistics of each dataset. 

Looking at the difference may be useful for spatial plots but can be challenging because [the reanalysis and model data are often on different grids](regrid_hint). If you don't want to deal with this regridding challenge, you could just plot the spatial fields side-by-side.

```{admonition} To-do
In your submission, include a few (3-4) comparison plots between the reanalysis and model datasets.
```

## 4) Assess the model's performance
Finally, assess how well the model's statistics matches the reanalysis's statistics. Some questions to think about include:
- What's your overall impression of the model's performance?
- How well does it represent the statistics of your index?
- Would the model do a better job if you modified your index (e.g., increased the area for spatial averaging, or looked at annual rather than monthly averages?).

```{admonition} To-do
In your submission, write 3-4 sentences summarizing your takeaways.
```

(marking_guide)=
## Marking guide
Part | # Points | Breakdown
:-- | --: | --:
(1) Index definition | 2 | 1 pt for plot & index description + 1 pt for plot labels
(2) Reanalysis diagnostics | 2 | 1 pt for plots + 1 pt for plot labels
(3) Model vs. reanalysis diagnostics | 3 | 2 pts for plots + 1 pt for plot labels
(4) Interpretation | 3 | 3 pts for written response
**Total** | 10 |

(filepaths)=
## Filepaths
Below are filepaths to several variables saved on WHOI's CMIP data server for the ERA5 reanalysis and the CESM2 model. Data for the ERA5 reanalysis is from 1979–2022 and data for the CESM2 model is from the "historical" run, 1950-2014. **Note**: if you copy and paste the paths below, make sure to update ```server_fp``` to reflect the location of the CMIP server on your PC.
```python
from pathlib import Path

## filepath to the CMIP server on your PC 
## TO-DO: update this!
server_fp = Path(...)

## Filepath to the ERA5 reanalysis
era5_fp = Path("cmip6/data/era5/reanalysis/single-levels/monthly-means")

## Filepath to the CESM2 historical model output
cesm_fp = Path("cmip6/data/cmip6/CMIP/NCAR/CESM2/historical/r1i1p1f1")

## sea surface temperature (SST) filepaths
era5_fp_sst = server_fp / era5_fp / Path("sea_surface_temperature")
cesm_fp_sst = server_fp / cesm_fp / Path("Omon/tos/gr/1")

## 2m-temperature (T2m) filepaths
era5_fp_t2m = server_fp / era5_fp / Path("2m_temperature")
cesm_fp_t2m = server_fp / cesm_fp / Path("Amon/tas/gn/1")

## sea level pressure (SLP) filepaths
era5_fp_slp = server_fp / era5_fp / Path("mean_sea_level_pressure")
cesm_fp_slp = server_fp / cesm_fp / Path("Amon/psl/gn/1")

## precipitation filepaths
era5_fp_pre = server_fp / era5_fp / Path("total_precipitation")
cesm_fp_pre = server_fp / cesm_fp / Path("Amon/pr/gn/1")
```
To list all of the NetCDF files in a given directory, you can use ```.glob("*.nc")```. For example, to list the SST files for the ERA5 reanalysis, use ```list(era5_fp_sst.glob("*.nc"))```.

(hints)=
## Hints

(relabel_hint)=
### Relabeling coordinates
The spatial coordinates for renanalysis and model datasets may have different names (e.g., "lon" vs. "longitude") and orderings/conventions (e.g., latitude in ascending vs. descending order, or longitudes ranging from $0^{\circ}\to 360^{\circ}$ vs. $-180^{\circ} \to 180^{\circ}$). These differences may cause errors if you try to apply the same computation/plotting functions from Part 2 to the model output. 

One way around this is to rename/reorder the model output coordinates to match those of the reanalysis before trying to compute statistics. The following ```xarray``` functions may be useful for this:
```python
## rename data's coords from "lat"/"lon" to "latitude"/"longitude"
data = data.rename({"lat":"latitude", "lon":"longitude"})

## reverse direction of "latitude" coordinate
data = data.reindex({"latitude" : data["latitude"].values[::-1]})
```

(regrid_hint)=
### Regridding data
It's likely that the spatial coordinates for the reanalysis and model will differ (e.g., one may have a datapoint every $0.25^{\circ}$ while the other has a datapoint every $1^{\circ}$). If you'd like to do a gridpoint-by-gridpoint comparison (e.g., for a spatial plot) you may need to interpolate or "regrid" one of the datasets to match the other. Some possible tools for this include:
```python
## 1) interpolate reanalysis data onto model's grid using xarray
reanalysis_data = reanalysis_data.interp_like(model_data)

## 2) same, but using xesmf package
## (useful if one of the grids is not "regular")
import xesmf as xe
regridder = xe.Regridder(ds_in = reanalysis_data, ds_out=model_data)
reanalysis_data = regridder(reanalysis_data)
```
