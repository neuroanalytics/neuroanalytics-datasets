# QSIPrep

QSIPrep configures pipelines for processing diffusion-weighted MRI
(dMRI) data. The main features of this software are:
- A BIDS-app approach to preprocessing nearly all kinds of modern
  diffusion MRI data.
- Automatically generated preprocessing pipelines that correctly
  group, distortion correct, motion correct, denoise, coregister and
  resample your scans, producing visual reports and QC metrics.
- A system for running state-of-the-art reconstruction pipelines that
  include algorithms from Dipy, MRTrix, DSI Studio and others.
- A novel motion correction algorithm that works on DSI and random
  q-space sampling schemes

The preprocessing pipelines are built based on the available BIDS
inputs, ensuring that fieldmaps are handled correctly. The
preprocessing workflow performs head motion correction, susceptibility
distortion correction, MP-PCA denoising, coregistration to T1w images,
spatial normalization using ANTs and tissue segmentation.

## Usage

QSIprep should be used through a container. A command like:

`qsiprep <input_dir>/ <output_dir>/ participant [OPTIONS]`

with `[OPTIONS]` specific to your dataset is easily invoked with
either Docker or Apptainer to use this tool.

You are also able to use `Nipoppy`!

## Output Structure

QSIPrep generates three broad classes of outcomes:

- **Visual QA (quality assessment) reports**: one HTML per subject,
  depicting images that provide a sanity check for each step of the
  pipeline.
- **Pre-processed imaging data** such as anatomical segmentations,
  realigned and resampled diffusion weighted images and the
  corresponding corrected gradient files in FSL and MRTrix format.
- **Additional data for subsequent analysis**, for instance the
  transformations between different spaces or the estimated head
  motion and model fit quality calculated during model-based head
  motion correction.
- **Quantitative QA**: A single-line csv file per subject summarizing
  subject motion, coregistration quality and image quality.

The file structure should resemble something like this:
```
sub-<label>/
  anat/
	sub-<label>_from-anat_to-ACPC_mode-image_xfm.mat
	sub-<label>_from-ACPC_to-anat_mode-image_xfm.mat
	sub-<label>_from-ACPC_to-<output-space>_mode-image_xfm.h5
	sub-<label>_from-<output-space>_to-ACPC_mode-image_xfm.h5
ses-<label>/
	anat/
		# Brain mask derived from SynthStrip
		<source_entities>_space-ACPC_desc-brain_mask.nii.gz

		# Tissue-probability maps
		<source_entities>_space-ACPC_label-CSF_probseg.nii.gz
		<source_entities>_space-ACPC_label-GM_probseg.nii.gz
		<source_entities>_space-ACPC_label-WM_probseg.nii.gz

		# Tissue class map derived SynthSeg
		<source_entities>_space-ACPC_dseg.nii.gz

		# Bias field corrected T1w file, using ANTS' N4BiasFieldCorrection
		<source_entities>_space-ACPC_desc-preproc_T1w.nii.gz

		# The same files as above, but in the selected output space.
		<source_entities>_space-MNI152NLin2009cAsym_desc-brain_mask.nii.gz
		<source_entities>_space-MNI152NLin2009cAsym_label-CSF_probseg.nii.gz
		<source_entities>_space-MNI152NLin2009cAsym_label-GM_probseg.nii.gz
		<source_entities>_space-MNI152NLin2009cAsym_label-WM_probseg.nii.gz
		<source_entities>_space-MNI152NLin2009cAsym_dseg.nii.gz
		<source_entities>_space-MNI152NLin2009cAsym_desc-preproc_T1w.nii.gz

		sub-<label>_ses-<label>_from-orig_to-anat_mode-image_xfm.txt
		sub-<label>_ses-<label>_from-anat_to-orig_mode-image_xfm.txt

	dwi/

		# A tab-separated value file with one column per calculated confound
		# and one row per timepoint/volume
		<source_entities>_desc-confounds_timeseries.tsv

		<source_entities>_space-ACPC_dwiref.nii.gz

		# The generous brain mask that should be reduced probably
		<source_entities>_space-ACPC_desc-brain_mask.nii.gz
		<source_entities>_space-ACPC_desc-preproc_dwi.nii.gz

		# FSL-style bval and bvec files.
		# These will be incorrectly interpreted by MRTrix,
		# but will work with DSI Studio and Dipy.
		<source_entities>_space-ACPC_desc-preproc_dwi.bval
		<source_entities>_space-ACPC_desc-preproc_dwi.bvec

		# Use the ``.b`` file for MRTrix.
		# The gradient table to import data into MRTrix.
		# This can be used with the preprocessed DWI file and
		# converted directly to a ``.mif`` file using the
		# ``mrconvert -grad _dwi.b`` command.
		<source_entities>_space-ACPC_desc-preproc_dwi.b

		# Contrast-to-noise model defined as the variance of the
		# signal model divided by the variance of the error of the signal model.
		<source_entities>_space-ACPC_stat-cnr_desc-<label>_dwimap.json
		<source_entities>_space-ACPC_stat-cnr_desc-<label>_dwimap.nii.gz
```

Naturally, different versions and analysis options can change what
files appear.

## References

Cieslak, M., Cook, P. A., He, X., Yeh, F. C., Dhollander, T.,
Adebimpe, A., ... & Satterthwaite, T. D. (2021). QSIPrep: an
integrative platform for preprocessing and reconstructing diffusion
MRI data. Nature methods, 18(7), 775-778.
