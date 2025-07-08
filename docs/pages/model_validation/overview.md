# 2) Model validation 

## Overview

### Recap of lecture
Today in lecture, we discussed climate models and their assessment. Climate models have been developed painstakingly over decades, and are a cornerstone of climate science. For example, they are how the scientific community makes projections about future climate change (e.g., "if we continue emitting $CO_2$ at the current rate, how much will global temperatures rise in the next 50 years"?). 

Climate models are also very computationally expensive to run, necessitating that they use a relatively "coarse" spatial resolution (distance between gridcells). For example, the width of a gridcell in CESM2 is on the order of 100 km, approximately the distance from Boston to Woods Hole. This means that, from the perspective of CESM2, Boston and Woods Hole -- and everywhere in between -- have exactly the same conditions at all times. If you've ever driven into a wall of fog crossing the Cape Cod canal (going from Boston to Woods Hole), you know this isn't true (another example: today, on July 8, 2025, the temperaure high was 35$^{\circ}$C in Boston and 26$^{\circ}$ in Woods Hole).

```{figure} figs/cesm2-cell.png
---
height: 250px
name: validation_cesm2-cell 
---
The CESM2 gridcell (black outline) which encompasses Woods hole and Boston.
```

While this coarse spatial resolution clearly degrades climate models' ability to simulate small-scale processes, it also degrades their ability to simulate *large-scale* features. This is because in the real world, the small-scale processes "within the gridcells" (i.e., those *not* simulated by the model) impact the large-scale features. In practice, this means that the large-scale climate simulated by the model looks different than the observed climate (e.g., the mean temperature is too high/low).  

Owing to these deficiencies, we typically assess a climate model's ability to simulate the current climate before using it to project future changes. One way to do this assessment is to compare the statistics between the climate model's simulation and observations (or a reanalysis product). As discussed in class, some metrics might include: mean, standard deviation, linear trend (over time), power spectrum, autocorrelation, etc. It's good to be aware of mismatches between the simulated and observed statistics, and to think about what limitations these mismatches might imply (e.g., "can the model be trusted to simulate future change in a given region"?). While there's no clear "best" climate model, certain models may simulate certain regions and large-scale features better than others. Note as well that defining "best" is difficult: just because the model's statistics look "right" doesn't mean it's simulating the processes correctly: if the model has compensating errors, it might get the "right answer" for the wrong reason.

### Goal for tutorial 
Today, we're going to assess the CESM climate model's ability to represent variability in our index/region (Danabasoglu, G. et al., 2020). We'll compare CESM's statistics to those from ERA5 (a reanalysis product, which we'll treat as our observational "ground truth"). In the previous tutorial, we defined an index and computed statistics for this index (e.g., linear trend and histogram). Today, we'll re-compute several of these statistics, for both ERA5 and CESM, and decide whether CESM is a trustworthy model for projecting future changes to this index. Next time, we'll look at the projected changes.

#### To-do list
0. **Pick a climate index**: we suggest using the same index as last time!
1. **Pick some metrics**: In the example, we compare the probability distribution of the index in (i) ERA5 and (ii) CESM2. We also look at differences in the time-mean spatial pattern. Several other useful metrics -- also listed above -- may include linear trend, power spectrum, and autocorrelation in time.
2. **Compute the metrics and make a comparison plot**: e.g., plot the difference between the mean states (the "bias") or overlay the histograms on a single plot.
3. **Make your assessment**: is the model doing a "good" job? Would you trust its future projections in the region you're looking at? If not, does modifying the index give a better match? (e.g., average over a larger region).

Rather than doing all this from scratch, we suggest adapting [the example](./example.ipynb), discussed below.

### Example: CESM's biased representation of Woods Hole SST
(see notebook on the [following page](example.ipynb) for code to reproduce results)  
1. In the CESM climate model, SST near Woods Hole is much warmer than in the real world:

```{figure} figs/timeseries.svg
---
height: 200px
name: validation_timeseries 
---
Timeseries of Woods Hole temperature index in ERA5 and the CESM climate model (both averaged over Jul-Aug-Sep for each calendar year).
```

2. It's also much less variable (note the tails of the distribution):

```{figure} figs/histogram.svg
---
height: 200px
name: validation_histogram 
---
Histogram of Woods Hole temperature index in ERA5 and CESM (monthly data, taken from all calendar months).
```

3. Interestingly, this large positive bias seems to be a relatively local feature: CESM is *too cold* over lots of the North Atlantic. Perhaps the cold bias in the eastern equatorial Pacific (west of the Sahara) is related to poor representation of clouds?

```{figure} figs/spatial-bias.svg
---
height: 300px
name: validation_spatial-bias
---
**Mean-state SST bias in CESM**. **Left**: time-mean SST from ERA5, for Jul-Aug-Sep season. **Center**: same as left, but for CESM. **Right**: difference between CESM and ERA5.
```


### References
Danabasoglu, G. et al. "The Community Earth System Model Version 2 (CESM2)". *J Adv Model Earth Syst* 12, e2019MS001916 (2020).




```{tableofcontents}
```





```{tableofcontents}
```


