# Testing Research Code

Research code is usually written to be run once, by its author, on one dataset — and then
it is run for three years, by four people, on six datasets. The gap between how it was
written and how it ends up being used is where silent errors live. A retracted finding
traced to a spreadsheet indexing mistake is not a story about carelessness; it is a story
about code that nothing checked.

Testing analysis code is not the same as testing software. You are rarely verifying a
function against a specification. More often you are pinning behaviour: this pipeline, on
this small fixture, produced these numbers yesterday, and if it produces different ones
today something changed and you should know before it reaches a manuscript. That style —
regression tests against known outputs — catches most of what actually goes wrong.

The highest-value targets are narrow: coordinate and index conventions, unit conversions,
anything reading a file format with a header, and any function where an off-by-one is
plausible and silent. You do not need coverage of a plotting script. You need a test on
the function that decides which subjects are excluded.

:::{note} This page is a stub
Needs: `pytest` basics oriented to analysis code; building a tiny fixture dataset that
runs in seconds; regression tests on pipeline outputs and how to store the expected
values; GitHub Actions on a research repo without burning the free minutes; testing code
that requires data you cannot redistribute; where property-based testing pays off.

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Git overview](git-overview.md) and [git examples](git-examples.md)
- [Coding environments](code-environments.md) — a test is only meaningful if the environment is pinned
- [Containers](containers.md) — the strongest form of the same guarantee
- [Programming obscurities](../../discussions/coding-obscurities.md) — the class of bug tests are for
