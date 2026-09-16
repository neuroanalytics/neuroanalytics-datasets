# Data Leakage

Leakage is when information from your test set reaches your model during training. The
result is an accuracy figure that is real in the sense that you computed it correctly, and
worthless in the sense that it will not survive contact with new data. It is the single
most common reason a promising neuroimaging classifier fails to replicate, and it is
usually invisible — the code runs, the numbers look good, nothing errors.

Three forms account for most cases in this field:

- **Subject leakage.** The same participant contributes scans to both training and test
  sets. With longitudinal cohorts this happens by default if you split on scans rather than
  on people. The model recognizes the individual, not the condition.
- **Site leakage.** Splitting randomly across a multi-site cohort puts every site on both
  sides, so the model never has to generalize to an unseen scanner — the thing you actually
  need it to do. See [harmonization](../stats/harmonization.md).
- **Preprocessing leakage.** Any step fitted on the full dataset before splitting —
  feature selection, normalization, dimensionality reduction, imputation — carries test-set
  information into training. Selecting the top-k features by their correlation with the
  outcome across all subjects is the classic version, and it can manufacture strong
  accuracy from pure noise.

The general rule: the split comes first, and every fitted transformation is fitted inside
the training fold only.

:::{note} This page is a stub
Needs: a worked example manufacturing high accuracy from random data via feature
selection; grouped and stratified cross-validation in `scikit-learn`; leave-one-site-out
as the honest protocol for multi-site work; leakage through hyperparameter tuning and the
need for a nested loop; how to audit a pipeline you inherited.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Working with imbalanced data](imbalanced-data.md) — its "Combination and Proper Cross-Validation" section covers resampling inside the fold
- [Harmonization and site effects](../stats/harmonization.md)
- [Prediction vs inference](../stats/prediction-vs-inference.md)
- [Transfer learning](../methods/transfer-learning.md)
