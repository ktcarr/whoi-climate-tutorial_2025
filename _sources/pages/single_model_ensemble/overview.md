# 4) Single-model ensemble 

## Recap of lecture
Today in lecture, we talked about (single-model) ensembles. The benefit of using an "ensemble" of simulations -- rather than a single simulation -- is that it becomes easier to separate the climate system's "forced" response to external forcing from its natural, "internal" variability. The forced response (the "climate change signal") is often estimated as the mean of all simulations in the ensemble at a given timestep. By definition, "internal variability" is the difference between the total signal and the forced component (i.e., "total" minus "forced"). The idea is that by averaging over ensemble members, we "filter out" the naturally ocurring month-to-month and year-to-year variations in the climate (e.g., due to El Niño and La Niña). This works because this natural variability occurs independently in each ensemble member (e.g., El Niño events are not synchronized across ensemble members[^1]). 

It's possible to estimate the "external forcing" signal without an ensemble: for example, a linear trend fit to a single ensemble member filters out the higher-frequency fluctuations associated with natural climate variability, and provides an estimate of the climate to gradually increasing GHG emissions. Ensembles make it possible to estimate the time-varying nature of the response (e.g., the response to volcanic eruptions is typically fast and short-lived compared to the response to gradually-increasing GHG emissions). They also make it easier to estimate the uncertainties associated with the forced response and diagnose changes in higher-order statistics (e.g., will the standard deviation of SST increase?): for example, with a 35-member ensemble of simulations, each 30-year period of the simulation has 35x30=1,050 simulation years. 

[^1] So if there is a 20\% chance of an El Niño in any given year, there is a $\left(20\%\right)^3=0.8\%$ chance that three particular ensemble members will all have an El Niño in a given year. Note that external forcing -- e.g., volcanic eruptions -- may affect these probabilities...


## Goal for tutorial 
Today, we're going to going to use the CESM1 large ensemble ("CESM-LENS") to estimate the forced response to the RCP8.5 emissions scenario (we'll use the same Woods Hole SST-based index as in previous tutorials). After estimating the forced response (using up to 35 ensemble members), we'll look at its seasonality (and estimate its robustness). We'll also do one example of estimating changes in the standard deviation of SST.

## Example: forced changes in Woods Hole-adjacent SST
(see notebook on the [following page](woods-hole_example.ipynb) for code to reproduce results)  

1. First, we'll estimate the (annual-average) forced response in our climate index: 
```{figure} figs/forced-response.svg
---
height: 250px
name: forced-resopnse 
---
```

2. Next, we'll look at the forced response in March and September
```{figure} figs/forced-response_by-seasonal.svg
---
height: 250px
name: forced-response_by-season 
---
```


3. The March/September responses seem to happen on slightly different timescales; we'll estimate the robustness by looking at histograms of changes for the first half of the simulation and for the second half:
```{figure} figs/histograms.svg
---
height: 250px
name: forced-response_histograms 
---
```

4. As an example of looking at changes in higher-order statistics, we also plot the change in standard deviation of SST between the first 30 years and last 30 years of the simulation:
```{figure} figs/sigma-change.svg
---
height: 250px
name: sigma-change 
---
```


## To-dos
0. (optional) Run the example. Note that loading the data from all 35 ensemble members is time-consuming ($\sim 30$ minutes). As noted in the example, two options for reducing this time are (i) loading a subset of ensemble members or (ii) [use Google Colab](../resources/cesm_cloud.ipynb). We'd suggest starting with fewer ensemble members.
1. Adapt example to look at forced response / internal variability in a different region and using a different index (see example for details about where to make these changes).
2. Re-run the analyses using your index.

### Advanced (optional)
1. Pick a different variable other than SST.
2. Look at change in autocorrelation over time.
3. Look at change in standard deviation as a function of season.


### Other resources


```{tableofcontents}
```


