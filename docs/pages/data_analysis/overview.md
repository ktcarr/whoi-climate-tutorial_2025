# 1) Climate diagnostics 

## Overview

### Recap of lecture
Today in lecture, we talked about data assimilation and reanalysis products. As discssed, reanalysis products use a combination of observations and models to produce a "best estimate" of climate variables (e.g., temperature, velocity, ozone, etc.) at specified intervals and space and time. The benefit of reanalysis products is that -- unlike observations -- there are no "gaps" in the data (the "gaps" are filled in by climate models), which makes data analysis easier. One important caveat is that there may be large uncertainty about the "true" value of a variable in regions with sparse observations (most reanalysis products also include estimates of this uncertainty). Another is that reanalysis products typically violate conservation principles (e.g., conservation of mass/heat/salt).

### Goal for tutorial 
Today, we're going to analyze sea surface temperature (SST) data from a widely-used reanalysis product, ERA5 (Hersbach et al, 2020). The goal is to become familiar with how to load, preprocess, and analyze (e.g., compute statistics on) gridded climate data. In particular, we'll define a climate "index" and compute some statistics on the index over the recent historical period. We'll use these statistics as baselines for the current climate; later in the short course we'll compute the same statistics on simulations of the future climate to see how they're projected to change over time.

#### To-do list
1. **Define a climate index**: choose a region (e.g., the North Atlantic Ocean) and a variable (e.g., sea surface temperature), then define an "index" which can be evaluated at every timestep (e.g., SST averaged over the North Atlantic).
2. **Preprocess the climate index**: compute "anomalies" by removing the time-mean seasonal cycle.
3. **Plot some diagnostics**: try to characterize variability of the index. Here are a few ideas:
    * Timeseries of anomalies with linear best-fit line
    * Histogram of anomalies
    * Spatial correlation plot (e.g., correlation of your index with SST at each gridpoint)
    * Power spectral density

Rather than doing all this from scratch, we suggest adapting [the example](./example.ipynb), discussed below.

### Example: climate diagnostics for Woods Hole SST
(see notebook on the [following page](example.ipynb) for code to reproduce results)  
1. We'll start by defining an "index", the SST near Woods Hole:
```{figure} figs/sst-sample.svg
---
height: 250px
name: sst-sample 
---
North Atlantic SST during September, 2022. Black star shows Woods Hole.
```

2. After computing the index, we'll compute its seasonal cycle and trend over time (shown here):
```{figure} figs/trend.svg
---
height: 250px
name: trend 
---
Woods Hole climate index (blue line), defined as SST averaged over region 72.5$^{\circ}$–66.5$^{\circ}$W and 39$^{\circ}$–44$^{\circ}$N (time-mean value has been removed). Black line shows linear trend. 
```


3. Finally, we'll look at the statistics of these anomalies, including their probability distribution and their correlation with SSTs in the rest of the North Atlantic:
```{figure} figs/spatial-correlation.svg
---
height: 250px
name: woodshole-correlation
---
SST correlation at each gridpoint with Woods Hole climate index.
```


### Hints
````{dropdown} Defining a climate index
This is where we define the climate index in the example:
```python
def compute_T_wh(x):
    """Compute Woods Hole temperature index"""

    ## define lon/lat range for averaging
    ## (note latitude is in descending order in ERA5)
    region = dict(
        latitude = slice(44, 39), longitude=slice(-72.5, -66.5)
    ) 

    ## get subset of data inside the box
    data_subset = x.sel(region)

    ## compute spatial average
    return spatial_avg(data_subset)
```
````

````{dropdown} Spatial plots
You may have to modify the lon/lat ranges in the plotting functions to show your region. E.g., 
```python
def plot_setup_atlantic(fig):
    """Plot Atlantic region"""

    ## adjust figure size
    fig.set_size_inches(5, 3)

    ## specify map projection
    proj = ccrs.Orthographic(central_longitude=-50, central_latitude=40)

    ## get ax object
    ax = plot_setup(
        fig,
        proj,
        lon_range=[-100, 0],
        lat_range=[10, 70],
        xticks=[-90, -45, 0],
        yticks=[0, 35],
    )

    return fig, ax
```
````

### Links



### References
Hersbach, H. et al. "The ERA5 global reanalysis." *Quarterly Journal of the Royal Meteorological Society 146*, 1999–2049 (2020).



```{tableofcontents}
```


