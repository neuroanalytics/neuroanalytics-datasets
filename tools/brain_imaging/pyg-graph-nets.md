# Graph Neural Networks with PyG

## Graph Machine Learning

Machine learning (ML) on a graph fundamentally differs from ML on
two-dimensional data (such as images or structured tables) primarily
due to the **irregular structure** and **relational nature** of graph
data, which necessitates specialized models and data representations.

### The shape of the data matters

FreeSurfer 8 will be transitioning many of it's core algorithms to
convolutional neural nets (CNNs), but not necessarily graph
networks. New tools like `SynthMorph` and `SynthSeg` are not based on
graph machine learning methods. Instead, they rely on widely adopted
CNN architectures common in medical image analysis.

This is not meant to diminish the power or utility of using Graph ML
models, but only to point out that they need to be used
deliberately. Just because data can be characterized as a graph does
not mean the added complexity and assumptions of the model will
benefit the analysis.

To further use the new FreeSurfer 8 functions as an example,

#### SynthMorph (Joint Registration)
SynthMorph is a deep learning (DL) tool developed for fast, symmetric,
diffeomorphic end-to-end affine and deformable brain registration .
1. Overall Approach: SynthMorph uses DL methods that learn a function
mapping an image pair to an output transform . The general framework
employs convolutional neural networks (CNNs).
2. Affine Component: The affine model hθ​ implemented in SynthMorph
 uses a modified Detector architecture. This architecture relies on a
 series of convolutions to predict spatial feature maps. These feature
 maps are used to calculate corresponding moving and fixed point
 clouds, and a weighted least-squares (WLS) solution then provides the
 affine transform. Alternative affine architectures considered in the
 related work include the Encoder, which uses a convolutional encoder
 combined with a fully connected (FC) layer, and Decomposer, a fully
 convolutional network. Some affine DL strategies utilize vision
 transformers instead of convolutional layers, but SynthMorph
 ultimately favors the Detector architecture.
3. Deformable Component: The deformable module uses a U-Net
 architecture (a convolutional neural network) for predicting a
 stationary velocity field (SVF). The model also employs a
 hypernetwork Γξ​, which is described as a simple feed-forward network
 with four ReLU-activated hidden FC layers, used to parameterize the
 weights of the deformable task network gη​ based on a regularization
 weight λ.

#### SynthSeg (Segmentation)
SynthSeg is a convolutional neural network (CNN) designed for the
segmentation of brain MRI scans of any contrast and resolution without
requiring retraining or fine-tuning .
1. Architecture: The core segmentation network used in SynthSeg (and
related variant SynthSeg+) is based on a 3D UNet architecture .
2. Components: The UNet comprises five levels, with operations
including two convolutions per level, batch normalization, max-pooling
(contracting path), upsampling (expanding path), and skip
connections. The modules used in the robust variant, SynthSeg+ (such
as segmenters S1, S2, S3, and the denoiser D), are also implemented as
CNNs.

The descriptions of both SynthMorph and SynthSeg emphasize
architectures based on standard convolutional networks (U-Nets,
encoders, detectors) for image processing, not methods explicitly
utilizing graph structures or Graph Neural Networks (GNNs).

Here is a breakdown of the differences based on data characteristics
and the resulting ML pipeline requirements:

### 1. Data Structure and Relationship

The core distinction lies in the organization of the input data:

*   **Two-Dimensional Data:** Data like images or certain structured
	data often possesses a **regular structure** (e.g., a grid for
	images or a sequence for time series). Traditional deep learning
	models like Convolutional Neural Networks (CNNs) are specialized
	for computer vision applications dealing with these images,
	leveraging inherent **spatial locality**.
*   **Graph Data:** Graphs are a general language for describing
	entities and their **relations and interactions**. Graph data is
	characterized by its **irregular structure**. The central problem
	in ML on graphs is finding the right way to incorporate the
	**information about the graph structure**—the relational
	structure—into the machine learning model.
*   **Data Storage:** Graph data requires specific formats to store
	this relational information, such as:
	*   A matrix containing the **features of every node**.
	*   A representation of the **graph connectivity**, often called
		the `edge_index`. This matrix specifies the source and
		destination of every edge.
	*   Optional **edge features** (e.g., weights or attributes).
	*   The graph data can also be complex, often being massive,
		messy, constantly changing, and sometimes **heterogeneous**
		(having more than one node type or more than one edge type) or
		**temporal** (changing over time) in real-world applications.

### 2. Traditional ML Feature Engineering

