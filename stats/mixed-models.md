# Mixed Models

Nearly every cohort in this book is longitudinal or multi-site or both.
[PREVENT-AD](../data/databases/prevent-ad.md) follows people over years,
[ADNI](../data/databases/adni.md) and ONDRI scan the same participants repeatedly, and
[ABCD](../data/databases/abcd.md) does it across 21 sites. In all of these, observations
are **not independent** — two scans from one participant resemble each other more than two
scans from different participants, and the standard tests assume otherwise.

Mixed-effects models handle this by splitting variance into *fixed* effects (the
population-level relationships you want to estimate) and *random* effects (systematic
variation attached to participants, sites, or scanners). The practical consequence of
ignoring the structure is inflated false positives: treating 300 scans from 100 people as
300 independent observations pretends you have three times the information you actually
have.

They also handle unbalanced designs gracefully, which matters because real longitudinal
cohorts are always unbalanced — people miss visits, drop out, or enrol late. Methods that
require complete data force you to discard participants; mixed models use the observations
you have.

:::{note} This page is a stub
Needs: random intercepts vs random slopes and how to choose; specifying a model in
`lme4` and `statsmodels`; the convergence failures everyone hits and what they mean;
handling missing visits and dropout; when site belongs as a random effect versus needing
[harmonization](harmonization.md); extending to voxelwise or connectome-wide data.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Harmonization](harmonization.md) — the other way to deal with site
- [Power and sample size](power-and-sample-size.md)
- [Aging](../research_topics/aging.md) — where longitudinal designs concentrate here
- [Normative modelling](../methods/normative-modelling.md)
