# MRIQC

MRIQC extracts no-reference IQMs (image quality metrics) from
structural (T1w and T2w), functional and diffusion MRI (magnetic
resonance imaging) data.

MRIQC is an open-source project, developed under the following
software engineering principles:

-**Modularity and integrability**: MRIQC implements a nipype workflow
to integrate modular sub-workflows that rely upon third party software
toolboxes such as ANTs and AFNI.  -**Minimal preprocessing**: the
MRIQC workflows should be as minimal as possible to estimate the IQMs
on the original data or their minimally processed derivatives.
-**Interoperability and standards**: MRIQC follows the the brain
imaging data structure (BIDS), and it adopts the BIDS-App standard.
-**Reliability and robustness**: the software undergoes frequent
vetting sprints by testing its robustness against data variability
(acquisition parameters, physiological differences, etc.) using images
from OpenfMRI. Its reliability is permanently checked and maintained
with CircleCI.

## Usage

MRIQC is most reliably envoked from a container. A command like:

`mriqc <input_dir>/ <output_dir>/ participant [OPTIONS]`

with `[OPTIONS]` specific to your dataset is easily invoked with
either Docker or Apptainer to use this tool.

You are also able to use `Nipoppy`!

## Outcomes

### Structural IQMs

#### Measures based on noise measurements:

- **cjv** – coefficient of joint variation (CJV): The cjv of GM and WM
  was proposed as objective function by [Ganzetti2016] for the
  optimization of INU correction algorithms. Higher values are related
  to the presence of heavy head motion and large INU artifacts. Lower
  values are better.
- **cnr** – contrast-to-noise ratio (CNR): The cnr [Magnota2006], is
  an extension of the SNR calculation to evaluate how separated the
  tissue distributions of GM and WM are. Higher values indicate better
  quality.
- **snr** – signal-to-noise ratio (SNR): calculated within the tissue
  mask.
- **snr_dietrich**: Dietrich’s SNR (SNRd) as proposed by
  [Dietrich2007], using the air background as reference.
- **art_qi2**: Mortamet’s quality index 2 (QI2) is a calculation of
  the goodness-of-fit of a distribution on the air mask, once the
  artifactual intensities detected for computing the QI1 index have
  been removed [Mortamet2009]. Lower values are better.

#### Measures based on information theory

- **efc**: The EFC [Atkinson1997] uses the Shannon entropy of voxel
  intensities as an indication of ghosting and blurring induced by
  head motion. Lower values are better.  The original equation is
  normalized by the maximum entropy, so that the EFC can be compared
  across images with different dimensions.
- **fber**: The FBER [Shehzad2015], defined as the mean energy of
  image values within the head relative to outside the head
  [QAP-measures]. Higher values are better.

#### Measures targeting specific artifacts

- **inu_\***"": (nipype interface to N4ITK): summary statistics (max,
  min and median) of the INU field (bias field) as extracted by the
  N4ITK algorithm [Tustison2010]. Values closer to 1.0 are better,
  values further from zero indicate greater RF field inhomogeneity.

- **art_qi1()**: Detect artifacts in the image using the method
  described in [Mortamet2009]. The QI1 is the proportion of voxels
  with intensity corrupted by artifacts normalized by the number of
  voxels in the background. Lower values are better.

#### The workflow to compute the artifact detection from [Mortamet2009].

-- **wm2max**: The white-matter to maximum intensity ratio is the
median intensity within the WM mask over the 95% percentile of the
full intensity distribution, that captures the existence of long tails
due to hyper-intensity of the carotid vessels and fat. Values should
be around the interval [0.6, 0.8].

#### Other measures

- **fwhm** (nipype interface to AFNI): The FWHM of the spatial
  distribution of the image intensity values in units of voxels
  [Forman1995]. Lower values are better, higher values indicate a
  blurrier image. Uses the gaussian width estimator filter implemented
  in AFNI’s 3dFWHMx:
- **volume_fraction** (icvs_*): the ICV fractions of CSF, GM and
  WM. They should move within a normative range.
- **rpve** (rpve_*): the rPVe of CSF, GM and WM. Lower values are
  better.
- **summary_stats** (summary_*_*): Mean, median, median absolute
	deviation (mad), standard deviation, kurtosis, 5% percentile, 95%
	percentile and number of voxels of the distribution of background
	(bg), foreground (fg: corresponds to the voxels within the brain
	mask), CSF, GM and WM.
- **overlap_\*_\***: The overlap of the TPMs estimated from the image
  and the corresponding maps from the ICBM nonlinear-asymmetric 2009c
  template. Higher values are better.