In traditional ML pipelines for graphs, significant effort must be
placed on creating features that capture the topological context, a
step often less critical or handled differently for standard tabular
or image data:

*   **Feature Focus:** Traditional ML pipelines on graphs require two
	steps: first, representing the data points (nodes, links, or
	entire graphs) with vectors of features, and second, training a
	classical ML classifier on those vectors. The predictive
	performance depends heavily on creating **structural features**
	that describe how a particular node is positioned in the network
	and what its local network structure is.
*   **Handcrafted Features:** This process traditionally relies on
	**handcrafted features** to capture the relational structure of
	the network. Examples of structural features for nodes include:
	*   **Importance-based features:** Node degree or centrality
		measures (like eigenvector centrality, betweenness centrality,
		or closeness centrality).
	*   **Structure-based features:** Clustering coefficients
		(measuring connectivity among neighbors) or **graphlet degree
		vectors** (counting the number of times a pre-specified
		subgraph, or graphlet, appears rooted at a given node).

### 3. Deep Learning Approaches (GNNs vs. CNNs)

Graph Neural Networks (GNNs) are designed to handle the irregular
structure of graphs and overcome the need for non-trivial human
feature engineering.

*   **Core Operation:** The fundamental operation in GNNs, graph
	convolutions, is known as **message passing**. Message passing
	involves aggregating the features of a node's neighbors to update
	that node's representation. This process takes node features (X)
	and the connectivity matrix (`edge_index`) as inputs.
*   **Feature Aggregation:** GNN layers aggregate the features of
	neighbors using methods ranging from fixed weights to attention
	mechanisms. For tasks like classifying an entire graph (e.g.,
	classifying a protein structure), a **pooling function** (such as
	Global add pool) is used to sum up the information from all nodes
	in the graph.
*   **Model Input and Processing:** Unlike CNNs processing 2D grid
	data, GNNs must handle the complex and sparse nature of graph
	connections. Frameworks like PyTorch Geometric manage this by
	using sophisticated tensor structures (like the `edge_index`
	tensor) and optimized operations (like sparse matrix
	multiplications, or SPMs) designed specifically for data with lots
	of zeros, which is typical for graphs.

## Wait, what's a graph?
The basic features of a graph, how data is mapped to them, and the
resulting advantages stem from the graph's ability to natively
represent entities and their complex, irregular relationships.

### 1. Basic Features of a Graph Structure

A graph is a general language for describing **entities**, their
**relations**, and **interactions**. When storing graph data for
machine learning, the structure requires specific components:

**Core Data Components (PyTorch Geometric Context):**
1.  **Node Features ($X$ or $data.x$):** A matrix containing the
	features or attributes of every node. These features must
	generally be of a numerical type (float, double, or integer).
2.  **Graph Connectivity (`edge_index`):** This special format defines
	the connections (edges) between nodes. It is typically a matrix of
	two rows, where the first row writes the **source** of every edge
	and the second row writes the **destination**.
3.  **Edge Features (`edge_attribute`):** Optional features that
	describe the edges themselves, such as weights or multiple
	attributes.
4.  **Target ($Y$ or $data.y$):** The target values used for training,
	which can be flexible depending on whether the task involves
	classification for every node, every edge, or the entire graph.

**Structural Features (Derived Properties):** In traditional machine
learning on graphs, good predictive performance relies on engineering
**structural features** that describe the network topology. These
features capture how a node is positioned and what its local structure
is:

*   **Importance-Based Features:** Capture the influence or position
	of a node in the graph:
	*   **Node Degree:** The number of edges connected to a node,
		which is a useful but simple feature.
	*   **Node Centrality Measures:** Characterize the importance of a
		node, including **eigenvector centrality** (importance based
		on the importance of neighbors), **betweenness centrality**
		(importance as an important connector lying on many shortest
		paths), and **closeness centrality** (importance based on
		having small shortest path lengths to all other nodes).
*   **Structure-Based Features:** Capture the local neighborhood
	topology:
	*   **Clustering Coefficient:** Measures how connected a node's
		neighbors (friends) are. It basically counts the number of
		**triangles** in the node's immediate network.
	*   **Graphlet Degree Vector (GDV):** A generalization of triangle
		counting that counts the number of times a pre-specified,
		rooted, connected, non-isomorphic subgraph (a **graphlet**)
		appears rooted at a given node. The GDV provides a
		fine-grained measure of local topological similarity between
		nodes.

### 2. How Tabular Data Can Be Mapped to Graph Properties

