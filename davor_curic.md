# Davor Curic

Github: https://github.com/davorcuricGit

## Background

I finished my undergraduate in Physics in 2016 at University of Calgary where I focused on nonlinear optics and single photon generation. During this time I also had
the opportunity to work at the National Research Council in Ottawa as an undergraduate research. My masters thesis was done at the University of Ottawa under Dr. Jeff Lundeen,
focusing on quantum information science and optics. For my PhD I returned to uCalgary under Dr. Jorn Davidsen where my thesis focused on the critical brain hypothesis in
different physiological settings and was completed in January 2024.

I also am heavily involved in scientific communication, having worked in the communications department of the European Southern Observatory in Munich, as well
as joining the YouTube Partner Program for my science edutainment videos on YouTube.

## Project

[Public description] Neurodegenerative diseases such as Alzheimer’s have a profound and devastating societal impact. Despite this, relatively little is understood about how these diseases progress. One promising recent approach is studying the changes to the functional connectome (FC), a mathematical representation of brain activity obtained non-invasively from routine fMRI scans. Changes in the FC reflect alterations in interactions between brain regions and could serve as biomarkers for disease progression.

To study disease progression one requires multiple scans over time (each corresponding to a single FC), and a notion of 'distance' between FCs. This latter aspect has proven difficult because the mathematical space of FCs is not flat, but rather is non-linearly curved. In the same way that one needs to take into account the curvature of the Earth to properly find the distance between two cities, so too does the curvature of FC space need to be taken into account to find distance between two FCs. Traditional methods of defining distances between FCs (e.g., correlation distances) ignore the geometry of the space, resulting in erroneous distances.

‘Geometry-aware’ approaches have recently emerged that address this problem, defining distances between FCs while taking into account the curvature of the space. My research will utilize these new methods to study the progression of neurodegenerative disease. I hope to address the following: can geometry-aware methods detect neurodegenerative disease earlier? Can we observe acceleration in disease progression? Which brain areas are most affected? If successful, this project could provide a non-invasive approach to studying neurodegenerative disease progression, one which utilizes only existing measurement methods such as fMRI. This would not only provide researchers with a new tool for studying the disease but also offer healthcare providers a new diagnostic tool to enhance patient care.


## Technical description
The primary goal of this project is to develop and apply new analytical methods compatible with non-invasive modalities like fMRI to quantitatively track the progression of neurodegenerative diseases, specifically Alzheimer’s. This is important as many existing methods require invasive surgeries, and can often only be done post-mortem. I aim to utilize recently developed ‘geometry-aware’ methods [1], specifically Tangent Space Analysis (TSA) [2] to study the evolution of functional connectomes (FCs) [3], in order to track Alzheimer’s disease progression. FCs represent the communication strength between brain regions and are derived from time-series data, such as those obtained during routine fMRI. Changes in the FC can give insight into how the disease could be altering this communication. TSA takes into account the manifold on which these FCs exist in order to study how they are ‘moving’ over time, akin to considering the Earth's curvature to find paths between cities, which has not been previously done in FC studies of neurodegenerative disease. As this method focuses on data analysis, it requires no changes in data collection processes on the patients end. If successful, this approach could improve researchers' understanding of how neurodegenerative diseases progress and serve as an additional tool for healthcare providers to inform patient treatment.


<!-- 
Add links to the relevant shared pages where appropriate, like:
[deep learning](./deep-learning.md)

and to refer to a specific section:
[software](./deep-learning.md/#software-and-tools)

If you want to refer to a heading in the same document, just do: -->

## Tangent Space Analysis

Functional connectomes (FCs) are symmetric positive definite (SPD) matrices, meaning they inhabit a curved Riemannian manifold rather than a flat Euclidean space. Standard statistical methods assume flat geometry and are therefore not directly applicable. Tangent Space Analysis (TSA) addresses this by locally "flattening" the manifold: given a reference point $\Sigma_0$ on the manifold, the Riemannian logarithmic map $\text{Log}_{\Sigma_0}(\cdot)$ projects nearby FCs into the tangent space at $\Sigma_0$, which is a flat Euclidean space. Once projected, standard linear methods — regression, PCA, distance metrics — can be applied.

### Choice of reference

The reference $\Sigma_0$ acts as the "origin" of the local linearization. The theoretically optimal choice is the Fréchet mean (Riemannian geometric mean) of the dataset, as it minimizes the average distortion introduced by the projection. In practice, the geometric mean can be simpler to compute and has been observed to perform nearly as well. Depending on the classification task it may also make sense to use a subset of functional connectomes. As an example, in a Alzheimer's disease study, it may be useful to choose only control FCs for the reference with the hope being that this choice will cause control FCs to be closer to the origin, while AD-FCs would be further. That being said, care should be taken that the choice of reference does not cause data-leakage. Alternative choices — such as the identity matrix or a population template — are sometimes used when a stable, session-independent reference is required (e.g., for longitudinal comparisons across cohorts), though they introduce greater projection error for points far from the reference. The sensitivity of downstream analyses to reference choice is therefore an important methodological consideration.

### Regularization

SPD matrices obtained from short fMRI recordings can be poorly conditioned or near-singular, since the number of time points may be small relative to the number of brain regions. The criteria for non-singular FCs is that the number of time points must be at least equal to the number of variables. As an example, suppose a fMRI BOLD signal has 500 time points. If a Schaefer-100 parcellation is used, the resulting FCs will be non-singular (i.e., invertable). If a Schaefer-1000 parcellation is used, the resulting FCs will be singular (i.e., non-invertable). Near-singular matrices are also  numerically unstable and lie close to the boundary of the SPD manifold, making geodesic computations unreliable. Regularization — typically ridge-type shrinkage, which adds a small multiple of the identity $\Sigma \leftarrow \Sigma + \lambda I$ — pulls matrices away from the boundary and improves numerical stability. The regularization parameter $\lambda$ introduces a bias-variance tradeoff: too little regularization leaves computations unstable, while too much shrinks all FCs toward the identity and erases individual differences. In longitudinal disease studies this choice is particularly consequential, as over-regularization can obscure the subtle FC changes that characterize early disease progression.