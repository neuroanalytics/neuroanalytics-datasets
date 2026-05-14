# Semantic Embedding for Mapping Clinical Metrics

Large Language Models (LLMs) use semantic embeddings to arrange and
group topics based on their similarity by translating text into
numerical vectors and measuring the proximity of these vectors in a
high-dimensional space.

## How does it work?

### The Role of Embeddings and Vectors

1.  **Encoding Semantic Meaning:** The fundamental mechanism involves
	**Embedding**, which is a high dimensional encoding of tokens
	(words or pieces of words) that represents their **semantic
	meaning**. A transformer model, which is the core invention
	underlying current AI, embeds (or encodes) data into this
	high-dimensional space.
2.  **Representing Similarity:** In this high-dimensional space, the
	semantic meaning is encoded such that words or phrases with
	**similar meanings tend to land on vectors that are close to each
	other**.
3.  **Measuring Distance:** To determine how related topics are, the
	distance or alignment between their corresponding vectors is
	measured. The principle is that the **further apart phrases or
	topics are in the trained semantic space, the less related they
	are**.

### Measuring Similarity and Grouping Topics

The process of grouping topics based on similarity relies on
calculating the distance or alignment between these semantic vectors:

*   **Semantic Distance:** The similarity between topics is assessed
	by identifying relevant information based on the **semantic
	distance** between a query (or prompt) and the encoded information
	stored in a database.
*   **Cosine Scores:** **Cosine Scores** are a common measure used in
	text analysis to assess similarity between two vectors. **Semantic
	Similarity** evaluates the closeness of meaning between texts
	using this cosine index.
*   **Dot Product:** Geometrically, the dot product of two vectors is
	helpful because it measures **how well they
	align**. Computationally, dot products rely on weighted sums,
	which is a key component of LLM calculations.
*   **Vector Similarity Methods:** Generally, **vector similarity
	methods** are utilized to analyze textual data effectively,
	enabling the retrieval of closely related content.

### Application in Retrieval-Augmented Generation (RAG)

In practical LLM applications, especially those requiring access to
domain-specific or updated external knowledge, semantic embeddings are
central to Retrieval-Augmented Generation (RAG).

*   **Indexing and Storage:** In RAG, documents or data meant to be
	referenced are converted into embeddings—numerical representations
	in a large vector space—and then stored in a **vector database**.
*   **Retrieval:** When a user query is input, it is also converted
	into an embedding. This query embedding is then compared against
	the document embeddings in the vector database to identify the
	most **relevant documents** based on semantic distance and vector
	similarity.
*   **Context Generation:** The relevant, semantically similar
	information retrieved from the database is then fed back into the
	LLM, augmenting the original query so the LLM can generate an
	evidence-based response.
*   **Topic Complexity:** Within the LLM itself, the layers use
	weights to combine tokens together into **more complex topics**
	during processing.

By leveraging semantic embeddings and calculating vector proximity,
LLMs and associated systems can effectively cluster, retrieve, and
synthesize information based on conceptual similarity, rather than
just keyword matching.

## Clinical Applications

The utility of LLM embeddings in this domain stems from their ability
to quantify the semantic relationships between textual data, which is
crucial for identifying patterns that may correlate with disease
progression or characteristics.

### Direct Applications in Disease Context

A referenced paper highlights the explicit potential of this
technology in neurological disorders: the concept of leveraging LLM
embeddings is being explored for **"Revolutionizing Semantic Data
Harmonization in Alzheimer's and Parkinson's Disease"**. Semantic data
harmonization is a necessary step for comparing and analyzing complex
textual data (like clinical assessment responses) across different
studies or time points, which is fundamental to modeling disease
progression.

### Mechanism: Semantic Similarity and Vector Methods

The core function relies on translating the complex natural language
responses from cognitive or clinical assessments into high-dimensional
numerical representations:

1.  **Embedding Meaning:** An LLM utilizes **Embedding**, which is a
	high dimensional encoding of tokens (words or pieces of words)
	that represents their **semantic meaning**.
2.  **Quantifying Similarity:** In this embedding space, words or
	phrases with similar meanings are represented by vectors that are
	located close to each other. The similarity between two textual
	responses (or assessment sections) is quantified using **vector
	similarity methods**.
3.  **Metrics for Comparison:** Specific metrics used to evaluate the
	closeness of meaning (**Semantic Similarity**) between texts
	include **Cosine Scores** (which measure the similarity between
	two vectors using the cosine index).
4.  **Harmonizing Assessments:** This ability to measure semantic
	similarity has been demonstrated in facilitating the
	**harmonization of mental health questionnaires** through Natural
	Language Processing (NLP). Applying this technique to
	cognitive/clinical assessments means that even if responses are
	phrased differently, they can be objectively grouped if they share
	the same underlying meaning (semantic similarity).

### Utility for Modeling Progression

By leveraging semantic embeddings, researchers can potentially model
disease progression in the following ways:

*   **Pattern Recognition in Clinical Notes:** LLMs can extract
	concepts and aggregate information from medical text records,
	including unstructured **clinical notes**. Semantically linking
	responses gathered over time can highlight consistent themes or
	changes in cognitive performance or clinical state, providing
	vectors indicative of disease trajectory.
