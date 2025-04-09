# Julia-Katharina Pfarr

:octocat: https://github.com/julia-pfarr

## Background

During my time as a PhD student in the field of Biological Psychiatry, I had the opportunity to develop skills in both psychiatry and in data science. I conducted clinical interviews and neuropsychological testing with psychiatric patients, which helped me to understand the symptom manifestations of the disorders. As the research consortium in which I did my PhD was multi-sited, I was continuously faced with questions regarding imaging and (pre-)processing quality as well as statistical harmonization of the data. This fostered my interest in research on quality assurance of imaging data, methods, and science in general. I further trained in statistical analyses of large multidimensional data and broadened my data analysis skills. The topic of my dissertation was focused on multivariate statistical methods for neuroimaging analyses of psychiatric disorders in large cohorts.
I have also worked as a Data Scientist/Data Steward in another collaborative research consortium, which helped me to understand the need of open, reproducible, and generalizable neuroscience. I was able to gain professional skills in research data management (RDM), research code development, as well as appropriate data storage and sharing practices. I have developed several trainings for RDM, which are openly available through my [GitHub](https://github.com/julia-pfarr), and organized schools (e.g., [Brainhack School](https://school-brainhack.github.io/)) for researchers. Recently, I joined the team of the [BIDS](https://bids.neuroimaging.io/index.html) maintainers. 


## Project: Normative modelling for predicting neuropsychiatric symptoms in Parkinson's Disease

### Overview

Neuropsychiatric signs and symptoms in Parkinson's Disease (PD) are common and can be as disabling and as clinically relevant as motor symptoms. The prevalence and severity of these neuropsychiatric signs and symptoms often increase over time and can be placed in the broad categories of affect, perception and thinking, and motivation. Correlates and risk factors for developing neuropsychiatric signs and symptoms are vast and research shows large interindividual variation. What is more, administering diagnostic tools for neuropsychiatric symptoms is not common clinical practice unless the patient does already present clear signs of neuropsychiatric distress which is often at a point where interventions are less effective. What is (almost) common clinical practice, though, is brain imaging assessments such as MRI scans. 

The main purpose of this project is to find a way to accurately and reliably predict neuropsychiatric symptoms in PD patients using brain imaging data in order 

1. to facilitate diagnostics of neuropsychiatric symptoms in PD for early intervention selection
2. while not adding more burden to the clinicians daily practice. 

### Methods

Normative modeling in neuroimaging establishes a reference distribution of brain measures (e.g., cortical thickness, functional connectivity) across a healthy population while accounting for key covariates like age and sex. This model can then be used to quantify standardized subject-specific deviations in individual patients, identifying abnormal brain patterns linked to disorders. This approach facilitates disentangling the heterogeneity in brain imaging data within patient groups.

The pre-estimated normative model by [Rutherford et al.](https://doi.org/10.7554/eLife.72904), provides a reference cohort of neuroimaging data from 82 sites (N=58,836; ages 2–100) and characterizes lifespan trajectories of cortical thickness and subcortical volume. We will adapt the model to new sites and test this model on PD patients. With this approach we hope to be able to

1. stratify interindividual differences in cortical thickness data between PD patients
2. explain interindividual differences in development and manifestation of neuropsychiatric symptoms
3. and increase the performance of prediction models for neuropsychiatric symptoms in PD patients using the Z-predict scores from the normative model instead of the raw cortical thickness values. 

Important first steps in this project will be the gathering of available PD-brain imaging datasets with available neuropsychiatric assessment data as well as the harmonization of those assessments, since symptoms are measured by different inventories across datasets. For this, we collaborate closely with the [ENIGMA-PD](https://enigma.ini.usc.edu/ongoing/enigma-parkinsons/) Neuropsychiatry project, lead by [Eva van Heese](https://scholar.google.com/citations?user=JhpThRUAAAAJ&hl=en) and [Odile van den Heuvel](https://www.amsterdamumc.org/en/research/researchers/odile-van-den-heuvel.htm). 