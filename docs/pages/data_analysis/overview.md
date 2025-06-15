# 1) Climate diagnostics 

<!---
- Include EOFs here?
- show histogram/box and whisker?
-->

## Overview

### Recap of lecture
Today in lecture, we talked about data assimilation and reanalysis products. As discssed, reanalysis products use a combination of observations and models to produce a "best estimate" of climate variables (e.g., temperature, velocity, ozone, etc.) at specified intervals and space and time. The benefit of reanalysis products is that -- unlike observations -- there are no "gaps" in the data (the "gaps" are filled in by climate models), which makes data analysis easier. One important caveat is that there may be large uncertainty about the "true" value of a variable in regions with sparse observations (most reanalysis products also include estimates of this uncertainty). Another is that reanalysis products typically violate conservation principles (e.g., conservation of mass/heat/salt).

### Goal for tutorial 
Today, we're going to analyze sea surface temperature (SST) data from a widely-used reanalysis product, ERA5 (Hersbach et al, 2020). The goal is to become familiar with how to load, preprocess, and analyze (e.g., compute statistics on) gridded climate data. In particular, we'll define an "index" (Cape Cod SST) and compute some statistics on the index over the recent historical period. We'll use these statistics as baselines for the current climate; later in short course we'll compute the same statistics on simulations of the future climate to see how they're projected to change over time.

### Example: climate diagnostics for Woods Hole SST
(see notebook on the [following page](example.ipynb) for code to reproduce results)  
1. We'll start by defining an "index", the SST near Woods Hole:
```{figure} figs/sst-sample.svg
---
height: 250px
name: sst-sample 
---
```

2. After computing the index, we'll compute its seasonal cycle and trend over time (shown here):
```{figure} figs/trend.svg
---
height: 250px
name: trend 
---
```


3. Finally, we'll look at the statistics of these anomalies, including their probability distribution and their correlation with SSTs in the rest of the North Atlantic:
```{figure} figs/spatial-correlation.svg
---
height: 250px
name: woodshole-correlation
---
```


### To-dos
1. Check to make sure you can run the example.
2. Define your own climate index (e.g., pick a different region than Woods Hole). For reference, here's the area-averaged SST index used in the example:
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
3. Re-run the diagnostics from the example with your index. Note that you may need to update/replace spatial plotting functions (e.g., ```plot_setup_woodshole```) depending on the index you pick.


#### Advanced (optional)
1. Pick a different variable other than SST


### Other resources


### References
Hersbach, H. et al. The ERA5 global reanalysis. Quarterly Journal of the Royal Meteorological Society 146, 1999–2049 (2020).



```{tableofcontents}
```


