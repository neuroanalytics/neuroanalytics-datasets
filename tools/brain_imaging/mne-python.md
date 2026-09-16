# MNE-Python

MNE-Python is the standard open-source toolkit for EEG and MEG analysis: reading the
vendor formats, filtering, artefact removal, epoching, time-frequency decomposition,
source localization, and connectivity. If [Brainstorm](brainstorm.md) is the
GUI-first option, MNE is the scripted one — which makes it the better fit when the analysis
needs to be reproducible, batched over a cohort, or version-controlled.

Electrophysiology preprocessing differs from the MRI path documented elsewhere in this
book in an important way: there is no equivalent of [fMRIPrep](fmriprep.md). No widely
agreed pipeline takes raw EEG and returns analysis-ready data, because the right choices
depend heavily on the paradigm and the artefacts present. That places more decisions on
you — filter cutoffs, referencing, ICA components to reject, epoch rejection thresholds —
and makes recording those decisions unusually important.

MNE-BIDS connects it to the rest of the toolchain, reading and writing the EEG and MEG
flavours of [BIDS](../data_management/bids.md).

:::{note} This page is a stub
Needs: the `Raw` → `Epochs` → `Evoked` progression that structures the whole API; a
defensible default preprocessing sequence and where the judgement calls are; ICA for ocular
and cardiac artefacts; time-frequency analysis; source localization and what a head model
requires; MNE-BIDS; how to keep a manual cleaning step reproducible.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [EEG and MEG](../../modalities/eeg-meg.md)
- [Brainstorm](brainstorm.md) — the GUI-first alternative
- [BIDS](../data_management/bids.md) — via MNE-BIDS
- [Seizure disorders](../../research_topics/seizures.md) — where EEG concentrates here
