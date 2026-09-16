# Visualizing Brain Data

A brain figure has to do two things at once: show a spatial pattern honestly, and survive
being reduced to a two-column width in print. Most published brain figures fail at one of
them, usually by rendering a continuous statistical map with a rainbow colormap that
invents boundaries the data does not contain.

The choices that matter are mostly about colour and thresholding. Perceptually uniform
sequential maps for magnitudes, diverging maps centred at zero for contrasts, and never a
rainbow for continuous data — the perceived rate of change in a jet colormap has nothing
to do with the numeric rate of change, which means the reader sees structure that is an
artefact of the palette. Thresholding decisions deserve the same scrutiny: a map shown at
an arbitrary uncorrected threshold, with the threshold unstated, is a figure that cannot
be assessed.

Surface rendering versus volume slices is a genuine choice rather than a matter of taste.
Surfaces show cortical patterns without the foreshortening that makes gyral effects look
patchy on slices; slices remain the honest option for subcortical structures, which have no
surface.

:::{note} This page is a stub
Needs: the main tools — [Nilearn](nilearn.md) plotting, surface rendering, connectome
displays — with a worked figure from each; colormap guidance including colour-blind-safe
choices; how to state thresholds so a figure is interpretable; glass brains and when they
mislead; connectivity matrices vs node-edge diagrams; getting to a journal's resolution
and format requirements without re-rendering everything.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Nilearn](nilearn.md) — the plotting workhorse
- [Multiple comparisons](../../stats/multiple-comparisons.md) — what the threshold means
- [Reporting guidelines](../../publishing/reporting-guidelines.md) — what a figure must state
- [Brainstorm](brainstorm.md) — electrophysiology visualization
