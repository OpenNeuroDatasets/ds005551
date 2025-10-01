# Neural Mechanisms of Event Segmentation

This repository contains code and documentation for analyzing the neural mechanisms of event segmentation using fMRI data. The project investigates how the brain segments continuous experience into discrete events, comparing human annotations with computational models based on prediction error and uncertainty.

## Repository Structure

- **scripts/** - Contains analysis and visualization scripts, detailed in `scripts/README.md`
  - **combine_boundary_fmri/** - Scripts for processing and combining boundary data with fMRI measurements, detailed in `scripts/combine_boundary_fmri/README.md`
  - **fir_analyses.Rmd** - R script for Finite Impulse Response (FIR) analysis, detailed in `scripts/README.md`
  - Various Jupyter notebooks for visualization, detailed in `scripts/README.md`
- **data/** - Contains data files
  - **raw/** - Raw data files
    - **behavioral/** - Segmentation data files
    - **fmri/** - fMRI data files
    - **sem_output/** - Computational models' output files
  - **processed/** - Processed data files
    - **intermediate/** - Intermediate processed data files
    - ***.csv**: processed data files containing fmri and boundary data
- **results/** - Results from the analysis
  - **intermediate/** - Intermediate results from the analysis
  - ***.csv**: results from the FIR analysis
- **figures/** - Figures from the visualization
- **resources/** - Resources used in the analysis
  - Schaefer 400 volumetric and surface parcel atlases, Schaefer (cortex) and Tian (subcortical) brain parcellations, subject-specific movie onsets, afni polynomial drifts, etc.
- **assets/** - Assets (surface and volumetric images for each parcel) used in visualization

## Data Processing Pipeline

1. **Boundary Data Preparation**
   - Process human boundary annotations from a normative group
   - Generate computational boundaries using error-based and uncertainty-based models
   - Convert discrete boundaries to continuous density estimates
   - Identify significant peaks in boundary densities

2. **fMRI Data Processing**
   - Process fMRI data from fMRIPrep
   - Regress polynomial drifts and motion confounds
   - Calculate temporal pattern dissimilarity using Schaefer 400-parcel atlas
   - Merge boundary information with fMRI measurements

3. **Finite Impulse Response Analysis**
   - Examine temporal dynamics of BOLD activity and pattern shifts around event boundaries
   - Compare different boundary types (human-annotated, error-based, uncertainty-based)
   - Calculate z-statistics to identify significant neural responses

## Visualization Approaches

1. **Coefficient Line Plots** - Temporal dynamics across brain regions for both BOLD activity and pattern shifts
2. **Brain Surface Animations** - Visualize estimated BOLD activity and pattern shifts on brain surface
3. **Statistical Maps** - Z-statistics from FIR analyses for different boundary types (for BOLD activity and pattern shifts)

## Methods

Detailed methodology is available in the `methods.md` file, covering:
- Human and model segmentation approaches
- fMRI data processing techniques
- Pattern dissimilarity computation
- Finite impulse response analysis
- Visualization approaches

## References

- Bezdek et al. (2022)
- Nguyen et al. (2024)
- Schaefer et al. (2018)
