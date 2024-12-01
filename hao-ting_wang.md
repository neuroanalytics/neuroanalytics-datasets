# Hao-Ting Wang

[GitHub](https://github.com/htwangtw) | 
[Google Scholar](https://scholar.google.com/citations?hl=en&user=FrlzI8IAAAAJ)

## Background

I am a postdoc researcher of [laboratory for brain simulation and exploration (SIMEXP)](https://github.com/SIMEXP) at CRIUGM, Montreal, Canada. 
My current research focuses on brain connectivity-based biomarker discovery through machine learning. 
I also work on improving data infrastructure and software engineering in neuroimaging research. 
I am a core develop of neuroscience and machine learning software [Nilearn](https://github.com/nilearn/nilearn).
My other contributions includes, but not limited to: [Brainhack School](https://school-brainhack.github.io/) educational content, [NeuroLibre](https://github.com/neurolibre/full-stack-server), [cca-zoo](https://github.com/jameschapman19/cca_zoo/), the [nipy community](https://github.com/nipy) such as [nibabel](https://github.com/nipy/nibabel) and [pydra](https://github.com/nipype/pydra). 

## Project: Predicting presymptomatic features of Alzheimer’s disease with transfer learning in the PREVENT-AD cohort

Recent research suggests significant overlap in the brain changes underlying various neurodegenerative conditions, including Alzheimer's disease, schizophrenia, and Autism Spectrum disorder. 
Traditionally these conditions are studied in isolation, but we believe these conditions can be better investigated together as a broad spectrum. 
This project explores a novel approach to investigate these commonalities by pooling multiple datasets together. 
Once pooled, we aim to showcase how using a model pretrained on  this big datasets and a machine learning technique called transfer learning, we can predict diagnoses within a smaller clinical dataset, namely in the [PREVENT-AD dataset](./prevent-ad.md).
Ultimately, the goal is to discover markers that can predict the development of cognitive decline.

This analysis will focus on identifying "transdiagnostic brain signatures": 
shared features across these conditions. 
Brain scans will be used to extract relevant information. 
A pre-trained [deep learning](./deep-learning.md) model, informed by data encompassing natural variations in brain health across a large population, 
will then be applied to a dataset specifically focused on [Alzheimer](./alzheimers.md)'s development. 
The model architecture chosen leverages knowledge of brain connectivity to understand how these features evolve over time. 
Importantly, it also produces interpretable results, allowing researchers to dissect the model's findings. 

Furthermore, a secondary aim is to develop a user-friendly software framework to help  streamline future research by enabling seamless analysis of brain scans from diverse datasets. 
This bridge between research and clinical application has the potential to improve our understanding and management of neurodegenerative conditions.


<!-- 
A short paragraph describing your project.

Add links to the relevant shared pages where appropriate, like:
[deep learning](./deep-learning.md)

and to refer to a specific section:
[software](./deep-learning.md/#software-and-tools)

If you want to refer to a heading in the same document, just do:
[In my previous experience](#background) -->
