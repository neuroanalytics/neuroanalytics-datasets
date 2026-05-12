# The UK Biobank (UKB or UKBB)

## Introduction

The UK Biobank is a comprehensive dataset that collects extensive
health and genetic information from a significant portion of the UK
population over time. It includes detailed genetic data such as SNPs,
haplotypes, and DNA sequences, alongside personal medical records like
blood samples, health insurance details, and professional
history. This biobank serves as a long-term resource for research
purposes, aiding in studies on health conditions linked to genes,
understanding heritability, and tracking genetic diversity within the
UK population. Its primary goal is to enhance research capabilities by
providing high-quality datasets that can be used to identify
biomarkers, develop new treatments, and explore genetic factors
affecting disease outcomes. While it primarily focuses on the UK,
there may be international inclusion as people from other countries
living in the UK contribute data. Challenges include maintaining
privacy and ensuring anonymization of collected information, while
managing the immense size of the dataset requires robust computational
resources.

The UK Biobank includes both functional (fMRI) and structural
neuroimaging to study brain function and structure comprehensively. To
ensure privacy and data security, the UK Biobank employs specific
protocols for each imaging technique, ensuring consistency across
individuals through standardized formats, handling of patient data,
and pseudonymization or pseudopatience. The protocol may include
long-term longitudinal studies with multiple scans over time to track
individual changes in brain structure and function. The UK Biobank may
also manage different imaging modalities separately, such as MRI
vs. fMRI, to analyze relationships between structural and functional
alterations. Data storage and processing are optimized for efficiency
without compromising privacy or integrity, accommodating the extensive
volume of data collected from 100,000 individuals.

Overall, the UK Biobank's neuroimaging protocol ensures consistency,
privacy protection, and efficient dataset management, facilitating
research into brain-related health and genetic factors.

## Working with the UK Biobank

The primary advantage of the UKBB is the sheer number of subjects
available within the dataset for analysis. This poses many challenges,
as even tabular data can be many gigabytes (GB) in size, with the
imaging source data being many terrabytes (TB), and the source genomic
data being many hundred terrabytes (TB).

### The UK Biobank at McGill

McGill has a local download of the imaging and clinical data stored on
Beluga, a Compute Canada HPC system. This dataset is supposed to be
periodically updated with the new releases from the UKBB and currently
contains the summarized genomic data, behavioral and medical
variables, and the source imaging data for approximately 50,000 of the
participants baseline imaging data and another ~5,000 of the
longitudinal cohort.

The imaging files are stored as `.squashfs` disk images. These are a
read-only compressed archive of the source files. Importantly, these
images can be easily used by tools like apptainer that can `--bind`
these disk images as a hard-drive, allowing the user to read the data
stored within. This approach facilitates the storage of this data on
Compute Canada shared systems by reducing the space needed to store it
and preventing the data from being corrupted.

### The Research Analysis Platform (RAP)

The UKBB is pivoting to a cloud only model, with local copies of data
being largely discouraged. The RAP is an Amazon cloud instance that
lets users create the data derivates they need on the cloud from all
the available contributed resources. Ideally, this system will prevent
abuse of the data and facilitate the reuse of common derivatives.

Leverage high-performance cloud computing to conduct varied analyses
with greater efficiency and peace of mind. The UKB-RAP advertised
features of the RAP are:

- **Built-in tools for data organisation and analysis** – including
  JupyterLab, RStudio, Apache Spark, WDL Workflow, and Nvidia
  Parabricks.
- **Custom software installation** – tailor the platform to your
  specific research needs, including support for your own tools and
  Docker containerisation.
- **Integrated storage, computation, and analysis** – eliminating
  external storage costs while enhancing data security.
- **Seamless collaboration** – share analyses and datasets with
  colleagues within a secure environment.

## Tools for working with UKBB

### A *MyST*ical article for UKBB Scaling Reports

https://github.com/SIMEXP/ukbb_scaling_reports

This is an example of a structured article that can be managed as a
git repository. The code cotained within is a reproducible analysis
that can be easily evaluated on a subset of the UKBB data.

### UKBB Parser

https://github.com/USC-IGC/ukbb_parser

The UKBB uses a relatively dense coding convention for mapping the
many behavioral variables they provide. This tool provides a pythonic
way of seclecting and parsing the desired variables using these codes.