Tabular or relational data can be mapped to graph features by
identifying entities as nodes and relationships as edges.

**Example: Social Network Data Mapping:** If tabular data describes
people in a social network, the mapping could be structured as
follows:

| Tabular Data Component | Graph Property | Example Content |
| :--- | :--- | :--- |
| Individual Entity (Person) | **Node** | Person 1, Person 2, Person 3, Person 4 |
| Individual Attributes | **Node Features ($X$)** | Age (e.g., 42 years old) and Income Level (e.g., 1200 currency). |
| Relationship/Interaction | **Edge ($\text{edge\_index}$)** | An edge exists between two people if they know each other. |
| Relationship Strength | **Edge Attributes** | The weight of the edge could be the time (e.g., years) that both people have met each other. |
| Prediction Target | **Target Label ($Y$)** | The hours that person is working now. |

**Example: Relational Databases (RDL):** More complex structured data,
like an entire SQL database, can be treated as a graph for
**Relational Deep Learning (RDL)**. In this scenario:

*   **Entities** (e.g., database records) become **nodes**.
*   **Primary/foreign key links** become **edges**.
*   The nodes can handle multimodal data, including numbers,
	categories, and free text.

### 3. What is Gained by Performing this Mapping

The primary gain of mapping data onto a graph structure is the ability
to incorporate and leverage the **relational structure** inherent in
the data, moving beyond traditional models that assume data regularity
or independence.

**Key Advantages:**

1.  **Capturing Relational Structure:** The central challenge and
	benefit of ML on graphs is explicitly incorporating the
	**information about the graph structure** (the relational
	structure) into the model. This relational structure is crucial
	for obtaining **good predictive performance**.
2.  **Addressing Irregularity:** Graphs serve as a powerful
	representation because they can model interactions and relations
	that do not fit into the regular structures assumed by traditional
	deep learning (like the grid structures used for images by CNNs).
3.  **Advanced Representation Learning:** Graph Neural Networks (GNNs)
	utilize the graph structure to learn effective representations for
	nodes, links, or entire graphs. This process, often involving
	**message passing** (aggregating neighbor features), overcomes the
	need for complex, **hand-designed features** that were required in
	older, traditional ML pipelines on graphs.
4.  **Enabling Relational Deep Learning (RDL):** Mapping relational
	data to a graph allows for modern deep learning models (GNNs) to
	learn directly on raw relational databases, enabling breakthroughs
	in practical domains.
5.  **Augmenting Language Models (Graph RAG):** Graphs can be used in
	Retrieval Augmented Generation (RAG) pipelines for Large Language
	Models (LLMs) by providing crucial relational and **topological
	information**. Using GNNs to encode subgraphs and feed this
	structural context to an LLM can significantly enhance accuracy
	(in one scenario, leading to a 2x increase in accuracy compared to
	the LLM alone).
6.  **Predicting Links and Roles:** Graph features are essential for
	specific relational tasks, such as predicting new links between
	nodes (link prediction) or predicting a particular node's role in
	the network (e.g., predicting protein function) using
	structure-based features like Graphlet Degree Vectors.

## Mapping Brain Data With a Graph Network

### Strucutral MRI

Modeling structural brain data, such as a 3D volumetric MRI, with a
graph learning approach involves **transforming the regular, spatial
data structure into an irregular, relational structure** that captures
the entities (brain regions) and their interactions (connectivity).

Graphs provide a general language for describing **entities,
relations, and interactions**. Since the fundamental challenge in
machine learning (ML) on graphs is finding the right way to
incorporate the **relational structure**, mapping brain data to a
graph explicitly enables the ML model to utilize these connectivity
patterns.

Here is how structural brain data could be mapped onto the basic
features of a graph:

#### 1. Defining Nodes (Entities)

The entities in a structural brain graph would be the different
functional or anatomical regions of the brain.

*   **Mapping:** Each distinct region of the brain (often defined by
	an atlas or parcellation scheme) becomes a **node**.
*   **Context:** Graph nodes represent the basic data points of
	interest.

#### 2. Defining Node Features ($X$)

Node features are numerical attributes attached to each entity. For
structural MRI data, these features would be the metrics describing
the structural properties of the defined brain region.

*   **Mapping:** The node features ($X$ or $data.x$) would be a vector
	of metrics extracted from the 3D MRI volume for that specific
	region.
	*   These features could include structural measures such as
		volume, cortical thickness, gray matter density, or even
		complex statistics of tissue intensity within that region.
*   **Requirement:** Only features of a numerical type (float, double,
	or integer) are allowed in the feature tensor.

