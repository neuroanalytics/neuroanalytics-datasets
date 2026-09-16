# Power and Sample Size

Statistical power is the probability of detecting an effect that is genuinely there. A
study with 20% power will miss a real effect four times in five — and, less intuitively,
the significant results it *does* produce will be badly inflated. This is the part that
surprises people: underpowered studies do not merely fail to find things, they
systematically overestimate whatever they find, because only the largest noise-inflated
estimates clear the significance threshold.

That mechanism explains a good deal of the replication difficulty in brain-behaviour
research. Effect sizes for individual-difference associations in neuroimaging are
typically small, samples have historically been in the dozens, and the published estimates
were correspondingly optimistic. It is also why the large consortium cohorts documented in
this book — [UK Biobank](../data/databases/ukbb.md), [ABCD](../data/databases/abcd.md),
[ADNI](../data/databases/adni.md) — exist in the form they do.

Power analysis is most useful *before* data collection, but scholars here are usually
working with a fixed existing cohort. In that situation the honest version of the question
is inverted: given this sample, what is the smallest effect I could reliably detect, and
is that plausibly the size of the effect I am looking for?

:::{note} This page is a stub
Needs: how to run a power analysis for the common designs here; effect size measures and
how to interpret them for imaging data; the inflation-under-low-power argument shown
concretely; sensitivity analysis for a fixed cohort; power for predictive models, where
the relevant quantity is test-set size rather than n.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Multiple comparisons](multiple-comparisons.md) — corrections cost power
- [Mixed models](mixed-models.md) — repeated measures change the calculation
- [Pre-registration](../governance/pre-registration.md)
- [Working with imbalanced data](../discussions/imbalanced-data.md) — effective n is smaller than it looks
