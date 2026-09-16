# Functional Magnetic Resonance Imaging

fMRI measures the BOLD signal — a haemodynamic proxy for neural activity — with good
spatial coverage and poor temporal resolution. Most CNS projects use it to derive
**functional connectomes**: region-by-region matrices of how strongly brain areas
co-activate over time.

## Data

Datasets in this book that include fMRI:

- [PREVENT-AD](../data/databases/prevent-ad.md) — longitudinal, pre-symptomatic AD cohort
- [ADNI](../data/databases/adni.md) — Alzheimer's Disease Neuroimaging Initiative
- [UK Biobank](../data/databases/ukbb.md) — large population cohort
- [ABCD](../data/databases/abcd.md) — adolescent development

Portals that index open fMRI data: [OpenNeuro](../data/portals/open-neuro.md),
[CONP](../data/portals/conp-data-portal.md),
[BrainCODE](../data/portals/brain-code.md)

## Processing

The conventional path, in order:

1. Organize the raw data as [BIDS](../tools/data_management/bids.md)
2. Check quality with [MRIQC](../tools/brain_imaging/mriqc.md)
3. Preprocess with [fMRIPrep](../tools/brain_imaging/fmriprep.md)
4. Extract connectomes with
   [giga_connectome](../tools/brain_imaging/gigaconnectome.md)

[Nipoppy](../tools/brain_imaging/nipoppy.md) is useful for managing these pipelines
across a whole dataset rather than one subject at a time.
[Nilearn](../tools/brain_imaging/nilearn.md) is the usual next stop once preprocessing is
done — masking, atlases, timeseries extraction, and plotting.

## CNS Projects

- [Davor Curic](../cohort1/davor_curic.md) — geometry-aware distances between functional
  connectomes
- [Josh Neudorf](../cohort1/josh_neudorf.md) — how structural connectivity constrains
  functional connectivity
- [Mohamed Abdelhack](../cohort1/mohamed_abdelhack.md) — modelling cognition and
  psychiatric conditions from fMRI
- [Hao-Ting Wang](../cohort1/hao-ting_wang.md) — connectivity-based biomarker discovery
- [Julia-Katharina Pfarr](../cohort1/julia-katharina_pfarr.md) — functional connectivity
  as a normative measure
- [Tom George](../cohort2/tom_george.md) — self-supervised pretraining across open
  neural datasets
