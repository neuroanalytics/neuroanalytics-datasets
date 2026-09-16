# 6. Share your work

Reproducibility is not a step you do at the end. Everything here is cheaper to adopt at
the start of a project than to retrofit onto one — but if you are reading this partway
through, adopting it late still beats not adopting it.

## Version control

- **[Git overview](../tools/computer_science/git-overview.md)** — what git is for,
  setting up a repository, `.gitignore`, README conventions, and the collaboration
  problems that actually bite (merge conflicts, workflow confusion, undoing mistakes)
- **[Git examples](../tools/computer_science/git-examples.md)** — worked commands for the
  situations you will hit

## Environments

- **[Code environments](../tools/computer_science/code-environments.md)** — managing
  dependencies so your analysis runs somewhere other than your laptop
- **[Containers](../tools/computer_science/containers.md)** — pinning an entire software
  stack. This is how the preprocessing pipelines are distributed, and the most reliable
  way to make a result reproducible years later

An analysis that cannot be re-run is difficult to publish and impossible to hand over.
The point of both pages is that "it worked on my machine in 2024" is not a result.

- **[Testing research code](../tools/computer_science/testing-research-code.md)** — a
  pinned environment guarantees the code runs, not that it is correct. Tests are the other
  half

## Getting it published

- **[Reporting guidelines](../publishing/reporting-guidelines.md)** — what a methods
  section has to state
- **[Peer review](../publishing/peer-review.md)** — preprints, responding to reviewers, and
  reviewing for others
- **[Executable articles](../publishing/executable-articles.md)** — when the paper itself
  can be re-run

## Sharing data and results

- **[BIDS](../tools/data_management/bids.md)** — makes a dataset FAIR, and shareable
  without a manual
- **[OpenNeuro](../data/portals/open-neuro.md)**,
  **[CONP](../data/portals/conp-data-portal.md)**,
  **[DANDI](../data/portals/dandi.md)** — where to deposit data
- **[Releasing data and code](../publishing/data-and-code-release.md)** — DOIs, versioning,
  and citation
- **[External resources](../external.md)** — further reading

## Contribute back to this book

You have just been through this path. The parts that were harder than they needed to be
are exactly what the next person needs to know.

If you are a scholar, your page has a **Tools & Resources I Used** section waiting for
you — see the [scholar page template](../contributing/bio-template.md). A paragraph on
which tool you picked, what was painful, and what you would tell someone starting today is
the most useful thing you can leave behind.

For everything else, see the [formatting notes](../markdown.md), the
[coverage roadmap](../contributing/roadmap.md) for known gaps worth filling, and open a PR.
