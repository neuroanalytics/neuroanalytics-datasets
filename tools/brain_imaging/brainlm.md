# BrainLM - Foundation Model of Neural Data

The Brain Language Model (**BrainLM**) captures neural function by
acting as a **foundation model for brain activity dynamics** trained
on a massive corpus of fMRI recordings. It utilizes a
Transformer-based architecture and a self-supervised training paradigm
to learn robust, generalizable representations of complex neural
patterns, which are then used for prediction and interpretation.

## What it does

BrainLM's ability to capture neural function can be broken down into
its foundational mechanism and its demonstrated capabilities:

### 1. Foundational Mechanism: Transformer-Based Masked Autoencoder

BrainLM's architecture is a **Transformer-based masked autoencoder**
adapted from natural language processing (NLP) models like BERT.

*   **Data Preparation:** The model is trained on 6,700 hours of fMRI
	activity patterns from 77,298 recordings derived from datasets
	like the UK Biobank (UKB) and Human Connectome Project (HCP). The
	raw fMRI data is preprocessed and parcellated into 424 regions,
	yielding 424-dimensional scan sequences.
*   **Self-Supervised Pretraining:** BrainLM uses a **self-supervised
	masked-prediction training** objective. Segments of parcel time
	series are embedded, and a portion of these segments is **masked**
	(e.g., 20%, 50%, or 75% masking ratios).
*   **Learning Spatiotemporal Dynamics:** The encoder processes the
	visible (unmasked) input patches, and the decoder attempts to
	**reconstruct the original patches**, including the masked
	segments. By minimizing the reconstruction error, the model learns
	the inherent complex **spatiotemporal dynamics** and dependencies
	*within and across* brain activity sequences.

### 2. Capabilities Demonstrating Neural Function Capture

The model's learned representations allow it to perform various tasks
related to brain function, both through fine-tuning and zero-shot
inference.

#### 2.A Prediction of Functional Networks and Topology

BrainLM demonstrates the ability to extract fundamental organizational
principles of the brain, even without explicit network-based
supervision during training.

*   **Functional Network Identification:** BrainLM can segment parcels
	into intrinsic functional brain networks, clustering them into
	known systems such as visual, somatomotor, dorsal attention,
	ventral attention, limbic, frontoparietal, and default mode
	networks. A $k$-NN classifier applied to **BrainLM's
	self-attention maps** achieved the highest accuracy for parcel
	classification into these 7 regions, confirming that the
	self-attention maps hold meaningful information about network
	identity.
*   **Interpretability via Attention Analysis:** By visualizing the
	self-attention weights assigned to each parcel by the CLS token,
	BrainLM provides useful interpretability for neuroscientific
	insights. This revealed:
	*   **Functional Contrasts:** Task recordings exhibited **greater
		visual cortex attention** compared to resting state,
		consistent with the visual stimuli used during the task.
	*   **Clinical Relevance:** Differences were observed in attention
		patterns between mild and severe depression (PHQ9 scores),
		with more weighting found on **frontal and limbic areas** for
		severe depression.

#### 2.B Modeling and Predicting Dynamic Brain States

The model's ability to capture temporal dependencies allows for
forecasting future brain activity.

*   **Future Brain State Prediction:** BrainLM can extrapolate to
	**future brain states** by forecasting parcel activities at
	subsequent timepoints, showing how pretraining enables it to
	effectively learn fMRI dynamics. The fine-tuned BrainLM model
	significantly outperformed baseline approaches like LSTMs and
	ODEnets in next timestep prediction on both UKB and HCP test sets.

#### 2.C In Silico Perturbation Simulation

BrainLM can be leveraged as an *in silico* simulator, offering new
opportunities for computational modeling and causal discovery in a
completely computational manner.

*   **Perturbation Analysis:** A novel **in silico perturbation
	prompting approach** is used to simulate the effect of
	perturbations (input modifications) on neural activity patterns in
	a **zero-shot** manner (i.e., the model was never trained on
	perturbations).