*   **Specialized Models:** LLMs can be **fine-tuned** on specialized
	biomedical datasets to enhance accuracy and reduce issues like
	hallucinations. **Fine-tuned LLMs** on medical corpora continue to
	advance evidence-based medicine, suggesting they can be
	specialized enough to understand the nuances of disease-specific
	language in assessments.
*   **Augmentation and Reasoning:** Techniques like integrating
	**biomedical knowledge graphs** enhance the reasoning capabilities
	of LLMs, structuring medical knowledge into interconnected
	entities. This structured knowledge, combined with semantic
	vectors from assessments, could provide a richer basis for
	tracking and predicting disease states.

## Using Compute Canada / DRAC to Embed data

Running a Large Language Model (LLM) to embed a collection of text for
later retrieval involves significant machine learning computation and
data handling, presenting several challenges related to data
management, resource usage, and job scheduling within a
High-Performance Computing (HPC) cluster environment.

Here are the key challenges associated with performing large-scale LLM
embedding tasks:

### 1. Data Management and I/O Performance

LLM embedding typically requires reading a large text collection,
which creates substantial I/O demands. The distributed filesystem
architecture on HPC clusters introduces specific performance
bottlenecks:

*   **Slowdown from Small Files:** If the collection of text consists
	of **datasets containing lots of small files** (hundreds of
	thousands or more), your software could be **significantly slowed
	down** by streaming these files from shared storage like
	`/project` or `/scratch` to a compute node. On a distributed
	filesystem, files should ideally be stored in **large single-file
	archives**.
*   **Filesystem Quotas:** Large collections of small files may
	violate **filesystem quotas** imposed on clusters, which limit the
	maximum number of filesystem objects a user can possess.
*   **Shared Storage Limitations:** Shared storage locations (like
	`home`, `project`, or `scratch`) are intended for **storing and
	reading at low frequencies** (e.g., reading one large chunk every
	10 seconds, rather than 10 small chunks every second). Frequent
	reads necessary for an embedding task can stress these systems.
*   **Choosing the Right Storage:** Accessing a file on shared storage
	like `/project` has **very different performance implications**
	compared to accessing data locally on the compute node.
	*   For datasets up to **$100 \text{ GB}$ or less**, the optimal
		practice is to **transfer the data to the local storage** of
		the compute node (available at `$SLURM\_TMPDIR`) at the start
		of the job, as this storage is **orders of magnitude faster
		and more reliable** than shared storage.
	*   If the dataset is larger, it must be left in shared storage.

### 2. Computational Scale and Job Scheduling

LLMs require substantial resources, often including GPUs, and the
embedding process may be lengthy or involve many iterations, leading
to scheduling complexity:

*   **Long Running Computations:** If the process of embedding the
	text collection is **long** (e.g., expected to take many days),
	you **should use checkpointing**. For instance, a three-day
	computation should be split into smaller chunks (e.g., $24 \text{
	hours}$). This practice helps prevent the loss of work in case of
	an outage and provides an **edge in terms of job priority**
	because more nodes are typically available for shorter jobs.
*   **Time Limits and Priority:** General-purpose clusters like Fir,
	Narval, Nibi, and Rorqual have job time limits of up to **7 days**
	(168 hours). Shorter jobs (e.g., $3 \text{ hours}$ or less) have
	**more scheduling opportunities** than longer jobs and benefit
	from the backfilling mechanism employed by the scheduler.
*   **Inefficient Job Submission:** If the LLM embedding requires
	processing the text collection in many small, similar parts,
	submitting each part as a separate job is inefficient. It is
	recommended to **group many similar jobs into one** using tools
	like META, GLOST, or GNU Parallel.
*   **GPU Resource Accounting:** When requesting resources for the LLM
	job, the calculation of future job priority is based on the
	resources requested, not the resources actually used. GPU
	resources are tracked using **Reference GPU Units (RGUs)**.
	*   Users are charged for the **maximum number of RGU-core-memory
		bundles they request**. If you request more CPU cores or
		memory than the job uses in relation to the GPU resources, the
		scheduler will count the usage based on the largest
		requirement (RGU, cores, or memory), potentially leading to
		**over-charging** and lowering the priority of future jobs for
		your research group.

### 3. Software Environment Challenges

Setting up the necessary environment for LLMs (which rely heavily on
Python and specialized libraries) requires following specific HPC
directives:

*   **Anaconda Restriction:** Users are strongly advised to **avoid
	using Anaconda** and use `virtualenv` instead for managing Python
	environments. When switching to `virtualenv`, users should install
	all necessary packages, excluding low-level libraries like CUDA
	and CuDNN, which are **already installed on the clusters**.
*   **Dependency Management:** If using specialized machine learning
	packages like TensorFlow or PyTorch (which are relevant for LLMs),
	users must refer to the documentation for installation, common
	pitfalls, and ensuring compatibility.
*   **Non-Deterministic Behavior:** If using Recurrent Neural Networks
	(RNNs) or multi-head attention APIs within the LLM architecture
	with cuDNN built with CUDA Toolkit 10.2 or higher, there may be
	**non-deterministic behavior** unless the
	`CUBLAS_WORKSPACE_CONFIG` environmental variable is set to specify
	a single buffer size.