#### 3. Defining Edges and Connectivity (`edge_index`)

The edges describe the relationships or connections between the brain
regions. In neuroscience, these often represent structural
connections.

*   **Mapping:** An **edge** would be placed between two nodes
	(regions) if a meaningful connection exists.
	*   If the structural data includes information about white matter
		tracts (e.g., from Diffusion Tensor Imaging, which is often
		used alongside MRI), the edges would represent the presence
		and characteristics of these physical tracts connecting the
		regions.
	*   The **graph connectivity** is stored in a special format
		called `edge_index`, typically a matrix of two rows specifying
		the source and destination of every edge.
*   **Edge Attributes:** Optional **edge features (`edge_attribute`)**
	could be used to quantify the connection, such as the estimated
	strength, length, or fiber count of the white matter tract between
	the two regions.

#### 4. Defining the Prediction Target ($Y$)

The target ($Y$ or $data.y$) depends on the specific machine learning
task.

*   **Graph-Level Task:** Predicting a property of the whole brain
	structure. For example, using the resulting structural brain graph
	to classify an entire subject as belonging to a clinical group
	(e.g., diseased vs. healthy).
*   **Node-Level Task:** Predicting a property of an individual brain
	region. For example, predicting a particular region's functional
	role or classifying it based on localized pathology.

#### Gains of the Graph Mapping Approach

While 3D volumetric MRI data inherently has a **regular structure**
similar to 2D image data (which is typically handled by Convolutional
Neural Networks, or CNNs), modeling it as a graph provides critical
advantages for biomedical analysis:

1.  **Capturing Relational Structure:** The most significant gain is
	explicitly incorporating the **information about the graph
	structure**—the complex relational structure of brain
	connectivity—into the model. Traditional models designed for
	regular grids struggle to natively encode these non-local, sparse,
	and intricate topological relationships.
2.  **Specialized Deep Learning:** The graph representation enables
	the use of specialized models like Graph Neural Networks (GNNs),
	which use **message passing** (aggregating features from connected
	neighbors) to learn representations that naturally capture both
	the attributes of a region and its relational context within the
	network.
3.  **Handling Complexity:** The approach aligns with applications in
	other complex scientific domains, such as mapping out **complex
	molecular structures** and **protein-protein interaction
	networks** in biomedicine. The structure of the brain network
	itself is the key to obtaining **good predictive performance** in
	these relational learning tasks.

### Working with functional (MRI, EEG / MEG, etc) data as a graph

fMRI and EEG are more similar to **time series data** or **temporal
data**, and can be modeled using specialized graph networks and the
supporting frameworks.

Graph networks serve as a general language for analyzing entities and
the relationships and interactions between them. When dealing with
data that changes over time, like time series, the structure can be
captured using **temporal graphs**.

Here is how time-varying data can be conceptualized and managed within
a graph network framework:

### 1. Modeling Dynamic Relationships

Modeling time series data requires capturing the evolving structure of
the relationships, which can be done through **temporal graphs**
(graphs that change over time).

*   **Necessity for Temporal Support:** Real-world data is often
	dynamic, and networks, such as transaction networks or social
	networks, frequently evolve over time.
*   **Framework Support:** Frameworks like PyTorch Geometric (PyG) 2.0
	have introduced robust native support for **temporal graphs** to
	conquer the complexities of real-world graph data.

### 2. Handling Temporal Information and Sampling

To process a time series dataset modeled as a temporal graph,
techniques focusing on preserving the time order are crucial:

*   **Temporal Subgraph Sampling:** PyG 2.0 provides support for
	**temporal subgraph sampling** for both homogeneous and
	heterogeneous graphs.
*   **Respecting Constraints:** This technique allows the traversal of
	dynamic graphs over time, extracting snapshots that must strictly
	respect **temporal constraints**. This is essential to prevent
	**future information leakage** because you only see data available
	up to a specific timestamp.
*   **Querying Historical Data:** The design includes supporting the
	querying of **historical subgraphs**, where samplers can iterate
	over external seed nodes and timestamps while always respecting
	the temporal order.
*   **Sampling Strategies:** Various temporal sampling strategies are
	offered, including uniform, most recent K elements, or
	annealing-based strategies.

### 3. Graph Structure and Feature Assignment

When designing the graph network for time series data, the entities
(like sensors or measurement points) and their interactions would be
defined:

*   **Nodes and Features:** Entities become **nodes**. Each node can
	have features attached to it via a **tensor assignment**. For
	example, a matrix containing the features of every node is stored
	as `X`.
