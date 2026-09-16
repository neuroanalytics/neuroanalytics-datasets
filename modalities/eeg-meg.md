# Electroencephalography (EEG) / Magnetoencephalography (MEG)

EEG and MEG measure electrophysiological activity directly, at millisecond resolution —
the opposite trade-off to fMRI. Six CNS projects use them, making electrophysiology the
second-largest methodological cluster in the program after deep learning.

## Data

- [PREVENT-AD](../data/databases/prevent-ad.md) includes MEG/EEG alongside its imaging
  and biomarker measures
- [DANDI](../data/portals/dandi.md) archives open neurophysiology data
- [OpenNeuro](../data/portals/open-neuro.md) hosts EEG and MEG datasets in
  [BIDS](../tools/data_management/bids.md) format

## Processing

- [Brainstorm](../tools/brain_imaging/brainstorm.md) — open-source MEG/EEG analysis
  environment, and the platform the AD Toolbox is being built on
- [MNE-Python](../tools/brain_imaging/mne-python.md) — the scripted alternative, better
  suited to batching over a cohort and version-controlling your choices
- [BIDS](../tools/data_management/bids.md) has EEG and MEG extensions, so the same
  organizational conventions apply

Unlike the MRI path, there is no fMRIPrep equivalent here — no agreed pipeline turns raw
EEG into analysis-ready data, so more of the decisions are yours to make and record.

## CNS Projects

- [Diellor Basha](../cohort1/diellor_basha.md) — electrophysiology of neurological
  disorders; building an AD Toolbox within Brainstorm
- [Phillip Johnston](../cohort2/phillip_johnston.md) — detecting invisible brain injury
  from repeated head impacts using generative neurophysiological modelling
- [Selena Singh](../cohort2/selena_singh.md) — EEG signatures of neural attractor states
  in major depression
- [Nooshin Bahador](../cohort1/nooshin_bahador.md) — chirp localization in EEG
  spectrograms
- [Tom George](../cohort2/tom_george.md) — EEG as one input modality to a
  neuro-foundation model
- [Emmanuelle Renauld](../cohort2/emmanuelle_renauld.md) — EEG alongside diffusion MRI
  and fMRI

## Related topics

[Seizure Disorders](../research_topics/seizures.md),
[Major Depressive Disorder](../research_topics/depression.md),
[Alzheimer's Disease](../research_topics/alzheimers.md)
