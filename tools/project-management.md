# Project Management Tools

This is an overview of a variety of tools, platforms, and philosophies
of managing research data in a FAIR context.

## Reproducibility Benefits

The advantages of incorporating **reproducible methods** into a
research management workflow are numerous, benefiting the individual
researcher, the research team, and the broader scientific
community. These advantages primarily center around minimizing risk,
improving documentation, facilitating collaboration, and enhancing
reputation.

Key advantages include:

### 1. Risk Mitigation and Error Prevention
Reproducible methods protect against loss of work and ensure the
reliability of results:
*   **Avoid Disaster:** Reproducible practices help **avoid disaster**
	by ensuring that data and analyses are not lost ("this was my only
	copy!"). This is often tied to good **Research Data Management
	(RDM)**, which includes recommendations, schemes, and standards
	for practices like storage and backup.
*   **Ensuring Consistency:** When analysis is **reproducible** (same
	data, same analysis), it ensures that the same results can be
	obtained consistently, validating the integrity of the
	computational steps. The use of standardized structures, such as
	the BIDS standard for neuroimaging data, contributes significantly
	to this consistency.

### 2. Improved Documentation and Reporting
Structured, reproducible workflows streamline the process of
publishing and sharing findings:
*   **Helps Writing Papers:** Reproducible methods **help writing
	papers**. Tools and frameworks like Nipoppy, which manage
	configurations (`config.json`, `descriptor.json`,
	`invocation.json`), simplify the generation of detailed
	methodology sections.
*   **Reviewer Trust:** Reproducibility **helps reviewers see it your
	way**. Clear methods and documented steps allow reviewers and
	journal editors to verify the claims easily.
*   **Supports Transparency (FAIR Principles):** RDM and reproducible
	analyses make research **FAIR** (Findable, Accessible,
	Interoperable, and Reusable). Repositories and standardized
	formats ensure proper documentation, persistent identifiers, and
	licenses are applied to research outputs. Sharing derived results
	in an **interoperable and understandable way** makes them reusable
	and reproducible.

### 3. Enhanced Collaboration and Continuity
Reproducibility ensures that projects can be easily continued,
extended, and utilized by others:
*   **Enables Continuity:** Reproducible methods **enable continuity**
	of the project. If a team member leaves, the next person can
	easily understand and pick up the workflow without ambiguity.
*   **Facilitates Open Science and Data Sharing:** Reproducible Data
	Management (RDM) greatly facilitates **data sharing and Open
	Science**. When code and data are placed in containers (like
	Docker or Singularity) and described via formal standards (like
	BIDS Apps API), other researchers can quickly adopt and run the
	analysis pipeline. Nipoppy, for instance, uses containerized
	software and descriptor files to manage the execution of
	pipelines, making them portable and reusable across different
	computing environments.
*   **Avoids Duplication of Effort and Funding:** Sharing reproducible
	work prevents other researchers from unnecessarily duplicating
	efforts, which is beneficial to the overall scientific ecosystem.

### 4. Professional and Career Benefits
*   **Builds Reputation:** Reproducible work **helps to build your
	reputation** in the scientific community.
*   **Increases Citation Count:** Open source work is **more likely to
	be reused and cited**, benefiting the author.

## Dataset Hosting

Based on the sources, several resources are available to host and
share neuroimaging datasets, ranging from domain-specific repositories
to general-purpose platforms, all designed to support Research Data
Management (RDM) and Open Science principles.

### Dedicated Neuroimaging Data Repositories

These repositories are specifically recognized within the neuroimaging
and neuroscience research community:

*   **LORIS:** This system was customized for data entry, storage, and
	dissemination of the PREVENT-AD program data.
	*   The PREVENT-AD cohort uses **two LORIS databases** for data
		dissemination: one for **Open Access** (for basic demographics
		and longitudinal neuroimaging raw data, e.g.,
		`https://openpreventad.loris.ca`) and one for **Registered
		Access** (for sensitive material like cognitive, medical,
		neurosensory, genetic, and CSF data, e.g.,
		`https://registeredpreventad.loris.ca`).
	*   The PREVENT-AD repositories are also discoverable via the
		**Canadian Open Neuroscience Platform (CONP)** interface
		(`https://portal.conp.ca`).

*   **OpenNeuro:** This platform is listed as a repository for data
	sharing.
	*   It is considered **very popular for neuroimaging data**,
		especially data following the Brain Imaging Data Structure
		(**BIDS**) standard.
	*   It is designed for **publishing** certain neuroimaging
		modalities, uses the BIDS standard, and supports **version
		control**. However, it generally does not offer access
		control, though there may be an embargo period.

*   **Brainlife:** This platform is available for **work and
	publishing** of neuroimaging data.
	*   It supports the **BIDS** standard and offers **access
		control** and **version control**.

*   **OMEGA (Open MEG Archive):** This repository is specifically
	mentioned as the place where the raw MEG and T1-weighted MRI scans
	from PREVENT-AD participants are available.

*   **NITRC (Neuroimaging Tools and Resources Clearinghouse):** This
	repository publishes neuroimaging data, tools, and other
	resources.
	*   It **already hosts popular datasets** such as ABIDE, OASIS,
		and ADHD\_200.
	*   It offers access control and provides a file preview function.

*   **DANDI:** This repository publishes **physiology** data and
	supports the **BIDS** standard, **access control**, and **version
	control**.

*   **NeuroVault, TemplateFlow, and EBRAINS:** These platforms are
	listed as options for sharing **derived data and results**.

### General-Purpose Data Repositories

These platforms can host various types of research data, including
neuroimaging data derivatives:

*   **Zenodo:** This platform is a repository used for publishing
	**any** kind of data.
	*   It is often utilized in the context of open science for
		uploading and installing community-developed pipelines (like
		those used in Nipoppy).
	*   It is considered intuitive to navigate, offers access control,
		and supports version control, although it has storage limits.

*   **OSF (Open Science Framework):** This tool promotes open,
	centralized workflows throughout the research lifecycle, including
	**storing and analyzing collected data**.
	*   It is suitable for **work and publishing** of **any** data
		type.
	*   It provides a unique, **persistent uniform resource locator
		(URL)** or digital object identifiers (DOIs) and archival
		resource keys (ARKs).
	*   It features built-in **version control** and allows
		third-party add-ons or integrations.

*   **Figshare:** This repository is used for publishing **any** type
	of data.
	*   It is noted for being fast in up- and download, which is good
		for handling big data in large chunks.

*   **NDA (National Database for Autism Research) (Implied):** Listed
	as a repository for publishing **any human data**.

*   **GIN:** This tool is used for both **work and publishing** and
	can handle **any** kind of data.

### Considerations for Choosing a Repository

When selecting a repository, researchers should ideally submit data to
a **domain or discipline-specific, community-recognized
repository**. Discipline-specific repositories often have more
functionalities tailored for the type of data being
shared. Repositories ensure **long-term archiving** and preservation,
which differentiates them from collaborative development tools like
GitHub or GitLab. Repositories promote the **FAIR Principles**
(Findable, Accessible, Interoperable, Reusable) by assigning
persistent identifiers, using metadata standards (like BIDS),
providing licenses, and determining access levels.

## Project Frameworks

The sources describe several types of frameworks that exist for
research project management, ranging from high-level guiding
principles and documentation standards to comprehensive data and
pipeline management systems, particularly within the field of
neuroinformatics.

### Research Data Management (RDM) Frameworks

Research Data Management (RDM) provides the fundamental
recommendations, schemes, and standards for practices of storing and
annotating research data. RDM supports adherence to the Research Data
Lifecycle. Key formal documentation frameworks include:

*   **Research Data Management Plan (RDMP):** This is a formal, living
	document that outlines the handling of research data both during
	and after the project. It functions as a roadmap for efficiently
	and securely managing data. RDMPs often detail the following
	aspects of a project:
	*   **Types of data** (source, format, volume).
	*   **Contextual details** (metadata and documentation).
	*   **Storage, backup, and security** procedures.
	*   **Provisions for protection/privacy**.
	*   **Policies for re-use** by other researchers.
	*   **Access and sharing** plans.
	*   **Archiving and providing access** (long-term preservation
		plans).
	*   **Roles and plan oversight** (who is responsible for
		implementation).
*   **Software Management Plan (SMP):** A formal plan that describes
	how a specific software project will be developed and maintained,
	including version control, licensing, and license
	compatibility. This plan details how the software will be made
	available to users and how its contribution to research will be
	measured.
*   **FAIR Guiding Principles:** RDM makes research
	**FAIR**—**F**indable, **A**ccessible, **I**nteroperable, and
	**R**eusable. Repositories support these principles by assigning
	persistent identifiers, using metadata standards, and providing
	licenses.

### Neuroimaging Workflow and Data Management Frameworks

These tools facilitate open science and structured research
collaboration across the entire project lifecycle:

*   **Open Science Framework (OSF):** OSF is a tool that promotes
	**open, centralized workflows** and captures various aspects of
	the research lifecycle, including developing ideas, designing
	studies, storing/analyzing collected data, and writing/publishing
	reports or preregistrations. It features built-in **version
	control** and persistent URLs/DOIs for research objects.
*   **Project Organization Templates:** Frameworks that provide
	reusable templates for structuring project folders and files, such
	as:
	*   **Cookiecutter**.
	*   **TONIC Research Folder Structure**.

### Version Control Tools

These tools are essential components of reproducibility and project
management frameworks:

*   **Code and Data Versioning:** Tools mentioned for version
	controlling code, data, and text include Git, SVN, Git LFS (Git
	Large File Storage), git-annex, DataLad, and GIN.
*   **Containerization:** Tools like **Docker** and
	**Singularity/Apptainer** are used to containerize software, which
	is crucial for reproducible pipeline deployment within frameworks
	like Nipoppy.
