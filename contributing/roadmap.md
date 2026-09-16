# Coverage roadmap

This page tracks known gaps in the book — topics that belong here and are not yet written.
It exists so that contributors can see where the holes are without having to audit
80-odd pages, and so that the reasoning behind each gap is recorded rather than
rediscovered.

## How the gaps were identified

The book was compared against a full research-lifecycle model — a nine-stage pipeline
(propose, initialize, curate, analyze, checkpoint, QC, export, publish, disseminate)
running inside a continuous administrative track covering funding, ethics, deadlines and
credit.

The comparison produced one clear finding: **the book covers the middle of the lifecycle
well and both ends barely at all.**

```
Lifecycle:  Govern → Initialize → Curate → Analyze → Checkpoint → QC → Export → Publish → Disseminate
Coverage:   ░░░░░░   ████████     ██████   ███████   ░░░░░░░░░░   ███  ░░░░░░   ░░░░░░░   ░░░░░░░░░░
            └─ thin ─┘                                            └────────── thin ──────────┘
            └──────────── administrative track: thin throughout ────────────┘
```

The [Start Here pathway](../pathway/overview.md) reflects the same shape: it begins at
"find data" and ends at "share your work", which skips what must be settled *before* data
access and stops short of what turns an analysis into a citable research product.

Documentation of tools is genuinely strong. Documentation of **process** is where the gaps
are.

### A second axis: analysis and craft

The audit above asked whether every *stage of the lifecycle* was covered. A later pass
asked a different question — whether the **analytical and craft topics** a scholar needs
are present regardless of stage — and found a second set of holes, recorded as priorities
8–12 below.

Two findings stand out. The Research Methods section is **entirely machine learning**:
there was no classical statistical inference anywhere in a book about analysing clinical
neuroimaging cohorts. And **cross-cohort pooling is a stated premise of several CNS
projects** while site harmonization, the thing that makes pooling valid, had no coverage at
all.

Note that `research_topics/` and `modalities/` pages are deliberately short — they are
link-dense hubs, not content pages. Their length is not a gap.

## Priority 1 — Provenance and data versioning

The largest single hole, now partly filled.

| Topic | Status |
|---|---|
| [DataLad](../tools/data_management/datalad.md) | **Written** |
| YODA project layout | Covered inside the DataLad page |
| git-annex / large-file versioning | Covered inside the DataLad page |

Before this, DataLad appeared in four pages purely as a name in a list, despite being the
mechanism most of the reproducibility advice elsewhere in the book depends on.

## Priority 2 — Before you touch data

Nothing in the book covered this stage. A new scholar's first real blocker is usually data
access, and the book documented *which* datasets exist without ever describing how to get
into one.

| Topic | Status |
|---|---|
| [Ethics and data access](../governance/ethics-and-data-access.md) | Stub — highest value here |
| [Data management plans](../governance/data-management-plans.md) | Stub |
| [Pre-registration](../governance/pre-registration.md) | Stub |

## Priority 3 — Turning an analysis into a research product

The book stops at "share your work". The steps that make work citable and reusable are
absent.

| Topic | Status |
|---|---|
| [Releasing data and code](../publishing/data-and-code-release.md) | Stub — OSF, Zenodo, DOIs |
| [Reporting guidelines](../publishing/reporting-guidelines.md) | Stub — COBIDAS, TRIPOD |
| [Executable articles](../publishing/executable-articles.md) | Stub — NeuroLibre |

## Priority 4 — Curation depth

BIDS is explained as a standard, but not as a task you have to perform.

| Topic | Status |
|---|---|
| [Getting data into BIDS](../tools/data_management/raw-to-bids.md) | Stub — dcm2niix, HeuDiConv, validator |
| [Neurobagel and semantic annotation](../tools/data_management/neurobagel.md) | Stub — SNOMED, harmonization |

Cross-cohort pooling is a premise of several CNS projects, and variable harmonization is
the unglamorous step it depends on.

## Priority 5 — Administration and credit

| Topic | Status |
|---|---|
| [Credit and contributions](../governance/credit-and-contributions.md) | Stub — ORCID, CRediT |
| [Research administration](../governance/research-administration.md) | Stub — decision logs, obligations |

## Priority 6 — AI in the research workflow

The existing [AI models](../ai_models/llm-overview.md) section covers how large language
models work. It does not cover working *with* them, which is now part of most people's
daily practice and has unresolved implications for provenance.