*   **Edges and Connectivity:** The relationships or correlations
	between these entities form the connections (links). The graph
	connectivity is stored in a special format called `Edge\_index`,
	which is a matrix of two rows (source and destination).
*   **Edge Features (Weights):** Edge attributes (features) can also
	be stored, such as edge weights or multiple weights for every
	edge, often in the `edge\_attribute` matrix.
*   **Prediction Tasks Over Time:** Modeling over time is often done
	for **link prediction** tasks, where the objective is to predict
	new links based on the existing network structure. This can
	involve predicting links that will appear in the future (e.g.,
	between time $T_{0}$ and time $T_{0'}$), which is highly relevant
	for networks evolving over time.

## Updated context of Graph Signal Processing

Time series data from neuroimaging modalities, such as fMRI
(functional Magnetic Resonance Imaging) BOLD or EEG
(Electroencephalography), can be effectively modeled as a graph
structure to leverage sophisticated machine learning algorithms like
Graph Neural Networks (GNNs) and Graph Transformers.

This modeling approach is particularly valuable because the complex
data structure inherent in these imaging modalities aligns well with
the **networked organizational structure of the human brain**.

The general strategy involves three key steps: defining the nodes,
determining the node features (signals), and inferring the edges
(connectivity).

### 1. Defining Nodes and Node Features

In the context of brain imaging, the nodes in the graph typically
represent regions or entities of interest.

*   **Node Identity:** Nodes are often defined as specific brain
	regions, or, at a finer scale, as individual pixels or clustered
	areas used in modalities like calcium imaging.
*   **Node Features ($X_i$):** These features represent the localized
	information or time series signal associated with each node.
	*   For time series forecasting, a subset of nodes can serve as
		"forecasting entities" with a given past time series, denoted
		as $X_i^t$.
	*   Nodes can be annotated with arbitrary features, which are
		typically represented as vectors.
	*   In traditional machine learning pipelines applied to graphs,
		features can be handcrafted structural characteristics derived
		from the network topology, such as **node degree** (number of
		connections), **centrality measures** (like closeness or
		betweenness centrality), **clustering coefficient** (measuring
		neighbor interconnectedness), or **Graphlet Degree Vectors**
		(counting specified local subgraph structures, or graphlets).

### 2. Inferring Edges and Connectivity

Edges represent the relationships, interactions, or **connectivity
patterns** between the brain regions (nodes). The method for inferring
these connections transforms the raw time series data into a
relational structure:

*   **Functional Connectivity:** Graph theory analysis based on
	**fMRI** is used to investigate alterations of brain functional
	networks. Similarly, GNN models can investigate the alterations of
	connectivity patterns associated with disorders like Autism
	Spectrum Disorder (ASD). This functional connectivity represents
	the estimated statistical relationships between the time series of
	different brain regions.
*   **Correlation-Based Clustering:** For mesoscale dynamics,
	connectivity can be inferred by clustering spatially and
	temporally related activity. For example, two active pixels can be
	clustered in the same "avalanche" (clustered activity) if they are
	within one time frame and a given radius ($r$) of one
	another. This radius $r$ is estimated from the **Partial
	Correlation Function (PCF)**, which measures direct linear
	interaction between pixels.
*   **Neighborhood Overlap Metrics:** Connectivity can be quantified
	using link features that capture the overlap between the local
	neighborhoods of two nodes, such as the number of common neighbors
	or the Jaccard coefficient. More globally, metrics like the **Katz
	Index** count the number of paths of all different lengths between
	two nodes, with longer paths being exponentially discounted by
	their length, $l$.

### 3. Application in Machine Learning

Once the time series data is structured as a graph, it can be utilized
by specialized machine learning models:

*   **Graph Learning Frameworks:** Libraries like PyTorch Geometric
	(PyG) provide the framework for storing and processing this
	structured data, defining the node features ($X$), the graph
	connectivity (Edge Index), and any edge attributes.
*   **GNNs and Graph Transformers:** These methods are designed to
	extract important information from graphs.
	*   Graph learning provides a means to model the **interactions of
		multiple brain regions**.
	*   Graph Convolutional Networks (GCNs) are used in brain
		connectivity analysis to provide interpretable deep learning
		models. GNNs can also be used for individualized cortical
		parcellation on large **fMRI datasets**.
	*   In time series forecasting, Graph Transformers or GNNs are
		used to obtain **Graph entity encodings** ($\phi_i$). The
		subgraphs used for encoding are sampled to ensure that they
		only contain nodes with a timestamp earlier than the current
		prediction time, preventing future information leakage.
*   **Graph Signal Processing (GSP):** Techniques like the **Graph
	Fourier Transform (GFT)** can be applied to the resulting graph
	structure, where low-frequency components are used to approximate
	extended source activation. This is used in solving the
	**Electrophysiological source imaging problem** (highly relevant
	to EEG/MEG) using a BiLSTM neural network.

Graph learning is also incorporated into multimodal modeling
frameworks, allowing the integration of networks derived from
different brain imaging modalities (like functional and structural
networks).

## Applied description

Graph machine learning methods are highly effective tools for
observing and quantifying changes in functional networks, including
transitions between clinical or physiological states. This capacity
stems from the fundamental alignment between graph structures and the
**networked organizational structure of the human brain**. Graph
learning approaches are specifically designed to extract important
information from graphs and model the **interactions of multiple brain
regions**.

### 1. Identifying Alterations Associated with Clinical Disorders

Graph machine learning (GML) frameworks, particularly those utilizing
Graph Neural Networks (GNNs), have been applied directly to brain
imaging data to classify disorders and investigate corresponding
network alterations:

*   **Autism Spectrum Disorder (ASD):** An invertible dynamic Graph
	Convolutional Network (GCN) model was developed to **identify
	ASD** and **investigate the alterations of connectivity patterns**
	associated with the disorder. This approach provides an
	interpretable deep learning model for brain connectivity analysis.
*   **Bipolar Disorder (BD-I):** Researchers have explored the
	**aberrant functional connectivity** of sensory motor networks in
	BD-I patients and found a significant relationship between
	**abnormal intranetwork and internetwork functional connectivity
	values**, clinical symptoms, and executive function.
*   **Hearing Loss:** Graph theory analysis based on fMRI has been
	used to investigate **alterations of brain functional networks**
	in infants with profound bilateral congenital sensorineural
	hearing loss (SNHL), offering insights into functional network
	alterations in the early stage of the condition.
*   **Parkinson's Disease (PD):** Multimodal modeling frameworks, such
	as JOIN-GCLA (Joining Omics and Imaging Networks via Graph
	Convolutional Layers and Attention), use multiple graph
	convolution layers and an attention mechanism to combine
	multi-modal imaging and multi-omics datasets for the **prediction
	of PD**.

### 2. Observing Pharmacologically Induced State Transitions

Network analysis, which provides the foundation for GML approaches by
defining graph structure and features, can reveal distinct transitions
in brain activity associated with altered states of consciousness,
such as those induced by anesthetics:

*   **Multiple Pathways Away from Criticality:** Research using
	mesoscale cortical calcium imaging in mice studied the
	manipulation of the critical state (associated with healthy quiet
	wakefulness, QW) using various anesthetics (isoflurane, ketamine,
	pentobarbital). The study observed **multiple transitions away
	from the homeostatic critical state** observed during QW.
*   **Quantifying State Differences:** These transitions are
	quantified by measuring changes in network characteristics, such
	as:
	*   **Scale-Free Statistics:** Quiet wakefulness exhibits
		scale-free statistics in clustered neuronal activity
		(avalanches). Surgical plane anesthesia often induces
		**multiple dynamical modes**, most of which **do not maintain
		critical avalanche dynamics**.
	*   **Functional Connectivity/Correlation:** Changes are observed
		in the **Partial Correlation Function (PCF)**, which measures
		direct linear interaction between pixels and is used to define
		the clustering radius ($r$) for avalanches. For example,
		surgical plane isoflurane was found to heavily reduce the PCF.
	*   **Distinct Dynamical Modes:** Surgical planes of different
		anesthetics induce qualitatively different dynamics, such as
		**aperiodic cortex-wide bursts** (isoflurane) or **wave-like
		activity** (ketamine). These manifest as substantial
		differences in the **Kolmogorov–Smirnov distance ($D_{KS}$)**
		relative to QW recordings, which are used as a
		binning-independent measure of dissimilarity.
*   **Awake-Like Dynamics:** Interestingly, some recordings under
	surgical plane anesthesia for isoflurane and ketamine exhibited
	**awake-like (AL) dynamics**, with avalanche statistics and PCF
	resembling QW, despite the subject being in a
	loss-of-responsiveness state. This highlights that observing
	network characteristics is key to understanding the underlying
	state, which may be more nuanced than a simple binary
	classification of "critical" or "non-critical" states.
