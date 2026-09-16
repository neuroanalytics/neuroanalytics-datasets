# Prediction vs Inference

Two questions look similar and are not. *Does hippocampal volume differ between groups?*
is an inference question — it asks about a parameter in a population. *Can I tell from
this scan whether someone will convert to dementia?* is a prediction question — it asks
about accuracy on data you have not seen.

The methods diverge accordingly. Inference cares about standard errors, confounds, and
whether an effect is distinguishable from zero; it is evaluated on the data you have.
Prediction cares about generalization; it is evaluated on data held out from fitting. A
model can be excellent at one and useless at the other. A regression with a highly
significant coefficient can predict individual outcomes barely better than chance, because
a reliable group-level difference and a separable individual-level signal are different
things — and in clinical neuroimaging, the gap between them is usually large.

This matters here because most of the [Research Methods](../deep-learning.md) section is
predictive while most of the clinical literature scholars read is inferential. Knowing
which question you are answering determines what counts as evidence, what the reviewers
will ask for, and whether a p-value or a held-out AUC belongs in your abstract.

:::{note} This page is a stub
Needs: worked contrast of the same dataset analysed both ways; why a significant
coefficient and a useful classifier come apart; when explanation is genuinely the goal
and predictive framing is a distraction; the interpretability question that follows
(a model that predicts well but cannot say why).

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Multiple comparisons](multiple-comparisons.md) — the inference side
- [Data leakage](../discussions/data-leakage.md) — the prediction side
- [Deep learning](../deep-learning.md)
- [Normative modelling](../methods/normative-modelling.md) — sits deliberately between the two
