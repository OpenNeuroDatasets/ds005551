# FIR Analysis Visualization Scripts

This directory contains:
- 4 Jupyter notebooks to combine boundary and fMRI data,
- An R script to perform FIR analysis on the boundary fMRI data,
- 3 Jupyter notebooks to visualize results. 

The boundary fMRI data frame is created by running 4 Jupyter notebooks in `scripts/combine_boundary_fmri/`, and the data frame is saved as `C:\Users\nguye\Box\DCL_ARCHIVE\Documents\Events\exp152_fMRIneuralmechanisms\data\processed\activity_pattern_tr_distance_4_window_3_afni_5polys_and_sem_pe_pu_density_ind_peaks_random_mindis7_human_aligned_nearby5.csv`

The Rmd file `scripts/fir_analyses.Rmd` is used to perform the FIR analysis.

3 Jupyter notebooks are used for visualizing Finite Impulse Response (FIR) model results from neuroimaging analyses.

## Dataframe preparation

Instructions for preparing the dataframe to use for FIR analysis are in `scripts/combine_boundary_fmri/README.md`

## FIR Analysis

The Rmd file `scripts/fir_analyses.Rmd` is used to perform the FIR analysis. Features include:
- Compare standard errors of the Linear model (ignore subject random effects on coefficients) vs. pseudo-lmer model (account for subject random effects on coefficients).
- Fit a linear model for all subjects at once, this model is used to calculate the f-statistic for each boundary type and brain parcel (dependent variable). f-statistic is converted to z-statistic using the appropriate degrees of freedom (#lags, #subjects).
- Fit one linear model per subject, then estimate the standard error of the coefficients across subjects, then combine with the within-subject variance to get the total variance. This is used to plot the coefficient lines with correct standard errors.

## Visualization

### fir_coef_lines.ipynb
This notebook generates line plots of FIR coefficients across different brain networks. It allows visualization of temporal dynamics by plotting coefficient values over time lags. Features include:
- Parcel-wise coefficient plots
- Compare coefficient lines between error-evoked and uncertainty-evoked pattern fluctuations. 

### fir_brain_animation.ipynb
This notebook visualizes the temporal dynamics of the FIR coefficients (for both BOLD and PATTERN) on the brain surface. It includes:
- Static surface plots at each time lag.
- Animations of brain surface BOLD/PATTERN fluctuations across time lags.

### fir_lrr_to_intercept_fstat.ipynb
This notebook visualizes z-statistics from FIR analyses (compare to intercept-only model) of both BOLD and PATTERN. Features include:
- Plotting z-statistic maps for BOLD and PATTERN for each boundary type (human fine grain, human coarse grain, error-based, uncertainty-based).
- Plotting z-statistic for both error-based and uncertainty-based on the same brain surface (both BOLD and PATTERN).

### Usage
Each notebook loads FIR model results and provides various visualization options. They require access to the model output files and brain parcellation data. The notebooks are designed to work with results from analyses using the Schaefer 400-parcel atlas.
