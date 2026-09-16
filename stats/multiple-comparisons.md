# Multiple Comparisons

A whole-brain analysis runs one test per voxel. At roughly 200,000 voxels and an
uncorrected threshold of `p < 0.05`, about 10,000 will pass by chance alone. The
threshold that is standard for a single test is meaningless applied 200,000 times, and
correcting for that is not optional bookkeeping — it is the difference between a result
and an artefact.

There are two families of correction and they answer different questions.
**Family-wise error** control (Bonferroni, random field theory) bounds the probability of
*any* false positive — conservative, appropriate when a single false claim is costly.
**False discovery rate** control (Benjamini–Hochberg) bounds the expected *proportion* of
discoveries that are false — more permissive, appropriate when you are generating
candidates for follow-up. Cluster-based methods exploit spatial structure by testing
contiguous blobs rather than isolated voxels, which is more sensitive but makes the
inference about the cluster, not about any voxel inside it.

Permutation testing is the most defensible general answer: shuffle the labels, rebuild the
null distribution from your own data, and avoid assuming a parametric form that
neuroimaging data frequently violates. It costs compute, which is the main reason people
skip it.

:::{note} This page is a stub
Needs: FWE vs FDR with a worked example and when each is defensible; what cluster-based
inference does and does not license you to claim; permutation testing in practice and its
cost on a cluster; the connectome case, where the multiplicity is edges rather than
voxels; how this interacts with pre-registering a threshold.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Prediction vs inference](prediction-vs-inference.md)
- [Power and sample size](power-and-sample-size.md) — corrections cost power
- [Pre-registration](../governance/pre-registration.md) — fixing the threshold in advance
- [Reporting guidelines](../publishing/reporting-guidelines.md) — what must be stated
