# oggm-standard-projections 

OGGM provides what we like to call standard projections. Currently we make these future CMIP forced global glacier simulations available in two different formats, raw and aggregated data. In this document the raw data is described. 

**If you are only interested in regional volume or area changes (globally or per RGI region), we recommend you to use the aggregated data [README.md](README.md).** 

 ----
When you use the aggregated or the raw per-glacier data, please cite the dataset via:

**OGGM 1.6.3**: *Lilian Schuster, Patrick Schmitt, Anouk Vlug, & Fabien Maussion. (2026). OGGM/oggm-standard-projections-csv-files: (v1.1). Zenodo. https://doi.org/10.5281/zenodo.8286066*

**OGGM 1.6.1**: *Lilian Schuster, Patrick Schmitt, Anouk Vlug, & Fabien Maussion. (2023). OGGM/oggm-standard-projections-csv-files: v1.0 (v1.0). Zenodo. https://doi.org/10.5281/zenodo.8286065*

In addition, cite OGGM [(Maussion et al., 2019)](https://doi.org/10.5194/gmd-12-909-2019) and the CMIP option that you are using (references are linked below).

---

## Extended documentation of the raw oggm-output files and documentation of the standard (CMIP) projections computed with OGGM

***Model set-up***

At the moment, there are projections available using 
- OGGM v1.6.3 with the [preprocessed glacier directory version 2025.6](https://cluster.klima.uni-bremen.de/~oggm/gdirs/oggm_v1.6/L3-L5_files/2025.6/elev_bands/W5E5/per_glacier_spinup/), which you can find here:
[https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/](https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/),
- OGGM v1.6.1 with the [preprocessed glacier directory version 2023.3](https://cluster.klima.uni-bremen.de/~oggm/gdirs/oggm_v1.6/L3-L5_files/2023.3/elev_bands/W5E5_spinup/), which you can find here:
[https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/](https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/).

These projections use [elevation-band flowlines](https://docs.oggm.org/en/stable/flowlines.html#elevation-bands-flowlines), include the [dynamical spinup](https://docs.oggm.org/en/latest/dynamic-spinup.html), the [informed 3-step per-glacier geodetic calibration method](https://docs.oggm.org/en/latest/mass-balance-monthly.html), and use the W5E5v2.0 climate dataset [(Lange and others, 2021)](https://doi.org/10.48364/ISIMIP.342217) for calibration and a border of 160. 

We computed all GCMs and scenarios that are currently available at the OGGM cluster. However, you have to choose those scenarios that are suitable and representative for your study. [For example, you could select them after the method of Hausfather et al. (2022)](https://www.nature.com/articles/d41586-022-01192-2) or [aggregate them after their 2100 warming levels (e.g. as in Rounce et al., 2023)](https://doi.org/10.1126/science.abo1324).

***Options within one version***
- For OGGM v1.6.1, we provided just one option which uses W5E5, per-glacier calibrated dynamical spinup and RGI version 6.2.
- For OGGM v1.6.3, the standard option is the same as in OGGM v1.6.1, but we updated the file structure system by specifying the baseline climate and initialisation/calibration option (i.e. [1.6.3/w5e5/per_glacier_spinup](1.6.3/w5e5/per_glacier_spinup)). We also provide on the OGGM cluster (not as csv-files) additional test projection options for other preprocessed 2025.6 glacier directories under just one GCM (CMIP6 MRI-ESM2-0). 
    - ERA5 instead of W5E5 (in https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/era5/per_glacier_spinup/CMIP6/2100/)
    - regionally calibrated dynamical spinup instead of per-glacier spinup (https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/regional_spinup/CMIP6/2100/) for RGI version 6.2 and version 7.0G and 7.0C


***Data structure***

In the subfolders, we give the netCDF files for different CMIP and final year options for all of the 19 RGI regions. That means, an example filepath looks as follows:

- for OGGM v1.6.3: https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/CMIP6/2300/RGI06/run_hydro_w5e5_gcm_merged_ACCESS-CM2_ssp126_bc_2000_2019_endyr2301_Batch_0_1000.nc 
- for OGGM v1.6.1: https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2300/RGI06/run_hydro_w5e5_gcm_merged_ACCESS-CM2_ssp126_bc_2000_2019_Batch_0_1000.nc 
(the different options are explained in detail below).

Each netCDF file contains the projections from up to 1000 glaciers for a single GCM, scenario, RGI region and historical projection option from 2000 until 2100, 2101, 2299 or 2300 (for the ISIMIP3b GCM IITM-ESM and SSP370, projection only go until 2099). The timestep always corresponds to the glacier state at the beginning of the year, i.e., 2000 means 01-01-2000. We have run the `run_with_hydro` task of OGGM on a monthly basis, and thus, the following variables are included:

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
path = '/home/www/oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/CMIP6/2300/'
with xr.open_mfdataset(path + 'RGI11/run_hydro_w5e5_gcm_merged_CanESM5_ssp126_*.nc') as ds:
    ds['runoff'] = ds['melt_off_glacier']+ds['melt_on_glacier']+ ds['liq_prcp_off_glacier'] +ds['liq_prcp_on_glacier']
    ds = ds['runoff'].load()
```

Note, that some glaciers will have NaN values, so if you aggregate it, you need to either only use the common running glaciers (the method used to create the aggregated csv files) or do some filling. 
The annual glacier runoff that was computed here is the sum of annual melt and liquid precipitation on and off the glacier using a fixed-gauge with a glacier minimum reference area from year 2000 (unit: kg year-1).
- We also have already postprocessed per-glacier annual and monthly runoff files aggregated for every basin (notebook for [2023.3](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/compute_runoff_for_basins.ipynb) and [2025.6](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/compute_runoff_for_basins.ipynb), at the moment only for CMIP6 until 2100). The basin files are available at https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/CMIP6/2100/basins/ and https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/CMIP6/2100/basins/. All glaciers of a basin are aggregated in one file, and there are different files for different simulations. The files of each basin are in one subfolder, with the subfolder name being the MRBID of that basin. The basin files contain the variables: volume, area, runoff and runoff_monthly. The runoff components can not be computed for the last year, thus the last year has NaN values. 


**-> More details on how to handle the data (for example to estimate glacier runoff) is given in [this jupyter notebook](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/analysis_notebooks/workflow_to_analyse_per_glacier_projection_files.ipynb?flush_cache=true)** 


***CMIP option and final year:***
At the moment there are three categories of forcing data that have been used for the simulations (https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/ or https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/w5e5/per_glacier_spinup/). For all options, we bias-corrected the GCMs and then projected glacier changes until 2100 or 2300.
The sample of GCMs going until 2300 is much smaller than the one going until 2100. ISIMIP3b_CMIP6 only goes until 2100, thus projections only go until 2100. We did the projections until the end of the GCMs, i.e. until end of 2100 or 2300 (or until end of 2099 or 2299). In the OGGM v161 (2023.3) version projections, we only ran until beginning of 2100/2300, but for OGGM v163 (2025.6), we also ran the last year, i.e. give the glacier state at year 2101 or 2301. This is prescribed as e.g. `endyr_2100` or `endyr_2101` in the 2025.6 filepaths. For GCMs having data until 2300, we did two separate runs, once until 2100 and once until 2300, because there are glaciers that fail before 2300, but not until 2100 (specifically in Iceland, RGI region 6, see the overview table in [notebooks/1.6.3/missing_glacier_area_stats.png](notebooks/1.6.3/missing_glacier_area_stats.png)). 

- CMIP6/2100 or CMIP6/2300
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference: [Eyring et al. (2016)](https://doi.org/10.5194/gmd-9-1937-2016)
- CMIP5/2100 or CMIP5/2300
    - bias correction period 2000-2019 (file path named: `bc_2000_2019`)
    - main reference [Taylor et al. (2012)](https://doi.org/10.1175/BAMS-D-11-00094.1)
- ISIMIP3b_CMIP6/2100
    - internally bias-corrected to W5E5 using bias correction period 1979-2014 [(Lange, 2019)](https://doi.org/10.5194/gmd-12-3055-2019) and interpolated to a 0.5° spatial resolution, we did not apply any additional bias-correction on top of that
    - five primary GCMs from phase 3b of the Inter-Sectoral Impact Model Intercomparison Project (ISIMIP3b) for three different SSPs
    - only from OGGM v1.6.3 upwards: in addition nine other (secondary) GCMs and additional SSPs. Attention, the IITM-ESM only goes until end of 2098. Thus, the volume projections of IITM-ESM only go until 2099.
    - main references [Lange (2019](https://doi.org/10.5194/gmd-12-3055-2019), [2022)](https://doi.org/10.5281/zenodo.2549631)
    
*For CMIP5 and CMIP6, we have used  here the same bias correction period as in [Rounce et al. (2023)](https://doi.org/10.1126/science.abo1324), i.e. from Jan 2000 to Dec 2019. However, using a bias correction period of 1979-2014 instead of 2000-2019 (tested on five GCMs) resulted in glacier volume differences that were globally up to 20% different until 2100 for a single GCM. (analysed by Lilian Schuster with OGGM version 1.6.1/2023.3, [uncleaned notebook with the analysis](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/_uncleaned_analysis_notebooks/volume_evolution_common_running_glaciers.ipynb))*    


**Historical projections: 2000-2019** 

Inside of the RGI region folders, there are two options for how the period 2000-2019 is being treated:
- `w5e5_gcm_merged`: Here, W5E5 was applied from 2000-2019 and the GCMs were applied from 2020 onwards until 2100. This is the option that we chose for the aggregated files.
    - In the period 2000-2019, the projections are the same for every GCM and scenario (as we use W5E5).
- `gcm_from_2000` (only available in `1.6.1/2023.3`): W5E5 is only used to calibrate the mass-balance model. For the actual projections, the individual GCMs (and scenarios) are used form 2000 onwards. 
    - In the period 2000-2019, different GCMs create different output. 
    
*The influence of the historical projection choice is invisible globally. Regionally it is very small after 2000-2019 (regionally < 2% after 2050). However, the historical projection choice changes the projections over the period 2000-2019 by up to 8% regionally estimates although the CMIP GCMs were bias-corrected to that time period. (analysed by Lilian Schuster  with OGGM version 1.6.1/2023.3, [uncleaned notebook with the analysis](https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/_uncleaned_analysis_notebooks/volume_evolution_common_running_glaciers.ipynb))*


## Additional informations to rerun the projections

(here described for 2025.6, but very similar scripts were used for [2023.3](1.6.1/README_extended_per_glacier_files_only_1.6.1.md))

All scripts used to run the projections for https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16 are in the `_run_scripts` folders of the given oggm version (e.g. at https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/). They were run on the oggm bremen cluster, and parts of the scripts may be specific to that cluster. 

We have always run all projections on 1000 glaciers at once (instead of per RGI region), as this distributes the glaciers better to the different nodes. 

To run similar projections (for example with another bias correction period), you would need to adapt the paths and the files inside of that folder to your needs. Here is a short description of the different files:

- https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/0_create_slurm_command_list.ipynb
    - creates a slurm commando list which runs the slurm file (next item) with different options. This list has to be pasted into the terminal  
- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/run_slurm_with_hydro_per_rgi_reg_2025.6.slurm
    - Here is the slurm script that was actually run from the terminal
    - to run it type in e.g.: `sbatch --array=1-1 run_slurm_with_hydro_per_rgi_reg_2025.6.slurm 06 2101 CMIP6 w5e5 per_glacier_spinup 62`
        - this runs all simulations of RGI06 (Iceland) until 2101 (or until 2100 for those GCMs ending earlier) using CMIP6, W5E5, per-glacier spinup and RGI version 62
    - The amount of arrays corresponds to the amount of glaciers divided by 1000. In RGI11 with 3927 glaciers, we thus need array=1-4.
    - there are also different options (e.g. using ERA5 instead of E5E5, regional spinup instead of per-glacier spinup, or RGI version 70G, 70C instead of RGI version 62)
          - e.g. `sbatch --array=1-1 run_slurm_with_hydro_per_rgi_reg_2025.6.slurm 06 2101 CMIP6 w5e5 regional_spinup 70G` does the same as the example above but uses the [regional spinup RGI 70G glacier directory](https://cluster.klima.uni-bremen.de/~oggm/gdirs/oggm_v1.6/L3-L5_files/2025.6/elev_bands/W5E5/regional_spinup/RGI70G/b_160/L5/)
- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/run_with_hydro_diff_bc_methods_per_rgi_reg_2025.6.py
    - This is the main python script that is loaded by the slurm script. It selects the glaciers, the variables and the preprocessed glacier directory (Level 5, prepro_border=160, prepro_base_url=https://cluster.klima.uni-bremen.de/~oggm/gdirs/oggm_v1.6/L3-L5_files/2025.6/elev_bands/W5E5/per_glacier_spinup), do the bias correction of the GCMs, run the projections, and merge them into aggregated files of up to 1000 glaciers. If you are only interested in testing the workflow for a single GCM, do `one_gcm=True`.
- https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/func_add.py
    - Here are two helper functions that are used in the main `run_with_hydro_diff_*.py` python script. They do the actual `run_with_hydro` task on all GCMs and SSPs for each glacier individually (-> like that multiprocessing can be applied).

- only for oggm_v16/2023.3 (not anymore necessary for oggm_v16/2025.6): https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/_run_scripts/move_files_in_oggm_folder.ipynb
    - We changed the format and the location of the original oggm output slightly. The restructuring and renaming was done in this notebook. 
    - originally all output goes into the `output` folder here, but we moved it to https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2023.3/
    
- https://nbviewer.org/urls/cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/_run_scripts/compute_runoff_for_basins.ipynb
    - computes annual and monthly runoff for every basin for CMIP6 until 2100 and saves it in https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/oggm_v16/2025.6/CMIP6/2100/basins/

