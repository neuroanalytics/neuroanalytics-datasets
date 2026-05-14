# UK Biobank Rapid Access Portal (RAP)

The basic steps for getting started with an analysis on the Research
Analysis Platform (RAP) generally follow a three-step quickstart
process, covering account setup, project configuration, and initial
data access.

**BEWARE** - Work on this platform requires payment to be established.

## Basic Overview

### 1. Create a UKB-RAP Account

Before accessing the RAP, you must:
1.  **Create a UK Biobank Access Management System (AMS) account** and
	ensure you have received UK Biobank access approval from the
	Access Management Team (AMT).
2.  Ensure you are listed as a collaborator on a UK Biobank-approved
	access application.

Once prerequisites are met, the steps for account creation and
connection are:
1.  Navigate to the Research Analysis Platform and click **Create an
	Account** (if you don't have an existing DNAnexus account).
2.  Fill out the **Create New Account** form, click **Create
	Account**, and activate the account via the emailed link.
3.  Log into the Platform, complete your profile, and click **Access
	Platform**.
4.  Connect your RAP account to your AMS account by accepting the
	Terms of Service and logging into the AMS when prompted. Your
	accounts are now connected, and you can use the Platform.

### 2. Create a Project and Dispense Data

All work on the RAP takes place within the context of a
project. Projects allow a defined set of users to access specific
datasets and conduct analyses.

1.  On the RAP **Projects** screen, click the **New Project** button.
2.  In the **New Project** wizard, enter a **Project Name**.
3.  Enter the approved UK Biobank **Application ID**. Note that every
	project must be linked to **one and only one** access application.
4.  Check the **Dispense data to the project** option to populate the
	project with data specified on the linked access application.
5.  Choose a wallet for billing activities in the **Billed To** field.
6.  Specify access permissions for copying, deleting, and downloading
	data in the **Access** section.

The process of dispensing data, due to the large size of the UK
Biobank data, can take time—sometimes over an hour, or potentially
between 20 minutes to 2 hours. You can monitor the status and
percentage of populated data on the project list. Tabular data and
linked health data are placed into a Spark SQL database and an
associated dataset, while bulk data fields are dispensed as files
within the **/Bulk** folder of your project.

### 3. Accessing Data and Starting Analysis

Once the data has been dispensed, you can start exploring and
analyzing the data:

#### Accessing Datasets via the Cohort Browser
The Cohort Browser is a key tool for exploration, cohort definition,
and visualization.
1.  **Access the dataset:** Datasets must be prepared and ingested to
	be accessible via the Cohort Browser. To open a dataset, navigate
	to the project's root folder and click the dataset (or select the
	dataset and click **Explore Data**).
2.  **Explore Data:** In the Cohort Browser's **Overview** tab, you
	see visualizations providing an introduction to the dataset. You
	can click the **Add Tile** button to explore what fields are
	available, which are organized in a folder structure inspired by
	the UK Biobank Showcase.
3.  **Define a Cohort:** When you start exploration, an empty cohort
	is automatically created. You can narrow down your cohort by
	clicking the **Add Filter** button and selecting criteria based on
	data fields, genomic data, or join filters.

The Cohort Browser features can be used to further explore data,
create charts (by adding data fields as tiles), or define and compare
cohorts. Cohorts can be saved for later use or exported.

#### Advanced Analysis Options
*   **Statistical Analysis using JupyterLab/Spark:** Bioinformaticians
	can perform statistical analysis by creating **JupyterLab
	environments** backed by **Spark clusters** to directly query
	their data and create dataframes within a Python or R environment
	for further analysis.
*   **Running Analysis Jobs:** Once analysis jobs are run (e.g., using
	analysis apps or workflows), you can view their progress in the
	**Monitor** tab. This includes checking the status of jobs
	(running, errored out, successfully completed, or terminated).
*   **Machine Learning (ML) Models:** The RAP supports machine
	learning analysis and training models on imaging data, often
	leveraging Graphics Processing Units (GPUs). This is often done by
	selecting a JupyterLab session from tools and choosing a GPU
	instance type, such as `mem3 gpu`. Several ML frameworks are
	available, including TensorFlow, PyTorch, or Scikit-learn.

## Accessing Neuroimaging Data

Working with neuroimaging data collected by the UK Biobank (UKBB) on
the Research Analysis Platform (RAP) requires ensuring that your
project is correctly configured to access the large bulk data files
associated with imaging and then utilizing the appropriate analysis
environment.

Neuroimaging data—like magnetic resonance imaging (MRI), optical
coherence tomography (OCT) imaging, or cardiac imaging—are considered
**bulk data-fields**. The data dispensing process can take time,
sometimes over an hour or between 20 minutes and 2 hours, depending on
the scope of the application.

### 3. Locating and Accessing Neuroimaging Data Files

Once dispensing is complete, the data structure dictates how you
access the imaging files:

*   **Bulk Data Location:** Bulk data files, including imaging data,
	are placed into the **`/Bulk`** folder within your project.
*   **Folder Structure:** The `/Bulk` folder organizes files using
	subfolders based on the UK Biobank bulk field *category*. For
	instance, **Brain MRI images** will be found in folders within the
	Bulk directory. The folder structure includes categories and
	specific fields, such as `/Bulk/Brain MRI/dMRI`, which contains
	files from fields like Multiband diffusion brain images (DICOM
	\#20218 and NIFTI \#20250).
*   **Raw Data Formats:** Typical raw imaging files you would work
	with include **DICOM** (often containing multiple 2D slices per
	folder, storing data and metadata like image resolution) and
	**NIFTI** (storing 3D voxel data, usually one file per volume,
	which is more compact and often preferable for machine learning
	applications).

### 4. Setting up the Analysis Environment

Depending on your analytical goal (working with pre-computed metrics
versus running custom processing on raw images), you will need
different environments:

| Analysis Goal | RAP Access Method/Environment Setup | Details |
| :--- | :--- | :--- |
| **Working with Pre-computed Metrics** | **Cohort Browser** | You can directly utilize **Image Derived Phenotypes (IDPs)** and other neuroimaging features that have been pre-computed by UKBB. These IDPs are accessible via the Cohort Browser, where you can load, filter, and analyze cohorts by filtering criteria such as volumes of gray matter. |
| **Custom Processing/Statistical Analysis** | **JupyterLab Environments backed by Spark** | Bioinformaticians can create JupyterLab environments backed by Spark clusters to query data and create dataframes in Python or R. From here, you can install and use open-source tools like **FSL** or **FreeSurfer** for neuroimaging analysis. Analysis steps might include DICOM to NIFTI conversion or brain extraction (school stripping) using tools like FSL's `bet` command. |
| **Machine Learning (ML)** | **JupyterLab with GPU Instance** | To conduct machine learning analysis and train models on imaging data faster, you should select a **GPU instance type** for your JupyterLab session, such as `mem3 gpu`. ML frameworks like TensorFlow, PyTorch, or Scikit-learn are available. |
