# Spatiotemporal Tracking of Optical Speckles in Turbulent Atmospheric Propagation — Code and Data

This repository contains code and selected reduced data products used to recreate figures from the research article  
**"Spatiotemporal Tracking of Optical Speckles in Turbulent Atmospheric Propagation."**

**DOI:** _(to be added)_

---

## Need to know

1. This repository contains the code and reduced data files needed to reproduce the figures included in the paper.
2. Download the notebook **`Figure_Recreation_Notebook.ipynb`**.
3. Download the files within **`GitHub_Data/`**.
4. Update directory paths in the notebook so they point to your local copies of the downloaded files.
5. The repository contains reduced figure-generation data products, not the full simulation archive.

---

## Repository contents

The repository currently contains five categories of files:

1. Reduced **mode-field radius (MFR)** HDF5 files  
2. Full **time-series speckle tracking** HDF5 files  
3. Reduced **eta-sweep summary** CSV files  
4. One full **single-realization propagation** HDF5 file  
5. One notebook for **figure recreation and example visualizations**

---

## 1. Reduced MFR files

**Used in:** figures that plot ensemble mode-field radius (MFR) curves.

These files contain reduced metrics data for all saved propagation realizations at a fixed turbulence strength. Each iteration group contains only the dataset `mfrarr`, which is the mode-field radius evaluated at the 41 saved propagation planes.

### Files in this structure
- `Cn2_1e-13_Metrics_Data.h5`
- `Cn2_1e-14_Metrics_Data.h5`
- `Cn2_1e-15_Metrics_Data.h5`

### General structure
- **Top-level groups:** one group per realization / beam configuration
- **Group naming pattern:**  
  `w0=<w0>_Cn2=<Cn2>_l0=0.005_l=0_p=0_iteration=<N>`

- **Dataset inside each iteration group:**
  - **`mfrarr`**  
    Description: mode-field radius at each saved propagation plane  
    Shape: `(41,)`  
    Data type: floating-point array

### Notes
- These are reduced versions of the larger metrics files and only retain the dataset needed to regenerate the MFR curves.
- The propagation axis consists of 41 saved planes over a total propagation distance of 3000 m.

---

## 2. Time-series speckle tracking files

**Used in:** figures based on tracked speckle width evolution, pooled width statistics, and tracked-speckle convergence analysis.

These HDF5 files contain the object-level tracking output for detected speckles at a selected detection threshold. Each file corresponds to one `(w0, Cn2, l0, eta)` parameter combination.