### Functional IQMs

#### Measures for the spatial information

Definitions are given in the summary of structural IQMs.

- Entropy-focus criterion (efc.
- Foreground-Background energy ratio (fber, [Shehzad2015]).
- Full-width half maximum smoothness (fwhm_*, see [Friedman2008]).
- Signal-to-noise ratio (snr.
- Summary statistics (summary_stats.

#### Measures for the temporal information

- **DVARS**: D referring to temporal derivative of timecourses, VARS
  referring to RMS variance over voxels ([Power2012] dvars_nstd)
  indexes the rate of change of BOLD signal across the entire brain at
  each frame of data. DVARS is calculated with nipype after motion
  correction:

MRIQC calculates two additional standardized values of the DVARS. The
dvars_std metric is normalized with the standard deviation of the
temporal difference time series. The dvars_vstd is a voxel-wise
standardization of DVARS, where the temporal difference time series is
normalized across time by that voxel standard deviation across time,
before computing the RMS of the temporal difference [Nichols2013].

- **Global Correlation (gcor)**: calculates an optimized summary of
  time-series correlation as in [Saad2013] using AFNI’s \@compute_gcor,
  where is the average of all unit-variance time series in a (#
  timepoints), (# voxels) matrix.

- **Temporal SNR (tSNR, tsnr)**: is a simplified interpretation of the
  tSNR definition [Kruger2001]. We report the median value of the tSNR
  map calculated like where is the average BOLD signal (across time)
  is divided by the corresponding temporal standard-deviation
  map. Higher values are better.

#### Measures for artifacts and other

- **Framewise Displacement**: expresses instantaneous head-motion
  [Jenkinson2002]. MRIQC reports the average FD, labeled as
  fd_mean. Rotational displacements are calculated as the displacement
  on the surface of a sphere of radius 50 mm [Power2012].

Along with the base framewise displacement, MRIQC reports the number
of timepoints above FD threshold (fd_num), and the percent of FDs
above the FD threshold w.r.t. the full timeseries (fd_perc). In both
cases, the threshold is set at 0.20mm.

- **Ghost to Signal Ratio (gsr)**: is labeled in the reports as gsr_x
  and gsr_y (calculated along the two possible phase-encoding axes x,
  y):


- **AFNI’s outlier ratio (aor)**: Mean fraction of outliers per fMRI
  volume as given by AFNI’s 3dToutcount.

- **AFNI’s quality index (aqi)**: Mean quality index as computed by
  AFNI’s 3dTqual; for each volume, it is one minus the Spearman’s
  (rank) correlation of that volume with the median volume. Lower
  values are better.

- **Number of dummy scans** (dummy)**: A number of volumes in the
  beginning of the fMRI timeseries identified as non-steady state.

### Diffusion IQMs

#### IQMs relating to spatial information

Definitions are given in the summary of structural IQMs.

- Entropy-focus criterion (efc):
- Foreground-Background energy ratio (fber(), [Shehzad2015]).
- Full-width half maximum smoothness (fwhm_*, see [Friedman2008]).
- Signal-to-noise ratio (snr()).
- Summary statistics (summary_stats()).

#### IQMs relating to diffusion weighting

- **Noise in raw dMRI estimated with PIESNO (piesno_sigma)**: Employs
  PIESNO (Probabilistic Identification and Estimation of Noise)
  algorithm [Koay2009] to estimate the standard deviation (sigma) of
  the noise in each voxel of a 4D dMRI data array.

- **SNR estimated in the Corpus Callosum (cc_snr)**: Worst-case and
  best-case signal-to-noise ratio (SNR) within the corpus
  callosum. Additionally, provies:
  - Number of not-a-number (NaN) values in the FA map (fa_nan)
  - Fraction of NaNs within the brain mask, in ppm.
  - Number of degenerate modeled voxels in the FA map (fa_degenerate)
  - Fraction of invalid FA values (i.e., outside the [0, 1] closed
	range) within the brain mask, in ppm.

IQMs targeting artifacts that are specific of DWI images.

- **Global and slice-wise spike fractions (spikes_ppm)**: Fractions of
  voxels classified as spikes (in parts-per-million, ppm). The spikes
  mask is calculated by identifying voxels with signal intensities
  exceeding a threshold based on standard deviations above the mean.

## Citation

Esteban O, Birman D, Schaer M, Koyejo OO, Poldrack RA, Gorgolewski KJ;
MRIQC: Advancing the Automatic Prediction of Image Quality in MRI from
Unseen Sites; PLOS ONE 12(9):e0184661;
doi:10.1371/journal.pone.0184661.
