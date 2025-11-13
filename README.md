# Spatiotemporal-Tracking-of-Persistent-Localized-Speckles-in-Turbulent-Atmospheric-Propagation-Code
This repository contains code related to our research article titled "Spatiotemporal Tracking of Persistent, Localized Speckles in Turbulent Atmospheric Propagation". 

DOI: 

Need to know: 
1) This Repository contains code and data required to recreate figures used in the paper.
2) This requires the download of the Python notebook labeled as "Localization_Plotting.ipynb".
3) The files within "GitHub_Data" should be downloaded. 
4) The directories in the code should point to the files containing the metrics data files and propagation data files.

Contents of files: 
    =====
    FILE: Cn2_1e-13_Metrics_Data_final.h5 (Used in figure 1, figure 2, figure 3, and figure 4)
        This file is the metrics data for a Gaussian beam propagated through turbulence (Cn2 = 1e-13, w0 = 0.01 m, l0 = 0.005 m).
        It contains only two per-iteration datasets: Correlation and Mode-Field Radius (mfrarr).
    Group naming pattern (one per iteration N):
        w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=<N>
    Datasets inside each iteration group:
        -----Correlation (magnitude-squared coherence)-----
        Shape: (40,)
        Data type: float64
        Description: Axial correlation metric per interval (40 total intervals between 41 planes).
        -----mfrarr (Mode field radius)-----
        Shape: (41,)
        Data type: float64
    =====
    FILE: w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412.h5 (Used for figure 1 and figure 2, and figure 4)
        This HDF5 file contains a single simulated optical propagation realization for a Gaussian beam.
    Beam parameters:
        Beam waist (w0): 0.01 meters
        Turbulence strength (Cn2): 1e-13 m^(-2/3)
        Inner scale (l0): 0.005 meters
        Propagation length (L): 3000 meters
    Structure:
        The file contains one main group named: w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412
        Inside this group are two datasets:
        -----Uout_real-----
        Description: Complex field values at each propagation step.
        The real and imaginary parts are encoded in complex64 format.
        Shape: (41, 1024, 1024)
        Data type: complex64
        -----phase_screens-----
        Description: Random phase screens applied along the propagation path.
        Shape: (41, 1024, 1024)
        Data type: float32
    =====
    FILE: InitialSpeckleWidth_Average_w0_0.01_Cn2_1e-13_l0_0.005.h5 (Used in figure 4)
        This file contains the average speckle width statistics across 500 propagation realizations (w0 = 0.01 m, Cn2 = 1e-13 m^(-2/3), and l0 = 0.005 m).
        Datasets:
            -----alive_counts------
            Shape: (41,)
            Data type: int32
            Description: Number of active speckles detected at each propagation plane.
            -----avg_width-----
            Shape: (41,)
            Data type: float64
            Description: Mean 1/e² speckle width averaged across all realizations, per propagation plane.
            -----widths_per_iter-----
            Shape: (500, 41)
            Data type: float64
            Description: Individual 1/e² speckle widths for each of 500 iterations.
            -----zlist_norm-----
            Shape: (41,)
            Data type: float64
            Description: Normalized axial coordinate array corresponding to the 41 sampled propagation planes.
            Each column in the width arrays corresponds to a discrete normalized propagation distance.
    =====
    FILE: Speckle_Width_TimeSeries_w0_0.01_Cn2_1e-13_l0_0.005.h5 (Used in figure 4)
        This HDF5 file contains individual speckle widths and positions for multiple propagation realizations of a Gaussian beam under turbulence characterized by:
        w0 = 0.01 m, Cn2 = 1e-13 m^(-2/3), and l0 = 0.005 m.
        Structure:
            Each iteration group (e.g., iteration_0000, iteration_0001, …) corresponds to a single realization of the beam propagation.
            Within each iteration group, there are subgroups labeled sXXXX, each representing one detected speckle tracked across multiple axial planes.
            Each speckle subgroup contains the following datasets:
                -----width_m-----
                Shape: (N,)
                Data type: float32
                Description: Temporal evolution of the speckle’s 1/e² radius in meters at discrete propagation planes.
                -----width_px-----
                Shape: (N,)
                Data type: float32
                Description: Same as width_m but expressed in pixels for direct spatial indexing.
                -----x_px-----
                Shape: (N,)
                Data type: float32
                Description: X-coordinate of the speckle centroid (in pixels) at each tracked frame.
                -----y_px-----
                Shape: (N,)
                Data type: float32
                Description: Y-coordinate of the speckle centroid (in pixels) at each tracked frame.
                -----z-----
                Shape: (N,)
                Data type: int32
                Description: Frame indices corresponding to discrete axial propagation steps where the speckle was detected.
            Notes:
                • N varies per speckle, representing its lifetime across frames.
                • Each iteration group contains multiple speckle subgroups (s0000, s0001, …) corresponding to all tracked speckles in that realization.
                • The dataset captures both the spatial (x, y) motion and temporal (z) persistence of localized intensity peaks, used for computing lifetime distributions,                         confinement metrics, and width evolution statistics.

Comments: 
- All data calculated here can be obtained from the profiles in the file w0=0.01_Cn2=1e-13_l0=0.005_l=0_p=0_iteration=412.h5.
- We have data for 500 iterations and 412 was selected at random. The other 499 iterations, as well as other Cn2, l0, and w0 data could not be included due to the size of the dataset. This data can be made available upon request.
 
        
       
