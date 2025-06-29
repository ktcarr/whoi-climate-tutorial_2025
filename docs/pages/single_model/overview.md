# 3) Single-model climate-change

## Recap
In the first two tutorials, we (i) analyzed the variability of a climate index in the "real world" (a reanlysis dataset) and (ii) assessed the CESM1 climate model's ability to represent this variability. Now that we have an idea of the model's ability to represent our climate index, we're going to look at the model's projected future changes in the index. 


## Goal for tutorial 

To look at projected changes this, we're going to use a CESM simulation forced with the RCP8.5 protocol[^1], which spans the simulated years 2006 to 2080 (the corresponding "historical" simulation spans the years 1920-2006). The goal is to estimate the "forced component" of our climate index (in practice, the trend over time) and assess its statistical significance.

[^1]"RCP" stands for "representative concentration pathway"; the number references a 8.5 $W/m^2$ radiative forcing. RCP8.5 was the "worst-case" emissions scenario used in CMIP5.



## Example: Woods Hole and the North Atlantic "warming hole"
(see notebook on the [following page](woods-hole_example.ipynb) for code to reproduce results)  

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
Spatial pattern of SST change in the North Atlantic. White contours show the 1980-2010 climatology, in 4$^{\circ}$C increments. Western-most black box outlines region used to compute Woods Hole climate index. Eastern-most black box outlines the North Atlantic "warming hole" region[^2].
```

3. Next, we'll compare the robustness of the warming trend for the Woods Hole index (Fig 1) to that over the so-called North Atlantic "warming hole"[^2] (outlined by larger black box in Fig 2). For each region, we plot histograms of the area-average temperature for the periods (i) 1980-2010 and (ii) 2050-2080.
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

[^2]Keil, P. et al. "Multiple drivers of the North Atlantic warming hole." *Nat. Clim. Chang. 10*, 667–671 (2020).




## To-dos
0. Run the example.
1. Adapt example to look at trend / internal variability in a different region and/or using a different variable (see example for details about where to make these changes).
2. Re-run the analyses using your index.

### Advanced (optional)
1. Compute linear trend at each grid point (e.g., for period 2006-2080, when the warming signal looks more linear in Figure 1). Then, plot the slope of each linear trend as a spatial map (as in Fig 2).


### Other resources


```{tableofcontents}
```


