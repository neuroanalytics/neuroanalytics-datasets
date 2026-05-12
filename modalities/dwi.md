# Diffusion Weighted Imaging (DWI)

## Data

The Diffusion Magnetic Resonance Imaging (dMRI) data, which describes
the movement of water within the brain, is a specialized 4D
T2*-contrasted magnetic resonance sequence. This type of imaging is
critical because models fitted to the dMRI data can summarize the
orientation of water movement within the tissue, reflecting aspects
like water moving parallel to (along) axons.

When collecting dMRI data, the sequence iterates along **orientation**
rather than incrementing time along the 4th dimension.

### Structure of dMRI Data and Associated Files

A dMRI scan consists of two kinds of volumes collected in the 4D
sequence:

1.  **Unweighted volumes ($\text{b}0$)**: These are images collected
	with no diffusion weighting applied.
2.  **Weighted gradient volumes**: These volumes have diffusion
	gradients applied, which makes them sensitive to the movement of
	water molecules along specific directions.

To correctly interpret the weighted gradient volumes, additional
information describing the diffusion gradient applied during the
acquisition must be stored separately. These supplementary files
typically contain information about the magnitude and orientation of
the gradients used.

Specifically, the required information is stored in two accompanying
files, often adhering to conventions such as those used by analysis
software like FSL:

*   **Magnitude of the gradient applied ($\text{bvals}$ file)**: This
	file contains the magnitude of the diffusion weighting applied for
	each volume in the 4D scan. The magnitude ($\text{b}$-value)
	determines the strength of the diffusion weighting, and multiple
	different magnitudes (shells) can be collected in a modern
	sequence (e.g., multi-shell diffusion imaging).
*   **Orientation of the gradient applied ($\text{bvecs}$ file)**:
	This file contains the orientation vector of the diffusion
	gradient applied for each volume. The orientation of this file
	relative to the image data file is critical for proper use of the
	scan.

The ability to fit increasingly complex models to the data depends on
the number of shells ($\text{bvals}$) and directions ($\text{bvecs}$)
collected in the sequence. These models represent properties of the
underlying tissue by summarizing the difference in orientation from
the baseline ($\text{b}0$) image.

### Diffusion MRI in the PREVENT-AD Cohort

In the PREVENT-AD study, Diffusion Weighted Imaging (DWI) data were
collected across both Phase 1 (Stage 1) and Phase 2 (Stage 2).

**Stage 1/Phase 1 Acquisition (2011–2017):**
*   The DWI sequence used in Phase 1 was a 2D EPI transversal
	sequence.
*   The acquisition parameters involved $\text{b}$-values of $ \text{
	s}/\text{mm}^2$ with 1 ($\text{b}0$ image) and 64 directions. This
	indicates a single-shell protocol (excluding the $\text{b}0$
	acquisition).

**Stage 2/Phase 2 Transition (Post-2017):**
*   When the MRI protocol was updated in Phase 2 (resuming 2019), the
	diffusion MRI acquisition changed **from single shell to
	multi-shell**.
*   The final raw data acquired are available in **NIfTI file
	format**.

**Analytic Measures (Data Derivatives):** The PREVENT-AD repository
also shares analytical measures derived from the dMRI data,
specifically focusing on Diffusion Tensor Imaging (DTI) measures:

*   **Diffusion properties of white matter tracts** are quantified
	using pipelines such as TractoFlow, RBXFlow, and Tractometry Flow.
*   Examples of DTI measures shared include:
	*   **Fractional Anisotropy (FA)**
	*   **Mean Diffusivity (MD)**
	*   **Axial Diffusivity (AD)**
	*   **Radial Diffusivity (RD)**

Furthermore, advanced analyses leveraging multi-shell data, such as
**Neurite Orientation Dispersion and Density Imaging (NODDI)**, can
yield microstructure estimates like Neurite Density Index (NDI),
Orientation Dispersion Index (ODI), and Isotropic Volume Fraction
(ISOVF). These measures can be mapped to connectome edges using
tractography pipelines.

## Processing

The processing of Diffusion Weighted Imaging (DWI) data, which is
essential for deriving metrics about water movement and microstructure
within the white matter, involves several common and necessary steps,
primarily focused on artifact correction, normalization, and
modeling. These steps are often nearly identical across different
standardized pipelines (such as TractoFlow, QSIPrep, or MRTrix3
Preprocessing).

Here are the common steps necessary for processing DWI data, followed
by modeling and analysis steps:

