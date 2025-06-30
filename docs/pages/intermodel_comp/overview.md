# 5) Intermodel comparison 

## Recap
In the most recent tutorial, we diagnosed the climate change signal in the CESM1 climate model using an ensemble of simulations. Single-model ensembles -- like we analyzed -- are nice because they make it easy to separate the forced signal from internal variability (e.g., by computing the ensemble mean). While this analysis gave us insight into what climate change looks like in the CESM1 climate model, CESM1 -- like all climate models -- has biases which may impact its representation of the forced response to climate change. One option to mitigate the bias of any individual model is to use *multiple* models (i.e., a multi-model ensemble rather than a single-model ensemble). 


## Plan for tutorial 
In this tutorial, we'll analyze the results of the $1\%$ / year $CO_2$ experiment in seven different climate models. In this forcing scenario, atmospheric $CO_2$ concentration increases exponentially ($1\%$ / year) starting from an 1850 pre-industrial control (we'll look at the first 150 years of the simulation). We'll look at the ensemble-mean change (an estimate of the "forced reponse") by comparing the last 30 years of the simulation to the first 30 years (ideally, we'd diagnose the change by comparing the simulation to a pre-industrial baseline). Then we'll look at timeseries of the response in each model.



## Example
(see notebook on the [following page](example.ipynb) for code to reproduce results)

1. First, we'll look at the different mean states between two of the models in the ensemble. Here we calculate the mean state as the first 30 years of the $1\%$ / year simulation, but ideally we'd calculate the mean state from a pre-industrial control run. Note the large difference between the models in the North Atlantic!
```{figure} figs/ensemble-spread.svg
---
height: 600px
name: ensemble-spread
---
Difference between 1850-1880 climatologies of two climate models in the $1\%$ / year $CO_2$ forcing scenario.
```

2. Next, we'll look at the ensemble-mean warming pattern. Note that compared to the [CESM1-only pattern](../single_model/overview.md), the North Atlantic warming hole signal is farther northwards.
```{figure} figs/ensemble-spatial-response.svg
---
height: 200px
name: ensemble-spatial-response 
---
Ensemble-mean change between 1850-1880 and 1970-2000, diagnosed from seven-member, multi-model ensemble.
```

3. Next, we'll look at the change in the Woods Hole climate index over time. Note the large difference in temperatures between each model at the beginning of the simulation.
```{figure} figs/ensemble-trends.svg
---
height: 300px
name: ensemble-trends 
---
Woods Hole climate index in each model. **Left**: raw index. **Right**: same, but each curve is normalized by its mean over the first 30 years of the simulation.
```

4. Lastly, we'll plot the change in the index vs. its initial value in each model.
```{figure} figs/ensemble-scatter.svg
---
height: 300px
name: ensemble scatter
---
Scatter plot showing (i) initial value of Woods Hole climate index, averaged over 1850-1880, and (ii) the change between the periods 1850-1880 and 1970-2000.
```


## To-dos
0. Run the example.
1. Adapt example to look at a different region (see example for details about where to make these changes).
2. Re-run the analyses using your index.


```{tableofcontents}
```




