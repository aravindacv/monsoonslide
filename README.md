# MonsoonSlide

Code accompanying the manuscript:

> **Gridded Rainfall Acts as a Location Fingerprint That Inflates Landslide Susceptibility Models**
> Aravinda C. V., Anand Rajendran, J. Emerson Raja

This repository contains the complete, end-to-end pipeline used in the study. It covers data acquisition and harmonisation, terrain, hydrological, land-cover and rainfall feature engineering for Kerala (Western Ghats, India), spatially blocked cross-validation, and the CPU model benchmark that exposes how gridded rainfall features act as a location fingerprint.

## Repository contents

```
notebooks/
  monsoonslide_pipeline.ipynb   Full pipeline (Stage 1a → Stage 3), with outputs
requirements.txt                Python dependencies
CITATION.cff                    Citation metadata
LICENSE                         MIT licence (code)
```

## Pipeline overview

The notebook runs in this order, and its cells are labelled by stage:

| Stage | What it does |
|---|---|
| **1a: Catalogues and benchmark imagery** | Merges and de-duplicates the NASA Global Landslide Catalog exports. Takes the Western Ghats subset. Checks the structure, band statistics and class imbalance of Landslide4Sense, and verifies its official validation and test splits from Hugging Face. |
| **1b: Kerala 2018 study data** | Builds the Kerala 2018 landslide inventory (DANS). Prepares the Copernicus GLO-30 DEM reprojected to UTM 43N at 30 m. Derives terrain features (slope, aspect, curvature and others), pre-event 2017 land cover (Esri / Impact Observatory via Planetary Computer), and hydrological features (TWI, stream power, distance to streams). Downloads CHIRPS v2.0 daily rainfall for 2006–2024. |
| **2: Feature table and validation design** | Samples 51,746 landslide and non-landslide points and extracts all features. Runs the CHIRPS vs IMD rainfall diagnostic and builds bias-corrected rainfall features with held-out-cell validation. Defines leave-one-district-out folds, 30 km spatial blocks, and folds aligned to the rainfall grid. Writes a data dictionary. |
| **3: Benchmark** | Runs LR, RF, XGBoost, LightGBM and CatBoost on three feature sets: terrain + hydrology, + land cover, and + rainfall. Each is evaluated under five validation schemes and two protocols. This is the core experiment showing how much rainfall-grid identity inflates scores. |

## Data sources

All input data are public. The notebook downloads them, or reads them as attached Kaggle datasets.

| Data | Source |
|---|---|
| NASA Global Landslide Catalog | Kaggle: `nasa/landslide-events`, `pratul007/global-landslide-catalog-export` |
| Landslide4Sense | Kaggle `tekbahadurkshetri/landslide4sense`; official splits from Hugging Face `ibm-nasa-geospatial/Landslide4sense` |
| Kerala 2018 landslide inventory | DANS Data Station, doi:10.17026/dans-x6c-y7x2 |
| Copernicus DEM GLO-30 | `copernicus-dem-30m` AWS open data |
| Land cover 2017 | Esri / Impact Observatory 10 m LULC via Microsoft Planetary Computer |
| CHIRPS v2.0 daily rainfall | Climate Hazards Center, UCSB |
| IMD gridded rainfall (0.25°) | India Meteorological Department, via `imdlib` |
| Administrative boundaries | geoBoundaries (IND ADM1/ADM2) |

## How to reproduce

The pipeline was developed and run on **Kaggle**, using the standard Python 3 CPU image.

1. Create a Kaggle notebook and import `notebooks/monsoonslide_pipeline.ipynb`.
2. Attach the Kaggle datasets listed above, and enable Internet access in the notebook settings.
3. Run the cells in order. Stages write their outputs to `/kaggle/working/stage1`, `stage2` and `stage3`.
4. Stage 2 and Stage 3 can be resumed from a saved checkpoint dataset, as described in the notebook's "Cell 1 (run in a new notebook…)" section.

To run locally instead:

```bash
pip install -r requirements.txt
```

You will also need to change the `/kaggle/input` and `/kaggle/working` paths at the top of each stage.

The random seed is fixed (`SEED = 42`) throughout.

## Citation

If you use this code, please cite the paper. See `CITATION.cff`; the full reference will be added on publication.

## Licence

Code: MIT. Third-party datasets remain under their original licences.

## Contact

Aravinda C. V. (GitHub: [@aravindacv](https://github.com/aravindacv))