### Files in this structure
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.01_Cn2_1e-13_l0_0.005.h5`
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.01_Cn2_1e-14_l0_0.005.h5`
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.01_Cn2_1e-15_l0_0.005.h5`
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.02_Cn2_1e-13_l0_0.005.h5`
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.02_Cn2_1e-14_l0_0.005.h5`
- `Speckle_Width_TimeSeries_eta_3e-02_w0_0.02_Cn2_1e-15_l0_0.005.h5`

### General structure
- **Root-level contents:**
  - `metadata`
  - `iteration_0000`, `iteration_0001`, `iteration_0002`, ...

- **Metadata group:**  
  Stores run-level metadata such as:
  - `w0_m`
  - `Cn2`
  - `l0_m`
  - `L_m`
  - `wavelength_m`
  - `dim`
  - `dx_m`
  - `dy_m`
  - `eta`
  - `min_track_len_frames`
  - `assoc_r_px`
  - dataset `z_range`

- **Iteration groups:**  
  Each group corresponds to one propagation realization:
  - `iteration_0000`
  - `iteration_0001`
  - ...

  Each iteration group contains one subgroup per tracked speckle:
  - `s0000`
  - `s0001`
  - `s0002`
  - ...

- **Each speckle subgroup contains:**
  - **`z`**  
    Shape: `(N,)`  
    Data type: `int32`  
    Description: axial frame indices for that tracked speckle

  - **`x_px`**  
    Shape: `(N,)`  
    Data type: `float32`  
    Description: x-coordinate of the tracked speckle centroid in pixels

  - **`y_px`**  
    Shape: `(N,)`  
    Data type: `float32`  
    Description: y-coordinate of the tracked speckle centroid in pixels

  - **`width_px`**  
    Shape: `(N,)`  
    Data type: `float32`  
    Description: 1/e² speckle radius in pixels

  - **`width_m`**  
    Shape: `(N,)`  
    Data type: `float32`  
    Description: 1/e² speckle radius in meters

- **Speckle subgroup attributes include:**
  - `creation_z`
  - `death_z`
  - `right_censored`
  - `initial_x_px`
  - `initial_y_px`
  - `initial_intensity_raw`
  - `initial_width_px`
  - `min_running_mean_radius_px`
  - `running_mean_radius_px_final`

### Notes
- `N` varies from speckle to speckle because each tracked object persists for a different number of planes.
- These files store full tracked trajectories and widths, not just ensemble averages.
- The filename tag `eta_3e-02` corresponds to the saved threshold labeling used in this repository.

---

## 3. Eta-sweep summary CSV files

**Used in:** the figure showing the mean tracked speckle count as a function of detection threshold `eta`.

These CSV files are reduced summary products generated from the full tracking runs. Each file corresponds to one `(w0, Cn2, l0)` combination and contains the ensemble summary statistics across the eta sweep.

### Files in this structure
- `eta_sweep_summary_w0_0.01_Cn2_1e-13_l0_0.005.csv`
- `eta_sweep_summary_w0_0.01_Cn2_1e-14_l0_0.005.csv`
- `eta_sweep_summary_w0_0.01_Cn2_1e-15_l0_0.005.csv`
- `eta_sweep_summary_w0_0.02_Cn2_1e-13_l0_0.005.csv`
- `eta_sweep_summary_w0_0.02_Cn2_1e-14_l0_0.005.csv`
- `eta_sweep_summary_w0_0.02_Cn2_1e-15_l0_0.005.csv`

### General structure
Each CSV contains the columns:

- **`eta`**  
  Detection threshold used for the tracking run

- **`n_success`**  
  Number of successful realizations included in the summary

- **`mean_tracked_count`**  
  Mean number of tracked speckles across realizations

- **`std`**  
  Standard deviation of tracked speckle count

- **`min`**  
  Minimum tracked speckle count across realizations

- **`max`**  
  Maximum tracked speckle count across realizations

### Notes
- These files are reduced products intended specifically for the eta-sweep summary plot.
- They are sufficient for recreating the threshold-scaling figure without requiring the full intermediate archive for every eta value.

---

## 4. Single-realization propagation file

### `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412.h5`

**Used in:** example propagation-field visualizations, normalized intensity-plane examples, and tracking demonstrations.

This file contains a single simulated optical propagation realization for a Gaussian beam and is included as an example realization rather than as an ensemble summary product.

**Beam parameters:**
- Beam waist: `w0 = 0.01 m`
- Turbulence strength: `Cn2 = 1e-13 m^-2/3`
- Inner scale: `l0 = 0.005 m`
- Propagation length: `L = 3000 m`

### Structure
- One main group:  
  `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412`

- **Datasets inside that group:**
  - **`Uout_real`**  
    Description: complex optical field at each saved propagation plane  
    Shape: `(41, 1024, 1024)`  
    Data type: `complex64`

  - **`phase_screens`**  
    Description: applied turbulent phase screens  
    Shape: `(41, 1024, 1024)`  
    Data type: `float32`

### Notes
- This is one realization selected from a larger ensemble.
- It contains the full propagation field and phase-screen data, unlike the reduced summary products above.

---

## 5. Figure recreation notebook

### `Figure_Recreation_Notebook_with_tracking_example_zoomed.ipynb`

This notebook is the main figure-recreation entry point for the repository.

It includes code to:
- recreate pooled speckle-width versus MFR figures,
- recreate eta-sweep summary figures,
- recreate tracked-speckle convergence figures,
- display normalized intensity profiles from a single propagation realization,
- display tracked-structure overlays on selected intensity planes.

The notebook is intended to be run after downloading the files in `GitHub_Data/` and updating local directory paths.

---

## Comments

- The repository contains only the files needed to recreate the figures associated with the paper.
- Reduced files are used where possible to avoid uploading the full simulation archive.
- The included HDF5 files preserve the internal structure needed by the plotting scripts and notebook.
- The notebook provides a direct entry point for reproducing the paper figures from the reduced data products in `GitHub_Data/`.
- Additional data for other thresholds, beam parameters, or turbulence strengths are omitted due to repository size constraints.
