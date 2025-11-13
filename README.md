# Spatiotemporal Tracking of Persistent, Localized Speckles in Turbulent Atmospheric Propagation — Code

This repository contains code related to our research article titled  
**"Spatiotemporal Tracking of Persistent, Localized Speckles in Turbulent Atmospheric Propagation."**

**DOI:** _(to be added)_

---

## Need to know

1. This repository contains the code and data required to recreate the figures used in the paper.  
2. Download the Python notebook labeled **`Localization_Plotting.ipynb`**.  
3. Download the files within **`GitHub_Data/`**.  
4. Update directory paths in the notebook to point to your downloaded data (metrics and propagation files).

---

## Contents of files

### `Cn2_1e-13_Metrics_Data_final.h5`
**Used in:** Figure 1, Figure 2, Figure 3, Figure 4  

This file is the metrics data for a Gaussian beam propagated through turbulence  
(Cn2 = 1e-13, w0 = 0.01 m, l0 = 0.005 m).  
It contains only two per-iteration datasets: **Correlation** and **Mode-Field Radius (mfrarr)**.

- **Group naming pattern (one per iteration N):**  
  `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=<N>`

- **Datasets inside each iteration group:**
  - **Correlation (magnitude-squared coherence)**  
    Shape: `(40,)`  
    Data type: `float64`  
    Description: Axial correlation metric per interval (40 total intervals between 41 planes)
  - **mfrarr (Mode field radius)**  
    Shape: `(41,)`  
    Data type: `float64`

---

### `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412.h5`
**Used in:** Figure 1, Figure 2, Figure 4  

This HDF5 file contains a single simulated optical propagation realization for a Gaussian beam.

**Beam parameters:**
- Beam waist (w0): 0.01 meters  
- Turbulence strength (Cn2): 1e-13 m⁻²ᐟ³  
- Inner scale (l0): 0.005 meters  
- Propagation length (L): 3000 meters  

**Structure:**
- One main group:  
  `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412`
- Inside this group:
  - **Uout_real**  
    Description: Complex field values at each propagation step (real + imaginary encoded as `complex64`)  
    Shape: `(41, 1024, 1024)`  
    Data type: `complex64`
  - **phase_screens**  
    Description: Random phase screens applied along the propagation path  
    Shape: `(41, 1024, 1024)`  
    Data type: `float32`

---

### `InitialSpeckleWidth_Average_w0_0.01_Cn2_1e-13_l0_0.005.h5`
**Used in:** Figure 4  

This file contains the average speckle width statistics across 500 propagation realizations  
(w0 = 0.01 m, Cn2 = 1e-13 m⁻²ᐟ³, l0 = 0.005 m).

**Datasets:**
- **alive_counts**  
  Shape: `(41,)`  
  Data type: `int32`  
  Description: Number of active speckles detected at each propagation plane  
- **avg_width**  
  Shape: `(41,)`  
  Data type: `float64`  
  Description: Mean 1/e² speckle width averaged across all realizations, per propagation plane  
- **widths_per_iter**  
  Shape: `(500, 41)`  
  Data type: `float64`  
  Description: Individual 1/e² speckle widths for each of 500 iterations  
- **zlist_norm**  
  Shape: `(41,)`  
  Data type: `float64`  
  Description: Normalized axial coordinate array corresponding to the 41 sampled propagation planes  

Each column in the width arrays corresponds to a discrete normalized propagation distance.

---

### `Speckle_Width_TimeSeries_w0_0.01_Cn2_1e-13_l0_0.005.h5`
**Used in:** Figure 4  

This HDF5 file contains individual speckle widths and positions for multiple propagation realizations of a Gaussian beam under turbulence characterized by  
w0 = 0.01 m, Cn2 = 1e-13 m⁻²ᐟ³, and l0 = 0.005 m.

**Structure:**
- Each iteration group (`iteration_0000`, `iteration_0001`, …) corresponds to a single realization.  
- Within each iteration group are subgroups (`sXXXX`), each representing one detected speckle tracked across multiple axial planes.

**Each speckle subgroup contains:**
- **width_m**  
  Shape: `(N,)`, dtype `float32` — Speckle 1/e² radius in meters  
- **width_px**  
  Shape: `(N,)`, dtype `float32` — Speckle width in pixels  
- **x_px**  
  Shape: `(N,)`, dtype `float32` — X-coordinate of speckle centroid  
- **y_px**  
  Shape: `(N,)`, dtype `float32` — Y-coordinate of speckle centroid  
- **z**  
  Shape: `(N,)`, dtype `int32` — Axial frame indices  

**Notes:**
- `N` varies per speckle, representing its lifetime across frames.  
- Each iteration group contains multiple speckle subgroups (`s0000`, `s0001`, …).  
- Captures both spatial (x, y) motion and temporal (z) persistence of localized intensity peaks, used for computing lifetime distributions, confinement metrics, and width evolution statistics.

---

## Comments

- All data presented here can be derived from the field profiles in  
  `w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412.h5`.  
- 500 total iterations exist; iteration 412 was selected at random.  
- Additional data for other `Cn2`, `l0`, and `w0` values are available upon request but omitted due to size constraints.