| Topic | Status |
|---|---|
| [AI assistants in the research workflow](../ai_models/ai-assisted-research.md) | Stub |
| [Agent-callable methods](../ai_models/agent-callable-methods.md) | Stub — MCP, Paper2Agent |

## Priority 7 — A quality framework

| Topic | Status |
|---|---|
| [STAMPED principles](../governance/stamped.md) | Stub |

The book already argues for most of STAMPED without naming it — containers for
portability, provenance for tracking, BIDS for modularity. Naming the framework would let
those arguments reinforce each other instead of appearing as unrelated advice.

## Priority 8 — Statistics and inference

The largest hole on the second axis. Research Methods covers deep learning and five ML
method pages; nothing covered inference. A new **Statistics and Inference** section was
added to hold it.

| Topic | Status |
|---|---|
| [Prediction vs inference](../stats/prediction-vs-inference.md) | Stub — the distinction the rest depends on |
| [Multiple comparisons](../stats/multiple-comparisons.md) | Stub — FWE, FDR, cluster inference, permutation |
| [Power and sample size](../stats/power-and-sample-size.md) | Stub — effect sizes, inflation under low power |
| [Mixed models](../stats/mixed-models.md) | Stub — longitudinal and multi-site designs |
| [Harmonization and site effects](../stats/harmonization.md) | Stub — ComBat; highest value here |

## Priority 9 — Ways to fool yourself

| Topic | Status |
|---|---|
| [Data leakage](../discussions/data-leakage.md) | Stub — subject, site, and preprocessing leakage |

Pairs with [imbalanced data](../discussions/imbalanced-data.md), which already covers
resampling inside the fold.

## Priority 10 — Tools used everywhere and documented nowhere

Each of these appeared across the book only as a name in passing.

| Topic | Status |
|---|---|
| [Nilearn](../tools/brain_imaging/nilearn.md) | Stub — the most-used Python neuro package here |
| [MNE-Python](../tools/brain_imaging/mne-python.md) | Stub — six projects use EEG/MEG |
| [FreeSurfer](../tools/brain_imaging/freesurfer.md) | Stub — and fMRIPrep already runs it |
| [Visualizing brain data](../tools/brain_imaging/visualization.md) | Stub — colormaps, thresholds, figures |
| [Testing research code](../tools/computer_science/testing-research-code.md) | Stub — pytest, CI, regression tests |

## Priority 11 — Privacy and review

| Topic | Status |
|---|---|
| [De-identification](../governance/de-identification.md) | Stub — defacing, DICOM scrubbing, PHI in sidecars |
| [Peer review](../publishing/peer-review.md) | Stub — preprints, response letters, reviewing |

## Priority 12 — Program and career

A different genre from the rest of the book and lower priority, but a fellowship is
fixed-term and the transition out of it is a real part of the experience here.

| Topic | Status |
|---|---|
| [The job market](../career/job-market.md) | Stub |
| [Conferences and community](../career/conferences-and-community.md) | Stub |

## Also outstanding

- **Testimonial sections are empty.** Every scholar page has a `Tools & Resources I Used`
  section awaiting its author. See the [scholar page template](bio-template.md). This is
  the highest-value outstanding item in the whole book and cannot be written by anyone
  other than the scholar concerned.
- **[Gourab K Sar's page](../cohort2/gourab_k_sar.md)** is not linked from any topic hub —
  his project needs a few more sentences before it can be categorized.
- **[Johanna Bayer's page](../cohort2/johannaBayer.md)** has no Project section; hers is
  currently embedded in the Background paragraph.
- **Extending the pathway.** [Start Here](../pathway/overview.md) has six steps. Once the
  governance and publishing pages exist, it should gain a step at each end.
- **Reference management.** Zotero, BibTeX and the citation workflow have no coverage.
  Deliberately not stubbed, because it overlaps [Zettelkasten](../tools/zettelkasten.md) —
  the better move is probably a section there than a page of its own.
- **COMPASS-ND and LORIS** are named in [Arman Hassanpour's project](../cohort2/arman_hassanpour.md)
  but neither has a page, unlike the other portals in [Databases](../data/portals/brain-code.md).

## Contributing

Every stub above states what it needs. Pick one, write it, open a PR — and if you have
actually done the thing, write from that experience rather than from the documentation.
The first-hand version is worth considerably more.
