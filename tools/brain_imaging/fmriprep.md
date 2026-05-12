# fMRIPrep

fMRIPrep is a NiPreps (NeuroImaging PREProcessing toolS) application
([www.nipreps.org(www.nipreps.org)]) for the preprocessing of
task-based and resting-state functional MRI (fMRI).

fMRIPrep is a functional magnetic resonance imaging (fMRI) data
preprocessing pipeline that is designed to provide an easily
accessible, state-of-the-art interface that is robust to variations in
scan acquisition protocols and that requires minimal user input, while
providing easily interpretable and comprehensive error and output
reporting. It performs basic processing steps (coregistration,
normalization, unwarping, noise component extraction, segmentation,
skull-stripping, etc.) providing outputs that can be easily submitted
to a variety of group level analyses, including task-based or
resting-state fMRI, graph theory measures, and surface or volume-based
statistics.

fMRIPrep is built around three principles:

- Robustness - The pipeline adapts the preprocessing steps depending
  on the input dataset and should provide results as good as possible
  independently of scanner make, scanning parameters or presence of
  additional correction scans (such as fieldmaps).
- Ease of use - Thanks to dependence on the BIDS standard, manual
  parameter input is reduced to a minimum, allowing the pipeline to
  run in an automatic fashion.
- “Glass box” philosophy - Automation should not mean that one should
  not visually inspect the results or understand the methods. Thus,
  fMRIPrep provides visual reports for each subject, detailing the
  accuracy of the most important processing steps. This, combined with
  the documentation, can help researchers to understand the process
  and decide which subjects should be kept for the group level
  analysis.

## Usage

fMRIPrep should be used through a container. A command like:

`fmriprep <input_dir>/ <output_dir>/ participant [OPTIONS]`

with `[OPTIONS]` specific to your dataset is easily invoked with
either Docker or Apptainer to use this tool.

You are also able to use `Nipoppy`!

## Output Structure

1. Visual QA (quality assessment) reports: one HTML per subject, that
   allows the user a thorough visual assessment of the quality of
   processing and ensures the transparency of fMRIPrep operation.
2. Derivatives (preprocessed data) the input fMRI data ready for
   analysis, i.e., after the various preparation procedures have been
   applied. For example, INU-corrected versions of the T1-weighted
   image (per subject), the brain mask, or BOLD images after
   head-motion correction, slice-timing correction and aligned into
   the same-subject’s T1w space or in some standard space.
3. Confounds: this is a special family of derivatives that can be
   utilized to inform subsequent denoising steps.

The `output` folder where the derivatives is stored will have this
structure:
```
<output_dir>/
  logs/
  sub-<label>/
  sub-<label>.html
  dataset_description.json
  .bidsignore
```

and within each subject, the following files (generally) will be
found. Specifics will vary by version and the options selected, but
generlly speaking the typical output will contain:

```
sub-<subject_label>/
  anat/
	sub-<subject_label>_from-MNI152NLin2009cAsym_to-T1w_mode-image_xfm.h5
	sub-<subject_label>_from-T1w_to-MNI152NLin2009cAsym_mode-image_xfm.h5
	sub-<subject_label>_from-fsnative_to-T1w_mode-image_xfm.txt
	sub-<subject_label>_from-T1w_to-fsnative_mode-image_xfm.txt
	sub-<subject_label>_hemi-[LR]_white.surf.gii
	sub-<subject_label>_hemi-[LR]_midthickness.surf.gii
	sub-<subject_label>_hemi-[LR]_pial.surf.gii
	sub-<subject_label>_hemi-[LR]_desc-reg_sphere.surf.gii
	sub-<subject_label>_hemi-[LR]_space-fsLR_desc-reg_sphere.surf.gii
	sub-<subject_label>_hemi-[LR]_space-fsLR_desc-msmsulc_sphere.surf.gii
	sub-<subject_label>_hemi-[LR]_desc-cortex_mask.label.gii
	sub-<subject_label>_hemi-[LR]_thickness.shape.gii
	sub-<subject_label>_hemi-[LR]_curv.shape.gii
	sub-<subject_label>_hemi-[LR]_sulc.shape.gii
	sub-<subject_label>_space-fsLR_den-32k_thickness.dscalar.nii
	sub-<subject_label>_space-fsLR_den-32k_curv.dscalar.nii
	sub-<subject_label>_space-fsLR_den-32k_sulc.dscalar.nii
  func/
	sub-<subject_label>_[specifiers]_space-<space_label>_desc-brain_mask.nii.gz
	sub-<subject_label>_[specifiers]_space-<space_label>_desc-preproc_bold.nii.gz
	sub-<subject_label>_[specifiers]_desc-hmc_boldref.nii.gz
	sub-<subject_label>_[specifiers]_from-orig_to_boldref_mode-image_desc-hmc_xfm.txt
	sub-<subject_label>_[specifiers]_desc-coreg_boldref.nii.gz
	sub-<subject_label>_[specifiers]_from-boldref_to-T1w_mode-image_desc-coreg_xfm.txt
	sub-<subject_label>_[specifiers]_from-boldref_to-TOPUP_mode-image_xfm.txt
	sub-<subject_label>_[specifiers]_from-boldref_to-auto000XX_mode-image_xfm.txt
```
Additionally, `FreeSurfer` derivatives will be written and stored
independly in way accessible to typical `FreeSurfer` analysis functions.

## References

Esteban, O., Markiewicz, C. J., Blair, R. W., Moodie, C. A., Isik,
A. I., Erramuzpe, A., ... & Gorgolewski, K. J. (2019). fMRIPrep: a
robust preprocessing pipeline for functional MRI. Nature methods,
16(1), 111-116.

Esteban, O., Ciric, R., Finc, K., Blair, R. W., Markiewicz, C. J.,
Moodie, C. A., ... & Gorgolewski, K. J. (2020). Analysis of task-based
functional MRI data preprocessed with fMRIPrep. Nature protocols,
15(7), 2186-2202.
