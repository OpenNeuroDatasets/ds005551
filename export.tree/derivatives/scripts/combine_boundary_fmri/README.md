# Combining Boundary and fMRI Data

This folder contains scripts for processing and combining event boundary data with fMRI measurements.

## Data Processing Pipeline

1. Start with raw boundary annotations from human participants and computational models
2. Convert discrete boundaries to continuous density estimates
3. Identify significant peaks in boundary densities
4. Merge boundary information with fMRI measurements
5. Create a unified dataset for further analysis (Finite Impulse Response) of neural mechanisms of event segmentation

## Scripts Overview

Run notebooks in the order below. The final dataframe that combines boundary and fMRI data is saved as `data/processed/activity_pattern_tr_distance_4_window_3_afni_5polys_and_sem_pe_pu_density_ind_peaks_random_mindis7_human_aligned_nearby5.csv`. This file is used for the FIR analysis.

### 1. `event_boundary_to_density.ipynb`
Converts discrete event boundary annotations into continuous density estimates.
- Takes raw boundary annotations from human participants and models
- Applies Kernel Density Estimation (KDE) to create continuous boundary density distributions
- Smooths and resamples the density estimates for consistent temporal resolution
- Outputs CSV files with boundary density estimates for each model and movie

### 2. `boundary_density_to_peaks.ipynb`
Identifies peaks in boundary density distributions.
- Combines human and model boundary density estimates from the previous step
- Detects significant peaks in the boundary density distributions
- Creates visualizations comparing boundary densities across different models and movies
- Generates dictionaries of peak timestamps for each model and movie combination

### 3. `merge_fmri_and_boundary.ipynb`
Merges fMRI activity patterns with boundary information.
- Loads preprocessed fMRI data (both fMRIPrep and XCP) and regresses polynomial drifts and motion confounds from BOLD timeseries
- Computes pattern shifts from fMRIPrep voxelwise data for each parcel (Schaefer 400)
- Combines parcelwise BOLD and pattern shift data with boundary density estimates to create a master data frame for analyzing neural correlates of event boundaries across multiple subjects and movie runs

### 4. `add_peaks_to_master_df.ipynb`
Incorporates boundary peak information into the master data frame.
- Adds human and model boundary peaks to the master data frame
- Creates indicator variables for peaks from different models (uncertainty-SEM, pe-SEM)
- Exports the master data frame with boundary peak indicators


## Movies Used

The scripts process data from four movies with the following identifiers:
- 1.2.3
- 2.4.1
- 3.1.3
- 6.3.9
