# oggm-standard-projections-csv-files

**Repository of data files and documentation of the standard (CMIP) projections computed with OGGM**

Here you can find the glacier volume and area evolution, aggregated globally and for every RGI region separately. 

At the moment, there are only projections available using OGGM v1.6.1 (folder: [1.6.1/](1.6.1/])) with the preprocessed glacier directory version 2023.3. These projections include the [dynamical spinup](https://docs.oggm.org/en/latest/dynamic-spinup.html), the [new per-glacier geodetic calibration method](https://docs.oggm.org/en/latest/mass-balance-monthly.html), and use the W5E5v2.0 climate dataset [(Lange and others, 2021)](https://doi.org/10.48364/ISIMIP.342217) for calibration. 

|![Figure 1](notebooks/global_glacier_volume_until2100_common_running_2100.png)|
|:--:| 
| *Figure 1: Global glacier volume from 2000 to 2100 relative to 2020 (in %) for the different CMIP options using the common running glaciers until 2100.* |
|![Figure 2](notebooks/global_glacier_volume_until2300_common_running_2100_2300.png)|
|:--:| 
| *Figure 2: : Global glacier volume from 2000 to 2300 relative to 2020 (in %) only for CMIP6 by using the common running glaciers until 2100 and 2300. Note that the smaller GCM ensemble until 2300 is rather "hotter" than the more representative ensemble until 2100* |

Here is the code to create the figures above: [notebooks/analyse_csv_files_1.6.1.ipynb](notebooks/analyse_csv_files_1.6.1.ipynb). 

We computed and show in the figures all GCMs and scenarios that are currently available at the OGGM cluster. However, you have to choose those scenarios that are suitable and representative for your study. [For example, you could select them after the method of Hausfather et al., 2022)](https://www.nature.com/articles/d41586-022-01192-2) or [aggregate them after their 2100 warming levels (e.g. as in Rounce et al., 2023)](https://doi.org/10.1126/science.abo1324).

If you want `per-glacier` files, or other estimates than volume or area (e.g. the four components to compute glacier runoff, or the volume above sea-level), the non-aggregated raw oggm output files are here https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/. These files are further documented in [the README of that folder](https://cluster.klima.uni-bremen.de/~oggm/oggm-standard-projections/README), where it is also explained how to rerun the projections. The aggregation from the raw files to the csv files was done via [notebooks/aggregate_csv_files_1.6.1.ipynb](notebooks/aggregate_csv_files_1.6.1.ipynb). 

----
If you want to use the aggregated or the raw per-glacier data, please cite the dataset via:
- zenodo-link ...
In addition, cite OGGM [(Maussion et al., 2019)](https://doi.org/10.5194/gmd-12-909-2019) and the CMIP option that you are using (references are linked below).
---

## Some additional informations:

***Glacier aggregation option:*** 
We estimated those glaciers that do not fail for all scenarios, GCMs and CMIP variations. As more glaciers fail when running the simulations until 2300, we separated between 
- files with aggregated glaciers that run for all options until 2100 (`common_running_2100`)
    - use these files if you are only interested in glacier projections untils 2100
- files with aggregated glaciers that run for all options until 2100 and until 2300 (`common_running_2100_2300`). 
    - use these files if you are interested in glacier projections until 2300

For each of the two options, we give additional statistics:
- e.g. [1.6.1/common_running_2100/missing_region_overview.html](1.6.1/common_running_2100/missing_region_overview.html): missing glacier statistics
- e.g. [1.6.1/common_running_2100_2300/rgi_ids_missing.json](1.6.1/common_running_2100_2300/rgi_ids_missing.json): glacier IDs that have been removed as at least one GCM or scenario failed on it for one of the CMIP options
- e.g. [1.6.1/common_running_2100/metadata.csv](1.6.1/common_running_2100/metadata.csv):  can be helpful when doing analysis with the data (lists all the available files)

***Data units***
- volume is given in m3
- area is given in m2

***CMIP option and final year:***
At the moment there are three options here. For all options, W5E5 was applied from 2000-2019 and the GCMs were applied from 2020 onwards until 2100. So far, we only did projections until 2300 for CMIP6, but projections until 2300 for CMIP5 will be soon available. 

- CMIP6/2100 or CMIP6/2300
    - bias correction period 2000-2019
    - main reference: [Eyring et al. (2016)](https://doi.org/10.5194/gmd-9-1937-2016)
- CMIP5/2100
    - bias correction period 2000-2019
    - main reference [Taylor et al., (2012)](https://doi.org/10.1175/BAMS-D-11-00094.1)
- ISIMIP3b_CMIP6/2100
    - internally bias-corrected to W5E5 using bias correction period 1979-2014 [(Lange, 2019)](https://doi.org/10.5194/gmd-12-3055-2019) and interpolated to a 0.5° spatial resolution, we did not apply any additional bias-correction on top of that
    - we used the five primary GCMs from phase 3b of the Inter-Sectoral Impact Model Intercomparison Project (ISIMIP3b) for three different SSPs
    - main references ([Lange, 2019](https://doi.org/10.5194/gmd-12-3055-2019); [Lange, 2022](https://doi.org/10.5281/zenodo.2549631))
used for the historical period of 1979–2019, while the five
primary GCMs from phase 3b of the Inter-Sectoral Impact
All temperature-index model choices in this study are based
on the original temperature-index model of OGGM presented Model Intercomparison Project (ISIMIP3b, Lange, 2019, 2022)
in Maussion and others (2019), which itself builds upon Marzeion are used for the future period of 2020–2100.
    
(in the raw per-glacier data files, you can also find the same simulations using the GCMs from 2000 onwards, named `gcm_from_2000`)

The actual projections for the different scenarios are given in `{scenario}.csv` files in subfolders for every RGI region or globally. In every file, all GCM projections from one scenario (e.g. `ssp370.csv`) are given as different columns, where each of the rows shows one time point. 


***File structure**:
The file structure looks e.g. like that:
- [1.6.1/common_running_2100/volume/CMIP5/2100/RGI01/rcp85.csv]
- [1.6.1/common_running_2100_2300/volume/CMIP6/2300/global/ssp126.csv]

 
