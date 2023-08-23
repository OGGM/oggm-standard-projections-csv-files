# oggm-standard-projections 

OGGM provides what we like to call standard projections. Currently we make these future CMIP forced global glacier simulations available in two different formats, raw and aggregated data. In this document the raw data is described. 

**If you are only interested in regional volume or area changes (globally or per RGI region), we recommend you to use the aggregated data [README.md](https://github.com/oggm/oggm-standard-projections-csv-files).** 

 ----
When you use the aggregated or the raw per-glacier data, please cite the dataset via:
- zenodo-link ...
In addition, cite OGGM [(Maussion et al., 2019)](https://doi.org/10.5194/gmd-12-909-2019) and the CMIP option that you are using (references are linked below).
---

## Extended documentation of the raw oggm-output files and documentation of the standard (CMIP) projections computed with OGGM

***Model set-up***

At the moment, there are only projections available using OGGM v1.6.1 with the preprocessed glacier directory version 2023.3, which you can find here:
[https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/](https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/).

These projections use elevation-band flowlines, include the [dynamical spinup](https://docs.oggm.org/en/latest/dynamic-spinup.html), the [new informed 3-step per-glacier geodetic calibration method](https://docs.oggm.org/en/latest/mass-balance-monthly.html), and use the W5E5v2.0 climate dataset [(Lange and others, 2021)](https://doi.org/10.48364/ISIMIP.342217) for calibration and a border of 160. 

We computed all GCMs and scenarios that are currently available at the OGGM cluster. However, you have to choose those scenarios that are suitable and representative for your study. [For example, you could select them after the method of Hausfather et al. (2022)](https://www.nature.com/articles/d41586-022-01192-2) or [aggregate them after their 2100 warming levels (e.g. as in Rounce et al., 2023)](https://doi.org/10.1126/science.abo1324).

***Data structure***

In the subfolders, we give the netCDF files for different CMIP and final year options for all of the 19 RGI regions. That means, an example filepath looks as follows:

https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2300/RGI06/run_hydro_gcm_from_2000_ACCESS-CM2_ssp126_bc_2000_2019_Batch_0_1000.nc 
(the different options are explained in detail below).

Each netCDF file contains the projections from up to 1000 glaciers for a single GCM, scenario, RGI region and historical projection option from 2000 until 2100. The timestep always corresponds to the glacier state at the beginning of the year, i.e., 2000 means 01-01-2000. We have run the `run_with_hydro` task of OGGM on a monthly basis, and thus, the following variables are included:

| variable                     | description                                                        | unit       | coords                         |
|:-----------------------------|:-------------------------------------------------------------------|:-----------|:-------------------------------|
| volume                       | Total glacier volume                                               | m 3        | ['time', 'rgi_id']             |
| volume_bsl                   | Glacier volume below sea-level                                     | m 3        | ['time', 'rgi_id']             |
| volume_bwl                   | Glacier volume below water-level                                   | m 3        | ['time', 'rgi_id']             |
| area                         | Total glacier area                                                 | m 2        | ['time', 'rgi_id']             |
| length                       | Glacier length                                                     | m          | ['time', 'rgi_id']             |
| calving                      | Total accumulated calving flux                                     | m 3        | ['time', 'rgi_id']             |
| calving_rate                 | Calving rate                                                       | m yr-1     | ['time', 'rgi_id']             |
| off_area                     | Off-glacier area                                                   | m 2        | ['time', 'rgi_id']             |
| on_area                      | On-glacier area                                                    | m 2        | ['time', 'rgi_id']             |
| melt_off_glacier             | Off-glacier melt                                                   | kg yr-1    | ['time', 'rgi_id']             |
| melt_on_glacier              | On-glacier melt                                                    | kg yr-1    | ['time', 'rgi_id']             |
| liq_prcp_off_glacier         | Off-glacier liquid precipitation                                   | kg yr-1    | ['time', 'rgi_id']             |
| liq_prcp_on_glacier          | On-glacier liquid precipitation                                    | kg yr-1    | ['time', 'rgi_id']             |
| snowfall_off_glacier         | Off-glacier solid precipitation                                    | kg yr-1    | ['time', 'rgi_id']             |
| snowfall_on_glacier          | On-glacier solid precipitation                                     | kg yr-1    | ['time', 'rgi_id']             |
| snow_bucket                  | Off-glacier snow reservoir (state variable)                        | kg         | ['time', 'rgi_id']             |
| model_mb                     | Annual mass balance from dynamical model                           | kg yr-1    | ['time', 'rgi_id']             |
| residual_mb                  | Difference (before correction) between mb model and dyn model melt | kg yr-1    | ['time', 'rgi_id']             |
| melt_off_glacier_monthly     |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| melt_on_glacier_monthly      |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| liq_prcp_off_glacier_monthly |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| liq_prcp_on_glacier_monthly  |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| snowfall_off_glacier_monthly |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| snowfall_on_glacier_monthly  |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| snow_bucket_monthly          |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| residual_mb_monthly          |                                                                    | kg month-1 | ['time', 'rgi_id', 'month_2d'] |
| water_level                  | Calving water level                                                |            | ['rgi_id']                     |
| glen_a                       | Simulation Glen A                                                  |            | ['rgi_id']                     |
| fs                           | Simulation sliding parameter                                       |            | ['rgi_id']                     |


If a region has in  RGI6 3927 glaciers, then there will be four files going from `*Batch_0_1000.nc` to `*Batch_3000_4000.nc`. Note that, there can be less than 1000 glaciers inside, if it is the last batch. 
If you want to get e.g. the annual runoff of all glaciers from e.g. a specific gcm, ssp and RGI region, you can get it via:
```
import xarray as xr
path = '/home/www/oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2300/'
with xr.open_mfdataset(path + 'RGI11/run_hydro_w5e5_gcm_merged_CanESM5_ssp126_*.nc') as ds:
    ds['runoff'] = ds['melt_off_glacier']+ds['melt_on_glacier']+ ds['liq_prcp_off_glacier'] +ds['liq_prcp_on_glacier']
    ds = ds['runoff'].load()
```
Note, that some glaciers will have NaN values, so if you aggregate it, you need to either only use the common running glaciers (the method used to create the aggregated csv files) or do some filling. 
The annual glacier runoff that was computed here is the sum of annual melt and liquid precipitation on and off the glacier using a fixed-gauge with a glacier minimum reference area from year 2000 (unit: kg year-1).
- We also have already postprocessed per-glacier annual and monthly runoff files aggregated for every basin ([we used this notebook](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/compute_runoff_for_basins.ipynb), at the moment only for CMIP6 until 2100). The basin files are available here https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2100/basins/. All glaciers of a basin are aggregated in one file, and there are different files for different simulations. The files of each basin are in one subfolder, with the subfolder name being the MRBID of that basin. The basin files contain the variables: volume, area, runoff and runoff_monthly. 

**-> More details on how to handle the data (for example to estimate glacier runoff) is given in [this jupyter notebook](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/analysis_notebooks/workflow_to_analyse_per_glacier_projection_files.ipynb?flush_cache=true)** 


***CMIP option and final year:***
At the moment there are three categories of forcing data that have been used for the simulations (https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/). For all options, we used the bias-corrected GCMs to project glacier changes until 2100. Only for CMIP6, we also repeated the simulations until 2300. Note, that we did two separate runs, as there are glaciers that fail before 2300, but not until 2100 (specifically in Iceland, RGI region 6, see link to the error_analysis notebook ...). 

- CMIP6/2100 or CMIP6/2300
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference: [Eyring et al. (2016)](https://doi.org/10.5194/gmd-9-1937-2016)
- CMIP5/2100
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference [Taylor et al. (2012)](https://doi.org/10.1175/BAMS-D-11-00094.1)
- ISIMIP3b_CMIP6/2100
    - internally bias-corrected to W5E5 using bias correction period 1979-2014 [(Lange, 2019)](https://doi.org/10.5194/gmd-12-3055-2019) and interpolated to a 0.5° spatial resolution, we did not apply any additional bias-correction on top of that
    - five primary GCMs from phase 3b of the Inter-Sectoral Impact Model Intercomparison Project (ISIMIP3b) for three different SSPs
    - main references [Lange (2019](https://doi.org/10.5194/gmd-12-3055-2019), [2022)](https://doi.org/10.5281/zenodo.2549631)
    
*For CMIP5 and CMIP6, we have used  here the same bias correction period as in [Rounce et al., (2023)](https://doi.org/10.1126/science.abo1324), i.e. from Jan 2000 to Dec 2019. However,  using a bias correction period of 1979-2014 instead of 2000-2019 (tested on five GCMs) resulted in glacier volume differences that were globally up to 20% different until 2100 for a single GCM. (analysed by Lilian Schuster, [uncleaned notebook with the analysis](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/_uncleaned_analysis_notebooks/volume_evolution_common_running_glaciers.ipynb))*    


**Historical projections: 2000-2019** 

Inside of the RGI region folders, there are two options for how the perdiod 2000-2019 is being treated:
- `w5e5_gcm_merged`: Here, W5E5 was applied from 2000-2019 and the GCMs were applied from 2020 onwards until 2100. This is the option that we chose for the aggregated files.
    - In the period 2000-2019, the projections are the same for every GCM and scenario (as we use W5E5).
- `gcm_from_2000`: W5E5 is only used to calibrate the mass-balance model. For the actual projections, the individual GCMs (and scenarios) are used form 2000 onwards. 
    - In the period 2000-2019, different GCMs create different output. 
    
*The influence of the historical projection choice is invisible globally. Regionally it is very small after 2000-2019 (regionally < 2% after 2050). However, the historical projection choice changes the projections over the period 2000-2019 by up to 8% regionally estimates although the CMIP GCMs were bias-corrected to that time period. (analysed by Lilian Schuster, [uncleaned notebook with the analysis](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/_uncleaned_analysis_notebooks/volume_evolution_common_running_glaciers.ipynb))*


## Additional informations to rerun the projections

All scripts used to run the projections for https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/ are in the `_run_scripts` folders of the given oggm version. At the moment, we only have one version, thus the scripts are located here: https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/. They were run on the oggm bremen cluster, and parts of the scripts may be specific to that cluster. 

We have always run all projections on 1000 glaciers at once (instead of per RGI region), as this distributes the glaciers better to the different nodes. 

To run similar projections (for example with another bias correction period), you would need to adapt the paths and the files inside of that folder to your needs. Here is a short description of the different files:

- https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/0_create_all_gcm_list_2300_and_slurm_command_list.ipynb
    - just creates a list with those GCMs that actually go until 2299 or until 2300, by reading /home/www/oggm/cmip6/all_gcm_list.csv and /home/www/oggm/cmip6/all_gcm_table.html
        - saved under: https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/all_gcm_list_2300.csv
    - in addition creates a slurm commando list, which was then pasted into the terminal 

- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/run_with_hydro_diff_bc_methods_per_rgi_reg_2023.3.py
    - This is the main python script, where we select the glaciers, the variables and the preprocessed glacier directory (Level 5, prepro_border=160, prepro_base_url=https://cluster.klima.uni-bremen.de/~oggm/gdirs/oggm_v1.6/L3-L5_files/2023.3/elev_bands/W5E5_spinup), do the bias correction of the GCMs, run the projections, and merge them into aggregated files of up to 1000 glaciers.
- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/func_add.py
    - Here are two helper functions that are used in the main run_with_hydro script. They do the actual `run_with_hydro` task on all GCMs and SSPs for each glacier individually (-> like that multiprocessing can be applied).
- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/run_slurm_with_hydro_per_rgi_reg_2023.3.slurm
    - Here is the slurm script that was actually run from the terminal
    - to run it type in e.g.: `sbatch --array=1-1 run_slurm_with_hydro_per_rgi_reg_2023.3.slurm 06 2100 CMIP6`
        - this runs all simulations of RGI06 (Iceland) until 2300 using CMIP6 (i.e. here "pure" CMIP6 and ISIMIP3b_CMIP6), for both options `w5e5_gcm_merged` and `gcm_from_2000`
    - The amount of arrays corresponds to the amount of glaciers divided by 1000. In RGI11 with 3927 glaciers, we thus need array=1-4.
    - the complete list to run all simulations is in : https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/0_create_all_gcm_list_2300_and_slurm_command_list.ipynb
    
- https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/move_files_in_oggm_folder.ipynb
    - We changed the format and the location of the original oggm output slightly. The restructuring and renaming was done in this notebook. 
    - originally all output goes into the `output` folder here, but we moved it to https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/
    
- https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/compute_runoff_for_basins.ipynb
    - computes annual and monthly runoff for every basin and saves it in https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/**/basins
