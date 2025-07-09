# 3) Single-model climate-change

## Recap
In the first two tutorials, we (i) analyzed the variability of a climate index in the "real world" (a reanlysis dataset) and (ii) assessed the CESM1 climate model's ability to represent this variability. Now that we have an idea of the model's ability to represent our climate index, we're going to look at the model's projected future changes in the index. 


## Goal for tutorial 

To look at projected changes this, we're going to use a CESM simulation forced with the RCP8.5 protocol$^{1}$, which spans the simulated years 2006 to 2080 (the corresponding "historical" simulation spans the years 1920-2006). The goal is to estimate the "forced component" of our climate index (in practice, the trend over time) and assess its statistical significance. Next time, we'll look at a more robust method of estimating the forced component (averaging over an ensemble of simulations, each with the same external forcing).

### To-do list
1. **Load data** for (i) the historical period and (ii) the future scenario. In the example below, we'll use output from a CESM1 simulation that was forced with CMIP5-era emissions. The historical component spans 1920-2006 and the future component (forced with the RCP8.5 protocol) spans from 2006 onwards$^{2}$.
2. **Compute an index** of your choice on the data (for both historical and future).
3. **Estimate the long-term trend**: note the data may not follow a linear trend (see example below).
4. **Plot the spatial pattern of warming**: e.g., plot a difference between (i) average over last 30 years and (ii) average over first 30 years; alternatively, plot spatial pattern of trend coefficients.
5. **Assess robustness of the change**: E.g., overlay histograms of your index from the last 30 years and the first 30 years. How robust is the warming signal?

$^{\mathbf{1}}$"RCP" stands for "representative concentration pathway"; the number references a 8.5 $W/m^2$ radiative forcing. RCP8.5 was the "worst-case" emissions scenario used in CMIP5.  
$^{\mathbf{2}}$As a default, data loaded from the server will be 2-meter atmospheric surface temperature from CMIP6 (the variable is called "TREFHT"), where the future scenario is SSP5-8.5, and the cutoff between historical and future periods is 2014.



## Example: Woods Hole and the North Atlantic "warming hole"
(see notebook on the [following page](example.ipynb) for code to reproduce results)

1. First, we'll estimate the "forced component" of the Woods Hole climate index, for the fall season (September/October/November, or "SON"). To do this, we plot raw index over time (gray line on left side), and fit a trend line to the data (in red, we show two trend lines: one linear and one quadratic). The right side shows the difference between the raw timeseries and the quadratic trend. This difference represents the climate system's natural variability.
```{figure} figs/forced-internal-sep.svg
---
height: 250px
name: forced-internal-sep 
---
Historical and projected change to Woods Hole climate index. **Left**: raw index and estimated trends. **Right**: difference between raw index and quadratic (degree 2) trend.
```

2. Next, we'll check: is this warming trend spatially homogeneous? No, it turns out, as shown below.
```{figure} figs/warming-pattern.svg
---
height: 325px
name: warming-pattern 
---
Spatial pattern of SST change in the North Atlantic. White contours show the 1980-2010 climatology, in 4$^{\circ}$C increments. Western-most black box outlines region used to compute Woods Hole climate index. Eastern-most black box outlines the North Atlantic "warming hole" region (e.g., see Keil et al, 2020).
```

3. Next, we'll compare the robustness of the warming trend for the Woods Hole index (Fig 1) to that over the so-called North Atlantic "warming hole" (outlined by larger black box in Fig 2). For each region, we plot histograms of the area-average temperature for the periods (i) 1980-2010 and (ii) 2050-2080.
```{figure} figs/pdfs_total.svg
---
height: 225px
name: pdfs-total 
---
Histograms of area-averaged temperature during SON for (**left**) the Woods Hole SST index and (**right**) North Atlantic warming hole.
```

4. While Figure 3 suggests the means of the distributions for both climate indices have increased, changes in the *variability* of the indices are more subtle, as shown in histograms of the indices after removing the trend: 
```{figure} figs/pdfs_anom.svg
---
height: 225px
name: pdfs-anom 
---
Same as Figure 3, but after removing quadratic trend from both indices (and including data from all seasons, not just SON).
```


### References
Keil, P. et al. "Multiple drivers of the North Atlantic warming hole." *Nat. Clim. Chang. 10*, 667–671 (2020).


```{tableofcontents}
```


