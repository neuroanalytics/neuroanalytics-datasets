# FreeSurfer

FreeSurfer reconstructs the cortical surface from a T1-weighted scan and derives the
structural measures most anatomical studies report: cortical thickness, surface area,
curvature, subcortical volumes, and a parcellation into named regions. Its single command,
`recon-all`, runs an hours-long sequence of skull-stripping, intensity normalization,
tissue segmentation, and surface tessellation.

Two practical points matter for anyone here. First, **you may already be running it**:
[fMRIPrep](fmriprep.md) calls FreeSurfer internally for surface reconstruction and
registration, so a fMRIPrep derivatives directory usually contains a full FreeSurfer
output tree you can use without re-running anything. Second, `recon-all` takes 6–12 hours
per subject, which makes it the step that dominates wall-clock time on a cohort and the
one that most needs [job array scheduling](../computer_science/hpc-jobs.md) rather than a
loop.

Its outputs are also the input to a great deal else — surface-based statistics, normative
models of cortical thickness, and most structural biomarkers in the aging and dementia
literature.

:::{note} This page is a stub
Needs: what `recon-all` actually does, stage by stage; reading the output directory; QC,
including the failure modes worth inspecting by eye and where automated QC helps; using
the FreeSurfer tree fMRIPrep already produced; longitudinal processing for repeated scans;
license setup, which trips people up on a cluster.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Structural MRI](../../modalities/smri.md)
- [fMRIPrep](fmriprep.md) — runs FreeSurfer for you
- [MRIQC](mriqc.md) — quality control upstream
- [HPC jobs](../computer_science/hpc-jobs.md) — how to run it across a cohort
