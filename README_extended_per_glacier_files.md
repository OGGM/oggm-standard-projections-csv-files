# oggm-standard-projections 

**Extended documentation of the raw oggm-output files and documentation of the standard (CMIP) projections computed with OGGM**

---
If you are only interested in regional volume or area changes (globally or per RGI region), go to [README.md](https://github.com/oggm/oggm-standard-projections-csv-files)). This extended README describes the data format of the raw-output and how it was created .
---


At the moment, there are only projections available using OGGM v1.6.1 with the preprocessed glacier directory version 2023.3 [https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/](https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/). These projections include the [dynamical spinup](https://docs.oggm.org/en/latest/dynamic-spinup.html), the [new per-glacier geodetic calibration method](https://docs.oggm.org/en/latest/mass-balance-monthly.html), and use the W5E5v2.0 climate dataset [(Lange and others, 2021)](https://doi.org/10.48364/ISIMIP.342217) for calibration. 


We computed all GCMs and scenarios that are currently available at the OGGM cluster. However, you have to choose those scenarios that are suitable and representative for your study. [For example, you could select them after the method of Hausfather et al., 2022)](https://www.nature.com/articles/d41586-022-01192-2) or [aggregate them after their 2100 warming levels (e.g. as in Rounce et al., 2023)](https://doi.org/10.1126/science.abo1324).

In the subfolders, we give the netCDF files for different CMIP and final year options for all of the 19 RGI regions. That means, an example filepath looks like that:
https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2300/RGI06/run_hydro_gcm_from_2000_ACCESS-CM2_ssp126_bc_2000_2019_Batch_0_1000.nc
(the different options are explained in detail below).

Each netCDF file contains the projections from up to 1000 glaciers for a single GCM, scenario, RGI region and historical projection option from 2000 until 2100. We have run the `run_with_hydro` task of OGGM on a monthly basis, and thus, the following variables are included:

"| variable                     | description                                                        | unit       | coords                         |\n|:-----------------------------|:-------------------------------------------------------------------|:-----------|:-------------------------------|\n| volume                       | Total glacier volume                                               | m 3        | ['time', 'rgi_id']             |\n| volume_bsl                   | Glacier volume below sea-level                                     | m 3        | ['time', 'rgi_id']             |\n| volume_bwl                   | Glacier volume below water-level                                   | m 3        | ['time', 'rgi_id']             |\n| area                         | Total glacier area                                                 | m 2        | ['time', 'rgi_id']             |\n| length                       | Glacier length                                                     | m          | ['time', 'rgi_id']             |\n| calving                      | Total accumulated calving flux                                     | m 3        | ['time', 'rgi_id']             |\n| calving_rate                 | Calving rate                                                       | m yr-1     | ['time', 'rgi_id']             |\n| off_area                     | Off-glacier area                                                   | m 2        | ['time', 'rgi_id']             |\n| on_area                      | On-glacier area                                                    | m 2        | ['time', 'rgi_id']             |\n| melt_off_glacier             | Off-glacier melt                                                   | kg yr-1    | ['time', 'rgi_id']             |\n| melt_on_glacier              | On-glacier melt                                                    | kg yr-1    | ['time', 'rgi_id']             |\n| liq_prcp_off_glacier         | Off-glacier liquid precipitation                                   | kg yr-1    | ['time', 'rgi_id']             |\n| liq_prcp_on_glacier          | On-glacier liquid precipitation                                    | kg yr-1    | ['time', 'rgi_id']             |\n| snowfall_off_glacier         | Off-glacier solid precipitation                                    | kg yr-1    | ['time', 'rgi_id']             |\n| snowfall_on_glacier          | On-glacier solid precipitation                                     | kg yr-1    | ['time', 'rgi_id']             |\n| snow_bucket                  | Off-glacier snow reservoir (state variable)                        | kg         | ['time', 'rgi_id']             |\n| model_mb                     | Annual mass balance from dynamical model                           | kg yr-1    | ['time', 'rgi_id']             |\n| residual_mb                  | Difference (before correction) between mb model and dyn model melt | kg yr-1    | ['time', 'rgi_id']             |\n| melt_off_glacier_monthly     |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| melt_on_glacier_monthly      |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| liq_prcp_off_glacier_monthly |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| liq_prcp_on_glacier_monthly  |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| snowfall_off_glacier_monthly |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| snowfall_on_glacier_monthly  |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| snow_bucket_monthly          |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| residual_mb_monthly          |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |\n| water_level                  | Calving water level                                                |            | ['rgi_id']                     |\n| glen_a                       | Simulation Glen A                                                  |            | ['rgi_id']                     |\n| fs                           | Simulation sliding parameter                                       |            | ['rgi_id']                     |"



If a region has in  RGI6 3927 glaciers, then there will be four files going from `*Batch_0_1000.nc` to `*Batch_3000_4000.nc`. Note that, there can be less than 1000 glaciers inside, if it is the last Batch. If you want to get e.g. the volume above sea level projections for all GCMs and glaciers from e.g. a specific ssp and RGI region, you can do for example:
```
import xarray as xr
path = '/home/www/oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2300'
with xr.open_mfdataset(path + 'RGI01/run_hydro_w5e5_from_gcm_*_ssp126_*.nc') as ds:
    ds = ds.volume_asl.load()
```
Note, that some glaciers will have NaN values, so you need to either only use the common running glaciers (the method used to create the aggregated csv files) or do some filling. 
-> More details on how to handle the data (for example to estimate glacier runoff) is given in this jupyter notebook: https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/analysis_notebooks/workflow_to_analyse_per_glacier_projection_files.ipynb?flush_cache=true 




***CMIP option and final year:***
At the moment there are three options here (https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/). For all options, we used the bias-corrected GCMs to project glacier changes until 2100. Only for CMIP6, we also repeated the simulations until 2300. Note, that we did two separate runs, as there are glaciers that fail until 2300, but not until 2100 (specifically in Iceland, RGI06, see link to the error_analysis notebook ...). 

- CMIP6/2100 or CMIP6/2300
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference: [Eyring et al. (2016)](https://doi.org/10.5194/gmd-9-1937-2016)
- CMIP5/2100
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference [Taylor et al., (2012)](https://doi.org/10.1175/BAMS-D-11-00094.1)
- ISIMIP3b_CMIP6/2100
    - internally bias-corrected to W5E5 using bias correction period 1979-2014 [(Lange, 2019)](https://doi.org/10.5194/gmd-12-3055-2019) and interpolated to a 0.5° spatial resolution, we did not apply any additional bias-correction on top of that
    - five primary GCMs from phase 3b of the Inter-Sectoral Impact Model Intercomparison Project (ISIMIP3b) for three different SSPs
    - main references ([Lange, 2019](https://doi.org/10.5194/gmd-12-3055-2019); [Lange, 2022](https://doi.org/10.5281/zenodo.2549631))
*For CMIP5 and CMIP6, we have used  here the same bias correction period as in [Rounce et al., (2023)](https://doi.org/10.1126/science.abo1324), i.e. from Jan 2000 to Dec 2019. However,  using a bias correction period of 1979-2014 instead of 2000-2019 (tested on five GCMs) resulted in glacier volume differences that were globally up to 20% different until 2100 for a single GCM. (analysed by Lilian Schuster, uncleaned notebook with the analysis https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~lschuster/runs_oggm_v16/analysis_notebooks/volume_evolution_common_running_glaciers.ipynb) *    


**Historical projections from 2000-2019** 
Inside of the RGI region folders, there are two options how the historical projections from 2000-2019 are computed:
- `w5e5_gcm_merged`: Here, W5E5 was applied from 2000-2019 and the GCMs were applied from 2020 onwards until 2100. This is the option that we chose for the aggregated files.
    - In the period 2000-2019, the projections are the same for every GCM and scenario (as we use W5E5).
- `gcm_from_2000`: W5E5 is only used to calibrate the mass-balance model. For the actual projections, the individual GCMs (and scenarios) are used form 2000 onwards. 
    - In the period 2000-2019, different GCMs create different output. 
*The influence of the historical projection choice is very small after 2000-2019 (regionally < 2% after 2050, and globally invisible from 2000 onwards). However, the historical projection choice changes the projections over the period 2000-2019 by up to 8% regionally estimates although the CMIP GCMs were bias-corrected to that time period. (analysed by Lilian Schuster, uncleaned notebook with the analysis https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~lschuster/runs_oggm_v16/analysis_notebooks/volume_evolution_common_running_glaciers.ipynb)*



 
 ----
If you want to use the aggregated or the raw per-glacier data, please cite the dataset via:
- zenodo-link ...
In addition, cite OGGM [(Maussion et al., 2019)](https://doi.org/10.5194/gmd-12-909-2019) and the CMIP option that you are using (references are linked below).
---

## Additional informations to rerun the projections

All scripts used to run the projections are in ....TODO ... 