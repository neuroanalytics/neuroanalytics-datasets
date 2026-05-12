# [CONP Data Portal](https://portal.conp.ca/)

The Canadian Open Neuroscience Platform (CONP) Portal is a web interface that facilitates open science for the neuroscience community by simplifying global access to and sharing of datasets and tools. The Portal internalizes the typical data cycle of a research project, beginning with data acquisition, followed by data processing with published tools, and ultimately the publication of results with a link to the original dataset.

## Description

The CONP Portal was built using technologies and best practices that make sharing easier and reproducible. DataLad and Git-Annex are used to track and index datasets, while Boutiques is used in conjunction with a container engine (e.g. Docker or Singularity) to ensure reproducibility of results. In addition, some pipelines can also be run using High Performance Computing (HPC) via links to the CBRAIN platform.

### Dataset and tool search engines

The CONP Portal contains a diverse range of datasets, primarily neuroimaging, but also transcriptomics, genomics, and other related data modalities. Many of these datasets come from neuroscience research institutes, while others link to public resources that may be of interest to neuroscientists.

### Streamlined data download capabilities

Access to datasets vary from open availability to agreeing to third party data agreements.

### Processing standarization with Boutiques descriptors

Note that in addition to the ability to run the tools/pipelines locally, it is possible to run some tools/pipelines via the CBRAIN platform (for example: the fmriprep BIDS App, Zenodo.3359640). Clicking on the blue icon "Use This Tool On CBRAIN" will bring you to the CBRAIN page where you can run pipelines on the super-computing network administered by the Digital Research Alliance of Canada. For more information on CBRAIN, visit http://www.cbrain.ca/.

## Datasets

- [Calgary Campinas](https://portal.conp.ca/dataset?id=projects/calgary-campinas)
- [MICA-MICS](https://portal.conp.ca/dataset?id=projects/mica-mics)
- Parkinson's Disease
  - [Tawou](https://portal.conp.ca/dataset?id=projects/Taowu)
  - [Neurocon](https://portal.conp.ca/dataset?id=projects/Neurocon)
- [SIMON](https://portal.conp.ca/dataset?id=projects/SIMON-dataset)
- [BigBrain](https://portal.conp.ca/dataset?id=projects/bigbrain-datalad)
- any many more...

## Datatypes

- Clinical
- Imaging (MRI, fMRI, dMRI)
- Electrophysiological (EEG, MEG)
- Ocular
- Genomic
- Proteomic
- Demographic

## Register

Follow the link [here](https://portal.conp.ca/index#) on Terms of Use at the top to
sign up for an account. Additional agreements for different datasets
may be necessary.
