# Multi-Modal Machine Learning to Uncover Genetic and Immune Pathways Driving WMH and Cognitive Decline

## Background and Rationale
Cognitive impairment is a life-altering condition, and identifying individuals at risk early is critical. This project aims to uncover biological pathways linking genetic variation to white matter hyperintensity (WMH) burden, a key marker of cerebral small vessel disease and a strong predictor of dementia.

Although WMH are common in aging, their progression varies significantly across individuals, suggesting underlying biological drivers. Previous work has shown that individuals with progressive WMH accumulation exhibit distinct leukocyte gene expression profiles, highlighting immune involvement. In parallel, preclinical studies demonstrate that leukocyte adhesion in brain capillaries disrupts microcirculation, providing a mechanistic link between immune activation and small vessel damage.

Using the Ontario Neurodegenerative Disease Research Initiative (ONDRI) dataset, this project investigates the genetic and immune basis of WMH progression and cognitive decline.

## Data and Modalities
This project integrates multi-modal data, including:
- Genomics: NeuroX genotyping (~250,000 SNPs)
- Imaging: 3T MRI (T1-weighted and FLAIR)
- Clinical: Longitudinal cognitive assessments

## Methodology

### 1. WMH Quantification and GWAS
WMH burden will be quantified using automated Bayesian segmentation of MRI data. These imaging-derived phenotypes will be used in genome-wide association studies (GWAS) to identify SNPs associated with WMH burden and progression, adjusting for demographic and vascular covariates.

### 2. Pathway and Gene Mapping
Significant variants will be aggregated into biological pathways to evaluate combined genetic effects. SNPs will be mapped to nearby or functionally relevant genes, with prioritization of immune and vascular pathways.

### 3. Machine Learning Pipeline
A multi-stage machine learning framework will be developed:
- Feature selection: Elastic Net, Gradient Boosting
- Prediction models: XGBoost, interpretable neural networks
- Graph-based modeling: Graph Neural Networks (GNNs), Graph Attention Networks (GATs)
- Interpretation: SHAP values for feature importance

This pipeline will integrate genetic, imaging, and cognitive features to predict WMH progression and cognitive decline.

### 4. Advanced Integration
Graph-based approaches will model SNP-gene-pathway relationships, identifying key biological hubs driving disease progression. Vision-language models may also assist with MRI report annotation and data labeling.

## Timeline
- **Year 1:** Data preprocessing, WMH quantification, GWAS, pathway analysis
- **Year 2:** Machine learning model development, validation, and interpretation

## Risks and Mitigation
The moderate sample size of ONDRI may limit statistical power. To address this:
- WMH will be modeled as a continuous trait
- Dimensionality reduction will be applied
- External datasets may be used for validation

## Expected Outcomes
- Development of a multi-modal risk score for cognitive decline
- Identification of immune-vascular pathways driving WMH progression
- Stratification of individuals based on biological risk profiles
- Discovery of potential therapeutic targets for neurovascular protection

## Significance
This project integrates genomics, imaging, and machine learning to better understand the biological drivers of cognitive decline and supports early detection and precision medicine approaches in neurodegenerative disease.