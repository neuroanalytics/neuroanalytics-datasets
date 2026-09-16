# De-identification

A structural MRI is identifiable. A surface reconstruction of a face can be matched to a
photograph well enough that treating T1 scans as anonymous is not defensible, which is why
**defacing** — removing or distorting facial features while preserving brain tissue — is a
precondition for sharing structural data rather than an optional courtesy.

The less obvious risk is metadata. DICOM headers carry patient names, birth dates,
institution names, accession numbers, and referring physician fields; some vendors write
identifiers into private tags that generic anonymizers miss. Conversion to
[BIDS](../tools/data_management/bids.md) drops most of this, but BIDS sidecar JSONs inherit
selected fields, and acquisition dates can persist and be enough to re-identify someone in
combination with other records. Participant lists are their own hazard: a spreadsheet
mapping study IDs to names, stored beside the data because it was convenient, is the most
common failure in practice.

Defacing is also not free. It can disturb registration and segmentation, so it belongs
early in the pipeline where its effects can be checked, not bolted on at sharing time.

:::{note} This page is a stub
Needs: the defacing tools and how they compare; validating that defacing worked; DICOM
header scrubbing and the private-tag problem; which BIDS sidecar fields carry risk; date
shifting; where the line sits between de-identified and anonymized under Canadian privacy
law, and what your REB will expect; how this constrains [releasing data](../publishing/data-and-code-release.md).

If you have done this on a CNS project, you are the right person to write it — open a PR.
:::

## Related

- [Ethics and data access](ethics-and-data-access.md)
- [Data management plans](data-management-plans.md)
- [BIDS](../tools/data_management/bids.md)
- [Releasing data and code](../publishing/data-and-code-release.md)