### I. Preprocessing and Quality Control

The initial stages focus on cleaning and correcting the raw 4D
T2*-contrasted sequence data for noise and artifacts.

1.  **Check Diffusion Gradient Orientation:** It is crucial to check
	the orientation of the gradient vectors ($\text{bvecs}$) in
	conjunction with the diffusion data itself. The raw DWI data
	consists of unweighted volumes ($\text{b}0$) and weighted gradient
	volumes, and the associated files ($\text{bvals}$ and
	$\text{bvecs}$) containing the magnitude and orientation of the
	diffusion gradients are necessary for using the scan.

2.  **Denoising (Remove Scanner Noise):** This step involves removing
	unwanted scanner noise from the data. For example, the MRTrix3
	pipeline includes a specific denoising command (`dwidenoise`)
	using MP-Distribution.

3.  **Gibbs Ringing Artifact Removal:** Gibbs ringing artifacts, which
	appear as characteristic ripples or ghosts in the image, must be
	removed.

4.  **Top-up, Eddy Current, and Motion Correction:** This step
	corrects for artifacts caused by susceptibility distortion, eddy
	currents (induced by rapidly switching magnetic gradients), and
	bulk head motion that occur during acquisition. This process often
	requires the input of two diffusion volumes with opposing
	phase-encode directions (e.g., AP/PA or RL/LR acquisitions, often
	referred to as "top-up").

5.  **Bias Correction:** The background bias field, which causes
	non-uniform intensity across the image, needs to be normalized or
	corrected.

6.  **Rician Denoising:** This step filters Rician noise, often using
	commands like `mrcalc`.

7.  **Intensity Normalization (Optional):** Image intensity can be
	optionally normalized.

8.  **Brain Extraction and Masking:** The brain tissue must be
	isolated from non-brain tissue (like skull and skin) to create a
	clean mask for subsequent processing.

9.  **Registration and Reslicing:** The preprocessed DWI data must be
	registered (aligned) to a standard anatomical space, such as AC-PC
	space. The data may also be optionally resliced to a new
	resolution.

### II. Modeling and Derivation

Once preprocessed, the cleaned DWI data is used to fit models that
summarize water movement and tissue properties.

1.  **Estimate the Diffusion Tensor Model:** The diffusion tensor
	model (DTI) is estimated by summarizing the differences in
	orientation from the baseline unweighted volume
	($\text{b}0$). This model can be complex depending on the number
	of shells ($\text{bvals}$) and directions ($\text{bvecs}$)
	collected. This step yields measures like **Fractional Anisotropy
	(FA)**, **Mean Diffusivity (MD)**, **Axial Diffusivity (AD)**, and
	**Radial Diffusivity (RD)**.

2.  **Estimate Advanced Microstructure Models:** For multi-shell data
	(like that collected in PREVENT-AD Phase 2), more complex
	microstructure models, such as **Neurite Orientation Dispersion
	and Density Imaging (NODDI)**, can be estimated. NODDI provides
	estimates like Neurite Density Index (NDI), Orientation Dispersion
	Index (ODI), and Isotropic Volume Fraction (ISOVF).

3.  **Estimate Fiber Orientation Distribution (fODF) using CSD:**
	Advanced analysis often involves estimating the Fiber Orientation
	Distribution (fODF) using methods like **Constrained Spherical
	Deconvolution (CSD)**, which estimates the response function of
	the tissue to derive directional information.

4.  **Tractography and Structural Connectome Estimation:**
	Tractography algorithms use the orientation summaries (like the
	diffusion tensor or CSD fODFs) to follow a likely path of
	connectivity through the brain, reconstructing white matter
	bundles or streamlines. This can involve anatomically constrained
	techniques, such as **Reproducible Anatomically Constrained
	Ensemble Tractography (RACE-Trac)**, which can involve steps like
	creating five tissue-type partial volume maps and estimating CSD
	models.

5.  **Validation and Mapping (Optional/Advanced):**
	*   **Linear Fascicle Evaluation (LiFE):** This technique uses the
		tractography estimates to predict the diffusion signal and
		assigns weights to the streamlines based on how well they
		match the underlying raw diffusion data.
	*   **Mapping Properties to Edges:** Once streamlines (tracts) are
		defined, other properties derived from the DWI data, such as
		FA, MD, or NODDI metrics (NDI, ODI, ISOVF), can be mapped onto
		these structural network edges to quantify microstructural
		integrity along specific white matter bundles.
