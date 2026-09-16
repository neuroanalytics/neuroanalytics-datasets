# Harmonization and Site Effects

Pooling cohorts is a stated premise of several CNS projects — training on one dataset and
validating on another, or combining cohorts to reach a usable sample size. The obstacle is
that scanner and site leave a signature on the data that is often **larger than the
clinical effect being studied**. Different vendors, field strengths, head coils, and
acquisition parameters all shift derived measures like cortical thickness or connectivity
strength.

The danger is not merely added noise. If diagnosis is unevenly distributed across sites —
and it usually is, because each site recruits its own participants — then site is a
confound, and a classifier can reach excellent accuracy by learning which scanner produced
the image rather than anything about the brain. That failure mode is common enough that
apparent cross-cohort success should always prompt the question of whether site was
separable.

**ComBat**, borrowed from genomics batch correction, is the most widely used answer: model
site as a batch effect and remove it while preserving the biological covariates you name.
Variants extend it to nonlinear age effects and to longitudinal data. It is not magic —
harmonization cannot recover information an acquisition never captured, and removing site
variance will also remove real biological variance that happens to align with site.

:::{note} This page is a stub
Needs: ComBat and its variants with a worked example; what to protect as a covariate and
what happens when you forget; harmonizing derived features vs harmonizing images;
diagnosing residual site effects after correction; when a site random effect in a
[mixed model](mixed-models.md) is the better tool; the travelling-subject design.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Mixed models](mixed-models.md) — site as a random effect
- [Data leakage](../discussions/data-leakage.md) — site leakage across a train/test split
- [Neurobagel and semantic annotation](../tools/data_management/neurobagel.md) — harmonizing what variables *mean*
- [Transfer learning](../methods/transfer-learning.md) — the other approach to a domain shift
