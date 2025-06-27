This directory contains the MTurk segmentation data in `mturk_segmentation_data/`, and memory data (recall and recognition) and fMRI segmentation data in `exp152_Session_2/` from the fMRI participants.

Memory data is in `exp152_Session_2/memory_df.csv` and fMRI segmentation data is in `exp152_Session_2/segmentation_df_08_15_2023.csv`. These dataframes are created by running `scripts/analyze_session_two.ipynb` to load and process raw recognition and recall and segmentation data from `exp152_Session_2/data/`. Note that the memory_df.csv and segmentation_df_08_15_2023.csv used PsychoPy subject indexing (e.g. e152003, e152004, etc.) while the fMRI data uses fmriPrep subject indexing (e.g. sub-01, sub-02, etc.). The script below was used to add the fmriPrep subject indexing to the memory_df.csv and segmentation_df_08_15_2023.csv, and output the updated files as `exp152_Session_2/memory_df_with_subject_mapping.csv` and `exp152_Session_2/segmentation_df_08_15_2023_with_subject_mapping.csv`.
```
import pandas as pd
import os

def add_subject_mapping():
    """
    Add subject mapping from e152003 format to sub-01 format in memory and segmentation CSV files.
    """
    
    # File paths
    counterbalance_file = r"C:\Users\nguye\Box\DCL_ARCHIVE\Documents\Events\exp152_fMRIneuralmechanisms\recruitment\forms&instructions\e152_counterbalance.xls"
    memory_file = r"C:\Users\nguye\Box\DCL_ARCHIVE\Documents\Events\exp152_fMRIneuralmechanisms\data\raw\behavioral\exp152_Session_2\memory_df.csv"
    segmentation_file = r"C:\Users\nguye\Box\DCL_ARCHIVE\Documents\Events\exp152_fMRIneuralmechanisms\data\raw\behavioral\exp152_Session_2\segmentation_df_08_15_2023.csv"
    
    # Load the counterbalance mapping
    print("Loading counterbalance mapping...")
    counter_balance = pd.read_excel(counterbalance_file, sheet_name='OA Participants')
    
    # Create mapping dictionary from VC##### to fmriPrep Number
    counter_balance_dict = counter_balance.set_index('VC#####').to_dict()['fmriPrep Number']
    
    print(f"Loaded {len(counter_balance_dict)} mappings")
    print("Sample mappings:")
    for i, (vc_id, fmri_id) in enumerate(counter_balance_dict.items()):
        if i < 5:  # Show first 5 mappings
            print(f"  {vc_id} -> {fmri_id}")
    
    # Function to process a CSV file
    def process_csv_file(file_path, output_suffix="_with_subject_mapping"):
        if not os.path.exists(file_path):
            print(f"Warning: File not found: {file_path}")
            return
        
        print(f"\nProcessing: {file_path}")
        
        # Read the CSV file
        df = pd.read_csv(file_path)
        print(f"Original data shape: {df.shape}")
        
        # Check if 'sub' column exists
        if 'sub' not in df.columns:
            print(f"Warning: 'sub' column not found in {file_path}")
            print(f"Available columns: {list(df.columns)}")
            return
        
        # Add the subject mapping column
        df['fmri_sub'] = df['sub'].map(counter_balance_dict)
        
        # Check for unmapped IDs
        unmapped = df[df['fmri_sub'].isna()]['sub'].unique()
        if len(unmapped) > 0:
            print(f"Warning: {len(unmapped)} unmapped IDs found: {unmapped}")
        
        # Count successful mappings
        mapped_count = df['fmri_sub'].notna().sum()
        print(f"Successfully mapped {mapped_count} out of {len(df)} rows")
        
        # Create output filename
        base_name = os.path.splitext(file_path)[0]
        output_file = f"{base_name}{output_suffix}.csv"
        
        # Save the updated file
        df.to_csv(output_file, index=False)
        print(f"Saved updated file: {output_file}")
        
        return df
    
    # Process both files
    print("\n" + "="*50)
    print("PROCESSING FILES")
    print("="*50)
    
    # Process memory file
    memory_df = process_csv_file(memory_file)
    
    # Process segmentation file
    segmentation_df = process_csv_file(segmentation_file)
    
    print("\n" + "="*50)
    print("SUMMARY")
    print("="*50)
    print("Script completed successfully!")
    print("Updated files have been saved with '_with_subject_mapping' suffix.")
    
    if memory_df is not None:
        print(f"Memory file: {len(memory_df)} rows processed")
    if segmentation_df is not None:
        print(f"Segmentation file: {len(segmentation_df)} rows processed")

if __name__ == "__main__":
    add_subject_mapping() 
```

MTurk segmentation data in `mturk_segmentation_data/` are aligned with the fMRI data, see `scripts/combine_boundary_fmri/README.md` for more details.