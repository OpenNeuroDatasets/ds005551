# fMRI Data Directory

This directory contains the fMRI data in vairous formats. 

## Directory Structure

- `fmri_prep_csv/`: Contains the FMRIPREP parcel-average fMRI data in csv format.
- `fmri_prep_npz/`: Contains voxel-wise BOLD data in `*schaefer_subcortical.npz` files converted from FMRIPREP `*.nii` files for more efficient storage. Motion and drift-regressed data are stored in `*_drift_motion_regressed_afni_5polys.npz`.
- `xcp_24p_gsr_csv/`: Contains the parcel-average fMRI data from XCP-24p-GSR processing after FMRIPREP, in csv format. **The csv files also contain time relative to the start of the movie for each TR, which is used to align the fMRI data with the boundary annotations.**
- `stimulus_program_210509/`: Contains the stimuli and scripts for the scanning session.

## Data Conversion

The original `.nii` files from FMRIPREP have been converted to `*schaefer_subcortical.npz` files using a custom script `fmriprep_to_npz_parcel_to_array_subcortical.py` to reduce storage requirements while maintaining data integrity. The conversion process preserves all voxel values and spatial information.

## Loading NPZ Files

To load the converted `*.npz` files in Python, use the following code:

```python
import numpy as np

# load the npz files containing the time series data
subject = 'sub-01'
run = '3.1.3'
schaefer_ts_example = np.load(rf'data/raw/fmri/fmri_prep_npz/{subject}_{run}_schaefer_subcortical.npz')
schaefer_ts_example = {file: schaefer_ts_example[file] for file in schaefer_ts_example.files}
print(schaefer_ts_example.keys()) 
print(schaefer_ts_example['1.0'].shape)  # TR x Parcel
```
