# Azores high: analysis
The analysis is divided into three parts:
1. __"Observed" expansion and model validation__: we'll plot the "observed" expansion of the Azores High – based on reanalysis output – and compare it to the expansion simulated by the CESM-LME over the overlapping period (approximately 1850-2005). The motivating question is: can we trust the model over the period where it *doesn't* overlap with observations?
2. __Time series analysis of model output and comparison to paleo record__: we'll compare statistics between CESM-LME ensembles with different "external forcing". In particular, we'll look at four types of forcing: (a) greenhouse gas emissions, (b) orbital/solar variability, (c) volcanic activity, and (d) all of the above (denoted "Full" in the paper). We'll also compare the Azores High variability "Full" forcing scenario to the $\delta^{13}C$ variability in the Portuguese stalagmite.
3. __Significance testing__: we'll use a Monte-Carlo approach to test for statistical significance of the detected Azores High expansion.

To run the notebook, download the pre-computed Azores High Area (AHA) index here: [https://drive.google.com/drive/folders/105TW_8MNUMddEIHKYPgou0iXKG8ZsCNd?usp=sharing](https://drive.google.com/drive/folders/105TW_8MNUMddEIHKYPgou0iXKG8ZsCNd?usp=sharing).

## Packages


```python
import xarray as xr
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib as mpl
import pathlib

## Set plotting defaults
sns.set(rc={"axes.facecolor": "white", "axes.grid": False})
```

## Utility functions


```python
def get_empirical_pdf(x, bin_edges=None):
    """
    Estimate the "empirical" probability distribution function for the data x.
    In this case the result is a normalized histogram,
    Normalized means that integrating over the histogram yields 1.
    Returns the PDF (normalized histogram) and edges of the histogram bins
    """

    ## compute histogram
    if bin_edges is None:
        hist, bin_edges = np.histogram(x)

    else:
        hist, _ = np.histogram(x, bins=bin_edges)

    ## normalize to a probability distribution (PDF)
    bin_width = bin_edges[1:] - bin_edges[:-1]
    pdf = hist / (hist * bin_width).sum()

    return pdf, bin_edges
```

## Load data

````{admonition} To-do
Below, specify the folder where the AHA data is saved (i.e., set the variable called ```AHA_save_fp```). The AHA data can be downloaded here: [https://drive.google.com/drive/folders/105TW_8MNUMddEIHKYPgou0iXKG8ZsCNd?usp=sharing](https://drive.google.com/drive/folders/105TW_8MNUMddEIHKYPgou0iXKG8ZsCNd?usp=sharing).
````


```python
## where is the data saved?
AHA_save_fp = pathlib.Path("data/AHA")
```

Load the data


```python
def load_AHA(save_fp):
    """Load AHA data from given directory"""

    ## names of datasets
    names = ["era", "noaa", "lme_full", "lme_orb", "lme_volc", "lme_GHG"]

    ## function to get absolute path to dataset
    get_fp = lambda name: pathlib.Path(AHA_save_fp, f"AHA_{name}.nc")

    ## function to open single dataset and rename it
    open_single = lambda name: xr.open_dataarray(get_fp(name)).rename(name)

    return xr.merge([open_single(name) for name in names])


## Load the data
aha = load_AHA(AHA_save_fp)
```

## LME model validation (Fig 2c)
Let's start by looking at the 25-year rolling count of AHA extremes. First, define a function to count the rolling count of Azores high extremes.

````{admonition} To-do: write function to count extremes
The function should take in an xr.DataArray/xr.Dataset containing the AHA index at each timestep, a cutoff percentile (used to define extremes) and a window size (in years) for the rolling count. The function should return an xr.DataArray/xr.Dataset containing the rolling count of extreme events (centered around each timestep). The function skeleton is:
```python
def count_extremes(AHA, cutoff_per=90.0, window=25):
    """
    Function to get rolling count of Azores High extreme events.

    Args:
    - AHA is xr.DataArray/xr.Dataset containing the AHA index at each time
    - cutoff_perc is percentile value in range (0 and 100) used to define
        'extreme' events
    - window is an integer specifying how many years the rolling window is.
    
    Returns:
    - rolling_count is xr.DataArray or xr.Dataset (type matches that of AHA)
    """

    
    ## To-do
    rolling_count = ...

    return rolling_count
````

````{admonition} Hint: rolling counts
:class: dropdown
Given an xr.DataArray called ```data``` consisting of ones and zeros -- representing whether an event occurred or not -- one can get the 25-year "rolling count" along the year dimension using:
```python
rolling_count = data.rolling(dim={"year" : 25}, center=True).sum()
```
````

````{admonition} My solution
:class: dropdown
```python
def count_extremes(AHA, cutoff_perc=90.0, window=25):
    """Get rolling count of Azores High extreme events.
    Args:
    - cutoff_perc is percentile value in range (0 and 100) used to define
        'extreme' events
    - window is an integer specifying how many years the rolling window is.
    """

    ## get threshold for extreme events
    threshold = AHA.quantile(q=cutoff_perc / 100)

    ## Get boolean array: True if AHA exceeds thresh
    exceeds_thresh = AHA > threshold

    ## put NaNs back in the array
    exceeds_thresh = exceeds_thresh.where(~np.isnan(AHA))

    ## Get rolling count
    rolling_kwargs = dict(dim={"year": window}, center=True, min_periods=window)
    rolling_count = exceeds_thresh.rolling(**rolling_kwargs).mean() * window

    return rolling_count
```
````

```python
## count extremes (from 1850-2005)
extreme_count = count_extremes(aha.sel(year=slice(1850, 2005)))

## make plot
fig, ax = plt.subplots(figsize=(6, 3))

## plot reanalysis
ax.plot(extreme_count.year, extreme_count["noaa"], label="NOAA", c="purple")
ax.plot(extreme_count.year, extreme_count["era"], label="ERA", c="blue")

# ## plot LME mean and min/max range
ax.plot(
    extreme_count.year,
    extreme_count["lme_full"].mean("ensemble_member"),
    label="LME",
    c="orange",
)
ax.plot(
    extreme_count.year,
    extreme_count["lme_full"].max("ensemble_member"),
    c="orange",
    lw=0.5,
)
ax.plot(
    extreme_count.year,
    extreme_count["lme_full"].min("ensemble_member"),
    c="orange",
    lw=0.5,
)

# ax.plot(rolling_count.year, rolling_count["era"])

## label plot
ax.legend()
ax.set_xlabel("Year")
ax.set_ylabel("Count (25-yr rolling)")
plt.show()
```

![png](azores_analysis_files/azores_analysis_12_0.png) 

## LME single-forcing comparison (Fig 3d,3e)

### Functions for loading paleo data and binning by century


```python
def load_paleo_data():
    """
    Function to load Paleo data. Description of data here:
    https://www.ncei.noaa.gov/access/paleo-search/study/37160
    """

    ## data is located at following link:
    fp = r"https://www.ncei.noaa.gov/pub/data/paleo/speleothem/europe/portugal/thatcher2022/thatcher2022-composite_isotope_records.txt"

    ## open dataset using Pandas
    paleo_data = pd.read_csv(fp, skiprows=117, sep=r"\s+").set_index("age_CE")

    ## convert to xarray and rename coordinate "year"
    paleo_data = paleo_data.to_xarray().rename({"age_CE": "year"})

    ## switch order of year coordinate so that it's increasing
    paleo_data = paleo_data.reindex({"year": paleo_data.year.values[::-1]})

    return paleo_data


def floor_nearest100(x):
    """rounds down to nearest 100"""

    return np.floor(x / 100) * 100


def bin_by_century(data, bin_offset=0):
    """
    Function to bin data by century. Args:
    - 'bin_offset' is number in [0, 100) representing
        where the first bin begins.
    """

    ## Get century to label data with
    century = floor_nearest100(data["year"] - bin_offset) + bin_offset

    ## add century as new coordinate on data
    ## (this makes it easy to group data by century)
    data = data.assign_coords({"century": century.astype(int)})

    # get mean for each century
    data_binned = data.groupby("century").mean()

    return data_binned
```

### Compute stats


```python
## Load paleo data
paleo_data = load_paleo_data()

## Get count of extremes on full LME simulation (100-year window)
extreme_count_100 = count_extremes(
    aha.drop_vars(["era", "noaa"]), window=100, cutoff_perc=90
)

## drop NaN values at start/end (from sliding window)
extreme_count_100 = extreme_count_100.sel(year=slice(901, 1956))

## Compute binned average for paleo data
offset = 25  # the first bin begins at year (800 + offset)
paleo_data_binned = bin_by_century(paleo_data, bin_offset=offset)

## compute binned average for AHA ensemble mean
aha_mean_binned = bin_by_century(aha.mean("ensemble_member"), bin_offset=offset)

## combined binned data into single DataArray
data_binned = xr.merge([paleo_data_binned, aha_mean_binned])
```

### Plotting functions


```python
def plot_LME_extremes(ax, extreme_count):
    """function to plot LME extremes on ax object"""

    ## specify colors and linestyles for each LME run
    varnames = ["lme_full", "lme_GHG", "lme_volc", "lme_orb"]
    labels = ["Full", "GHG", "Volcanic", "Solar"]
    colors = ["k", "green", "red", "orange"]
    scales = [1, 0.75, 0.5, 0.5]

    ## Plot lines one-by-one
    for label, varname, color, scale in zip(labels, varnames, colors, scales):

        ## get linewidth based on scale
        mean_width = 2.0 * scale
        range_width = 0.5 * scale

        ## plot ensemble mean
        ax.plot(
            extreme_count.year,
            extreme_count[varname].mean("ensemble_member", skipna=True),
            label=label,
            c=color,
            lw=mean_width,
        )

    ## plot standard deviation for "Full" run
    mean_ = extreme_count["lme_full"].mean("ensemble_member")
    std_ = extreme_count["lme_full"].std("ensemble_member")
    ax.fill_between(
        extreme_count.year, mean_ + std_, mean_ - std_, color="k", alpha=0.07
    )

    ## label plot
    ax.legend(prop={"size": 8})
    ax.set_xlabel("Year")
    ax.set_ylabel("Extreme events")
    ax.set_yticks([5, 10, 15, 20])
    # ax.set_ylim([None, 20])

    return ax


def plot_paleo_comparison(ax, data_binned):
    """function to plot paleo data and AHA over time"""

    ## get edges of bins (used for plotting)
    bin_starts = data_binned.century.values
    bin_edges = np.insert(bin_starts, obj=len(bin_starts), values=bin_starts[-1] + 100)

    ## get color for paleo plot
    color_paleo = sns.color_palette()[0]

    ## plot paleo data first
    paleo_plot = ax.stairs(data_binned["d13C_compSuess"], bin_edges, color=color_paleo)
    ax.set_ylabel(r"$\delta^{13}C$", c=color_paleo)
    ax.set_yticks([-1, -2, -3], labels=[-1, -2, -3], color=color_paleo)
    ax.set_xlim([830, 2020])

    ## plot AHA on same x-axis
    ax1 = ax.twinx()
    ax1.stairs(data_binned["lme_full"], bin_edges, color="k", ls="--")
    ax1.set_ylabel(r"Azores area ($km^2$)")
    ax1.set_yticks([9e6, 11e6, 13e6])
    ax1.set_ylim([9e6, 14e6])

    return ax, ax1
```

### Make plot:


```python
fig = plt.figure(figsize=(7, 5))
gs = mpl.gridspec.GridSpec(2, 1, height_ratios=[0.7, 1])

## plot binned paleo data and AHA over time
ax0 = fig.add_subplot(gs[0])
ax00, ax01 = plot_paleo_comparison(ax0, data_binned)
ax00.set_xticks([])

## Plot extreme count over time
ax1 = fig.add_subplot(gs[1])
ax1 = plot_LME_extremes(ax1, extreme_count_100)

## make sure x-axes match
ax1.set_xlim(ax0.get_xlim())

plt.show()
```

![png](azores_analysis_files/azores_analysis_21_0.png)

## Azores High distribution shift (Fig. 3b)

Split data into pre- and post-industrial, and compute PDFs


```python
## split data into pre-industrial control and historical
aha_PI = aha["lme_full"].sel(year=slice(None, 1850))
aha_hist = aha["lme_full"].sel(year=slice(1851, 2005))

## compute PDFs
pdf_PI, edges = get_empirical_pdf(aha_PI.values.flatten())
pdf_hist, _ = get_empirical_pdf(aha_hist.values.flatten())
```


```python
### Plot result
fig, ax = plt.subplots(figsize=(4, 3))

## plot histogram
ax.stairs(values=pdf_PI, edges=edges, label="PI")
ax.stairs(values=pdf_hist, edges=edges, label="hist")

## label
ax.set_xlabel(r"Azores High area ($km^{2}$)")
ax.set_ylabel("Density")

ax.legend()
ax.set_title("PDF")

plt.show()
```

![png](azores_analysis_files/azores_analysis_25_0.png)


## PDF of extreme events (Fig. 3c)

### Empirical PDFs

First, let's plot the PDF of extreme events for individual samples and for the ensemble mean.


```python
## specify bin edges
pdf, edges = get_empirical_pdf(
    extreme_count_100["lme_full"].values.flatten(), bin_edges=np.arange(1.5, 21.5, 1)
)

## Get sample from last year of LME simulation for comparison
last_sample = extreme_count_100["lme_full"].isel(year=-1)

### Plot result
fig, ax = plt.subplots(figsize=(4, 3))

## plot histogram
ax.stairs(values=pdf, edges=edges, color="gray")

# plot samples from last 100 years
ax.scatter(
    last_sample, xr.zeros_like(last_sample), marker="o", c="r", s=10, label="1905-2005"
)

## label
ax.set_xlabel(r"Extreme events per 100 yrs")
ax.set_ylabel("Density")
ax.set_title("PDF: individual ensemble members")
ax.set_yticks([0, 0.05, 0.1, 0.15])
ax.set_ylim([-0.01, None])
ax.legend()

plt.show()
```

![png](azores_analysis_files/azores_analysis_29_0.png)


Next, do the same thing but for the ensemble mean


```python
## specify bin edges
pdf, edges = get_empirical_pdf(
    extreme_count_100["lme_full"].mean("ensemble_member"),
    bin_edges=np.arange(7 + 1 / 26, 15, 4 / 13),
)

## Get sample from last year of LME simulation for comparison
last_sample = extreme_count_100["lme_full"].isel(year=-1).mean("ensemble_member")

#### Plot result
fig, ax = plt.subplots(figsize=(4, 3))

## plot histogram
ax.stairs(values=pdf, edges=edges, color="gray")

# plot samples from last 100 years
ax.scatter(last_sample, 0, marker="*", c="r", label="1905-2005")

## label
ax.set_xlabel(r"Extreme events per 100 yrs")
ax.set_ylabel("Density")
ax.set_title("PDF: ensemble mean")
ax.set_ylim([-0.05, None])
ax.legend()

plt.show()
```

![png](azores_analysis_files/azores_analysis_31_0.png)


#### Monte-Carlo PDF

Next, let's use a Monte-Carlo approach to check the statistical signficance of the increase in extreme events. Below, draw random samples.

````{admonition} To-do: recreate Fig 3c
Create a Monte-Carlo PDF for the number of extreme events per 100-years, for the ensemble mean. To do this: draw $10^5$ random 100-year samples (with replacement) from each ensemble member in the "Full" forcing ensemble, count the number of extremes in each 100-yr sample, then average across the ensemble members (resulting in $10^5$ random samples of the ensemble-mean extreme count). Call the resulting array ```rand_samples_ens_mean``` if you'd like to plug it into the plotting code which follows.
````

````{admonition} Hint: drawing samples
:class: tip
:class: dropdown
We've actually already counted the number of extreme events in 100 year samples; this variable is called ```extreme_count_100```. Therefore, instead of drawing 100-yr segments, we can just draw single timesteps from ```extreme_count_100```, which represents a 100-yr rolling mean.  

To create an array of $n$ random integers in the range $[0,m]$, you can use:
```python
rng = np.random.default_rng()
rand_ints = rng.choice(m, size=n, replace=True)
```

Then to draw $10^5$ samples from the $i^{th}$ ensemble member, you can use:
```python
rand_idx = rng.choice(len(extreme_count_100.year), size=int(1e5))
rand_samples_i = extreme_count_100["lme_full"].sel(ensemble_member=i).values[rand_idx]
```
````

````{admonition} My solution 
:class: dropdown

```python
## number of random samples to draw
n_samples = int(1e6)

## initialize a random number generator
rng = np.random.default_rng()

## for convenience, get number of years and ensemble size
ensemble_size = len(extreme_count_100.ensemble_member)
n_years = len(extreme_count_100.year)

## draw random indices for sampling
## Note: different set of indices for each ensemble member!
rand_idx = rng.choice(n_years, size=(ensemble_size, n_samples))

## Select the samples from the data
rand_samples = np.take_along_axis(
    arr=extreme_count_100["lme_full"].values, indices=rand_idx, axis=1
)

## Take "ensemble sum" of random samples
rand_samples_ens_mean = rand_samples.mean(0)
```

````


Next, plot the Monte-Carlo PDF and the ensemble mean from the last sample.


```python
## specify bin edges
pdf, edges = get_empirical_pdf(
    rand_samples_ens_mean, bin_edges=np.arange(6 - 1 / 26, 14 + 3 / 26, 1 / 13)
)

## Get sample from last year of LME simulation for comparison
last_sample = extreme_count_100["lme_full"].isel(year=-1).mean("ensemble_member")

#### Plot result
fig, ax = plt.subplots(figsize=(4, 3))

## plot histogram
ax.stairs(values=pdf, edges=edges, color="gray")

# plot samples from last 100 years
ax.scatter(
    last_sample,
    0,
    marker="*",
    c="r",
    label="1905-2005 mean",
)

## label
ax.set_xlabel(r"Extreme events per 100 yrs")
ax.set_ylabel("Density")
ax.set_title("PDF: ensemble mean")
ax.set_ylim([-0.05, None])
ax.legend(prop=dict(size=8))

plt.show()
```

![png](azores_analysis_files/azores_analysis_36_0.png)

