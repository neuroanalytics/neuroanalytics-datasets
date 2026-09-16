# Nilearn

Nilearn is the Python library that sits between a preprocessed dataset and an analysis. It
handles the operations that are conceptually trivial and tediously easy to get wrong:
masking, resampling to a common space, applying an atlas, extracting region timeseries,
building connectivity matrices, and plotting the result on a brain.

Its practical value is that it makes the standard path short. Turning
[fMRIPrep](fmriprep.md) output into a functional connectome — apply an atlas, extract
timeseries, regress confounds, correlate — is a handful of lines against a well-tested
implementation rather than a bespoke script whose indexing you will be re-checking in six
months. It reads BIDS derivatives directly and its confound-loading utilities understand
fMRIPrep's regressor files, which removes a common source of error.

It appears throughout this book without ever being introduced:
[giga_connectome](gigaconnectome.md) is built on it, and
[Hao-Ting Wang](../../cohort1/hao-ting_wang.md) is a core developer.

:::{note} This page is a stub
Needs: masker objects and why they are the central abstraction; choosing an atlas and
what that choice commits you to; confound regression with fMRIPrep outputs, including
which strategy to pick; the plotting API for figures worth publishing; where Nilearn's
own machine-learning helpers end and [scikit-learn](../../deep-learning.md) begins.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [fMRIPrep](fmriprep.md) — the usual upstream
- [giga_connectome](gigaconnectome.md) — built on Nilearn
- [fMRI](../../modalities/fmri.md)
- [Hao-Ting Wang](../../cohort1/hao-ting_wang.md) — Nilearn core developer
