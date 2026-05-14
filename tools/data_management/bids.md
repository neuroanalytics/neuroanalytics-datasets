# The Brain Imaging Data Structure (BIDS)

https://bids.neuroimaging.io/index.html

Experiments in neuroscience result in complex data that can be
arranged in many different ways. For a long time, there was no
consensus how to organize and share experimental data. Even two
researchers working in the same lab could opt to arrange their data in
a different way. Lack of consensus (or a standard) leads to
misunderstandings and time wasted on rearranging data or rewriting
scripts expecting certain structure. With the Brain Imaging Data
Structure (BIDS), we describe a simple and easy to adopt way of
organizing neural and associated, such as behavioral, data.

BIDS was heavily inspired by the format used internally by the
OpenfMRI repository that is now known as OpenNeuro. While working on
BIDS we consulted many neuroscientists to make sure it covers most
common experiments, but at the same time is intuitive and easy to
adopt. The specification is intentionally based on simple file formats
and folder structures to reflect current lab practices and make it
accessible to a wide range of scientists coming from different
backgrounds.

![image](../../images/dcm-to-bids.png)

By using this standard you will benefit in the following ways:

- It will be easy for another researcher to work on your data. To
  understand the organization of the files and their format you will
  only need to refer them to this document. This is especially
  important if you are running your own lab and anticipate more than
  one person working on the same data over time. By using BIDS you
  will save time trying to understand and reuse data acquired by a
  graduate student or postdoc that has already left the lab.

- There is a growing number of data analysis software packages that
  can understand data organized according to BIDS.

- Databases such as OpenNeuro.org, LORIS, COINS, XNAT, SciTran, and
  others will accept and export datasets organized according to
  BIDS. If you ever plan to share your data publicly (nowadays some
  journals require this) you can speed up the curation process by
  using BIDS.

- There are validation tools (also available online) that can check
  your dataset integrity and let you easily spot missing values.

## FAIR-ness of BIDS

A key motivation for the BIDS structure is creating an easy machine
and human parsable structure for research data. By facilitating both
the re-use of the data through standards of data structure while
maintaining the clarity of what data is available to the user, BIDS
provides an elegant solution to the findability, accessibility,
interoperability, and reusability of research data and their derivatives.

## Tools

### BIDS Validator

https://bids.neuroimaging.io/tools/validator.html

The BIDS Validator is a tool that checks if a dataset is compliant
with the BIDS standard. The validator is available for use within
several different environments to best suit individual user
preferences and use cases, those versions are:

- A web browser based version
- Command line version
- Docker based version
- A python library installable from pypi or conda-forge

Instructions to install and use these versions can be found within the
quickstart guide at the BIDS Validator repository.

### Nipoppy

Nipoppy is a framework to facilitate the lifespan management of a
project where you plan to utilize BIDS.

Please read more on the **Nipoppy** page.

## References

Gorgolewski, K. J., Auer, T., Calhoun, V. D., Craddock, R. C., Das,
S., Duff, E. P., ... & Poldrack, R. A. (2016). The brain imaging data
structure, a format for organizing and describing outputs of
neuroimaging experiments. Scientific data, 3(1), 1-9.

Wilkinson, M. D., Dumontier, M., Aalbersberg, I. J., Appleton, G.,
Axton, M., Baak, A., ... & Mons, B. (2016). The FAIR Guiding
Principles for scientific data management and stewardship. Scientific
data, 3(1), 1-9.