*   **Revealing Functional Relationships:** By optimizing a
	perturbation function, $G$, to make an initial recording
	($X_{\text{Input}}$) match a target cognitive condition ($X_T$),
	the analysis identifies which fMRI features are most influential
	in altering the predicted brain state.
*   **Biologically Meaningful Responses:** When optimizing a
	resting-state recording to match a task-based recording, the
	perturbation focused on the **visual cortex**, aligning with the
	expected functional changes required for visual processing.

#### 2.D Clinical Biomarker Discovery

The learned representations are robust enough to be used as powerful
biomarkers for decoding cognitive health and disorders.

*   **Clinical Variable Prediction:** BrainLM can be fine-tuned to
	predict clinical variables obtained from metadata, such as **age,
	neuroticism, PTSD, and anxiety disorder scores**. BrainLM
	significantly outperformed SVM baselines trained either on raw
	fMRI data or on extracted pretrained embeddings, validating its
	ability to uncover predictive signals within complex fMRI
	recordings.

## Potential Challenges

A user working with BrainLM might face challenges related to the
underlying **complexity and limitations of the fMRI data**, the
**specificity required for preprocessing and fine-tuning**, and the
**current scope of the foundation model**.

### 1. Challenges Inherited from fMRI Data

BrainLM operates on fMRI (functional Magnetic Resonance Imaging) data,
which presents intrinsic difficulties:

*   **Indirect and Complex Measurement:** The BOLD (blood oxygen level
	dependent) signals measured by fMRI are an **indirect measure of
	brain function** and can be **difficult to interpret**.
*   **High Dimensionality and Dynamics:** fMRI data is **massive,
	high-dimensional**, and exhibits **complex spatiotemporal
	dynamics**, with critical dependencies across both space and
	time. Modeling these complex nonlinear interactions poses major
	challenges.
*   **Required Preprocessing Reliance:** BrainLM relies on extensive
	preprocessing of the raw fMRI data, and a user attempting to
	reproduce or apply the model to new data must match these specific
	steps, which include:
	*   **Motion correction, normalization, temporal filtering, and
		ICA denoising**.
	*   **Parcellation** of the brain into **424 regions** using the
		AAL-424 atlas.
	*   **Robust scaling** applied by subtracting the median and
		dividing by the interquartile range for each parcel.

### 2. Specificity in Training and Fine-Tuning

When utilizing or adapting the foundation model, a user must adhere to
specific architectural and data handling choices:

*   **Adhering to Training Ratios and Segments:** The initial
	self-supervised pretraining was performed using specific
	parameters:
	*   The model processed **200-timestep subsequences**.
	*   The parcel time series were divided into segments of **20
		timesteps**.
	*   A subset of these segments was **masked** using ratios of
		**20%, 50%, or 75%**.
*   **Precise Normalization for Clinical Tasks:** Fine-tuning BrainLM
	to predict specific clinical variables requires specific data
	transformations:
	*   Age values must be normalized by **Z-scoring**.
	*   Neuroticism scores are subjected to **min-max scaling**.
	*   Post Traumatic Stress Disorder (PCL) and General Anxiety
		Disorder (GAD7) scores require a **log transformation** first,
		to make the values less exponentially distributed, followed by
		**min-max scaling**.

### 3. Limitations in Current Scope (Areas for Future Work)

The published capabilities of BrainLM, while significant, suggest
limitations in scope that a user may wish to overcome:

*   **Unimodal Dependency:** BrainLM is currently a foundation model
	for **fMRI recordings**. A major area for future work, and
	therefore a current limitation for users, is the need for
	**multi-modal training** to integrate fMRI with additional
	recording modalities, such as EEG and MEG, or different brain-wise
	information like structural, functional, and genomic data.
*   **Limited Network Atlas Assessment:** BrainLM demonstrated
	functional network prediction by clustering parcels into **7
	functional groups** (visual, somatomotor, dorsal attention,
	ventral attention, limbic, frontoparietal, and default mode
	networks). Future work is needed to assess zero-shot
	classification on **expanded functional atlases beyond the 7
	networks used**.
