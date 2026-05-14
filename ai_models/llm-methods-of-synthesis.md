# Methods of Synthesizing Information During LLM Training

These are notes regarding a systematic literature review that we
attempted to develop on the topic of information consolidation and
transparency in LLMs.

This inquery addresses the critical challenge of ensuring
**faithfulness and transparency** in Large Language Model (LLM)
outputs, moving beyond models that rely solely on their opaque
internal knowledge base (weights).

## How is information added?

The primary methods used to fine-tune or augment a model in a way that
allows a human to verify the information driving the decision are
**Retrieval-Augmented Generation (RAG) with Citation**, the
implementation of **Human-in-the-Loop** workflows, and specialized
**Data-Centric Fine-Tuning** coupled with rigorous metrics.

### 1. Retrieval-Augmented Generation (RAG) for Verifiable Citation

RAG is the most direct and functional method for human verification
because it explicitly links the generated text to the external sources
consulted.

*   **Evidence-Based Response Generation:** RAG integrates external
	knowledge retrieval into the LLM's generation process to improve
	accuracy and **interpretability**. The workflow involves fetching
	relevant documents (retrieval), processing the query along with
	the retrieved evidence, and generating a response that is
	**evidence-based**.
*   **Direct Citations for Verification:** The LLM output is designed
	to include sources in the response, allowing users to **verify the
	cited sources**. This gives users the ability to cross-check the
	retrieved content, ensuring accuracy and relevance.
*   **Examples of Verifiable Systems:** Tools utilizing RAG
	principles, such as Google's NotebookLM, are explicitly designed
	to take information, digest and analyze it, and then **provide
	citations that link back to the most relevant original passages in
	your sources**.
*   **Mitigating Hallucination:** This transparency helps reduce
	common issues like AI **hallucinations** (plausible-sounding but
	incorrect information) by ensuring the output is grounded in
	verifiable source material.

### 2. Human-in-the-Loop and Agentic Workflows

Advanced systems move beyond simple input-output by incorporating
human oversight and external tools, which makes the decision path more
auditable.

*   **Researcher-in-the-Loop (RIL):** Integrating
	**researcher-in-the-loop algorithms** is an advanced technique
	utilized to help mitigate misinformation in AI-driven
	systems. This involves a human expert actively reviewing and
	influencing the automated process.
*   **Agent Orchestration:** Agentic approaches, particularly those
	built using frameworks like LangGraph, support
	**Human-in-the-loop** interactions. This means the application
	flow can be designed to **pause, review, edit, or approve certain
	steps** by a human. This capability allows a human reviewer to
	verify the tool's decision-making process at critical junctures
	within a complex task.
*   **Explainable Information Retrieval (XIR):** AI's role in
	automating evidence retrieval also improves precision and
	**transparency** through techniques like XIR and relevance
	ranking.

### 3. Fine-Tuning and Specialized Models

While base LLMs are trained on massive corpora, the process of
specialization or fine-tuning applies additional training to refine
performance for specific, often factual, domains.

*   **Data-Centric Fine-Tuning:** One technique for mitigating medical
	hallucinations focuses on **improving LLM performance through
	high-quality, domain-specific data**. This involves **fine-tuning
	LLMs on specialized biomedical datasets** to enhance accuracy and
	reduce hallucinations. For example, fine-tuning LLMs for
	biomedical research advances evidence-based medicine.
*   **Model-Centric Refinement:** Other methods involve post-training
	modifications to refine factual accuracy, such as **Model
	Knowledge Editing** (directly modifying model weights to correct
	factual errors without full retraining) or using **Critic Models**
	(auxiliary models that critique and improve LLM outputs).
*   **Structured Reasoning (CoMT/CoK):** Techniques like
	**Chain-of-Medical-Thought (CoMT)** use structured prompting to
	mimic clinical reasoning steps, improving accuracy in medical
	report generation. Similarly, **Chain-of-Knowledge (CoK)**
	dynamically integrates domain knowledge to improve factual
	correctness in multi-step reasoning. By breaking down the
	reasoning process, these methods offer a structured path that is
	easier for a human to follow and verify logic steps.

### 4. Verification Through Evaluation Metrics

Beyond providing inline citations, verification relies on measurable
metrics that confirm the model's output quality, which are often
checked by humans.

*   **Reference Accuracy:** Evaluation measures designed to assess
	retrieval systems include **Reference Accuracy**, which
	specifically measures the correctness of citations or references
	in evidence synthesis.
*   **Quality Scores:** Metrics such as the **Overall Quality Score
	(OQS)** evaluate the quality of outputs based on checklists (like
	CONSORT-A).
*   **Faithfulness Metrics (RAG):** When evaluating RAG systems,
	**Faithfulness** is a key metric for the generator, quantifying if
	the generated response is based only on the retrieved context.
*   **Human Evaluation:** Ultimately, **Human Evaluation Metrics** are
	used to assess the overall accuracy, relevance, and occurrence of
	hallucinations in the answers.

## Methods of Evidence Retrieval During Training

The methods of evidence retrieval and augmentation that exist
**during** an LLM's training—or are integrated into its
architecture/fine-tuning process to enhance its eventual factual
function—fall broadly into structural modifications and data-centric
approaches.

### 1. Architectural Integration (Retrieval during Pre-training)

A fundamental way that evidence retrieval is integrated into an LLM
during its initial development is through specific architectural
designs, such as the **Retro** model.

*   **Retro (Retrieval-Augmented Language Model):** This approach
	redesigned the LLM structure with the retriever mechanism
	incorporated directly into the language model.
	*   **Training from Scratch:** Unlike standard RAG, which augments
		a fully trained model, Retro is trained from scratch. This
		process allows the model to receive **domain knowledge during
		training**.
	*   **Component Modification:** The Retro model incorporates
		retrieval functionality using specialized structures like
		**Chunked Cross Attention** layers within its processing
		blocks.
	*   **Efficiency:** The hypothesis behind Retro is that by
		embedding domain knowledge during training, the model needs
		less focus on the domain during later stages and can devote
		its (potentially smaller) weight resources primarily to
		language semantics.
*   **Retro++:** A more reproducible version of this method, known as
	Retro++, includes in-context RAG.

### 2. Fine-Tuning and Data-Centric Augmentation

After the initial large-scale pre-training (which establishes the
initial word embeddings), models can undergo transfer learning or
fine-tuning to specialize them for specific domains, improving their
functional accuracy and evidence retrieval capabilities.

*   **Improving Data Quality and Curation:** This data-centric
	approach focuses on boosting LLM performance by utilizing
	**high-quality, domain-specific data**. For instance, fine-tuning
	LLMs on specialized **biomedical datasets** is a technique used to
	enhance accuracy and reduce factual errors (hallucinations) in
	clinical applications.
*   **Transfer Learning:** This involves utilizing the existing
	pre-trained weights (embeddings) and transferring that context to
	facilitate learning on more **specialized or fine-tuned
	data**. The goal is to produce responses that are customized based
	on more targeted training inputs.

### 3. Knowledge Structuring and External Retrieval Techniques

Retrieval methods are critical for integrating external knowledge
bases, which act as verifiable evidence. These techniques are often
optimized during model specialization or deployment preparation:

*   **Retrieval-Augmented Generation (RAG):** While RAG is often
	applied post-training, it is a **standard way of augmenting a
	model** to specialize in a task without the high cost of
	retraining the entire model. It involves augmenting the model's
	knowledge base with updated, domain-specific information derived
	from external sources like databases or uploaded documents.
*   **Medical Knowledge Graphs (KGs):** Structured knowledge bases are
	used during augmentation to enhance factual grounding. KGs
	structure medical knowledge into interconnected entities, which
	helps in enhancing reasoning and reducing hallucinations.
*   **Chain-of-Knowledge (CoK):** This is a structured prompting
	technique that **dynamically integrates domain knowledge** to
	improve factual correctness, particularly in complex, multi-step
	reasoning processes.
*   **Hashing-Based Similarity Search:** Advanced techniques used to
	improve retrieval capabilities include hashing-based similarity
	search. Specifically, **topology-preserving hashing** is employed
	to maintain data relationships within graph structures, which is
	crucial for the development of biomedical knowledge graphs used as
	evidence sources.

### 4. Retriever Component Refinement

Even if the core LLM weights are static, the performance of the
retriever—the component responsible for finding the evidence—can be
enhanced during auxiliary training runs:

*   **Pre-training the Retriever:** The retriever itself can be
	optimized using methods like the **Inverse Cloze Task (ICT)**,
	which trains the retriever to learn optimal retrieval patterns by
	having it predict masked text within documents.
*   **Supervised Optimization:** This process involves aligning the
	probabilities of the retriever's selections with the likelihood
	distribution of the generative LLM, thereby helping to refine the
	retrieval quality based on what the model needs to generate
	accurate text.
*   **Reranking:** During training or optimization, reranking
	techniques are used to refine the retriever's output by
	prioritizing the most relevant documents extracted from the
	database.

## Efficiency - Computation and Performance

The most computationally efficient means of improving evidence
retrieval during an LLM's development and training stages primarily
involve architectural changes to reduce model size, or optimizing the
retrieval component separately from the massive language model
weights.

Here is a breakdown of computationally efficient methods for improving
evidence retrieval, organized by their impact on hardware requirements
and model performance, based on the provided sources:

### 1. Architectural Integration (Retrieval-Augmented Models)

This approach focuses on building retrieval capability directly into
the model structure from the start, yielding a final model that
requires less compute power for comparable results.

| Aspect | Method Details (Efficiency) | Performance Improvement |
| :--- | :--- | :--- |
| **Hardware Requirements** | The **Retro** (Retrieval-Augmented Language Model) approach integrates a retriever mechanism into the transformer architecture itself and is trained from scratch. This method results in a **25-times smaller network** that can achieve comparable performance to its much larger counterparts. While the initial training run is high cost, the resulting smaller network significantly reduces the computational and resource demands for deployment and inference,. The subsequent, more reproducible version, **Retro++**, further includes in-context Retrieval-Augmented Generation (RAG). | The smaller Retro network achieves **comparable perplexity** (a measure of how well a probability model predicts a sample) to larger models. This incorporation of retrieval functionality during training allows the model to receive **domain knowledge** directly, potentially enhancing its factual grounding compared to purely generative models. |

### 2. Incremental Training (Fine-Tuning)

Instead of the expensive process of training massive LLMs (which are
typically defined as deep learning models containing billions of
parameters trained on trillions of words), **Transfer Learning** is a
highly efficient technique because it avoids retraining the
foundational weights.

| Aspect | Method Details (Efficiency) | Performance Improvement |
| :--- | :--- | :--- |
| **Hardware Requirements** | **Transfer Learning** utilizes the **pre-trained weights** (embeddings) established during the initial, unsupervised training stage,,. Using this existing context allows the model to be **specialized or augmented** for specific domains without requiring the computational resources necessary for full foundational training,. Although specialization is generally considered expensive, fine-tuning the base weights is inherently more resource-efficient than starting the training process from scratch. | Fine-tuning LLMs with **high-quality, domain-specific data** (a data-centric approach) enhances accuracy and reduces issues like **hallucinations**. For example, fine-tuning LLMs on specialized **biomedical datasets** improves factual accuracy. This specialization ensures the resulting responses are customized and based on more targeted training inputs. |

### 3. Optimization of Retrieval Components

Computational efficiency can also be achieved by focusing training
resources on optimizing the search tools, which are smaller components
than the full LLM.

| Aspect | Method Details (Efficiency) | Performance Improvement |
| :--- | :--- | :--- |
| **Hardware Requirements** | The retriever component itself can be pre-trained using methods like the **Inverse Cloze Task (ICT)**, which trains it to learn optimal retrieval patterns by predicting masked text within documents. Alternatively, **Supervised Retriever Optimization** involves aligning the retriever’s probabilities with the generative model’s likelihood distribution. Since this focuses on refining the alignment of a smaller component (the retriever), the training effort is localized and more efficient than retraining the entire LLM. | These methods refine the **retrieval quality** by minimizing the difference (KL divergence) between the retriever's selections and the LLM's likelihoods, leading to better input context for the generator. |

### 4. Indexing and Search Algorithm Optimization

The efficiency of evidence retrieval relies heavily on how quickly the
model can locate relevant documents in a database (vector
store). Techniques applied during the data preparation and search
setup minimize runtime computation during RAG applications:

| Aspect | Method Details (Efficiency) | Performance Improvement |
| :--- | :--- | :--- |
| **Hardware Requirements** | Using **Approximate Nearest Neighbor (ANN) searches** significantly improves retrieval efficiency when looking up vector similarities compared to the computationally heavier K-nearest neighbors (KNN) searches. Hybrid vector approaches that combine **dense vector representations** with **sparse one-hot vectors** take advantage of the intrinsic **computational efficiency of sparse dot products** over dense vector operations. Additionally, **topology-preserving hashing** is employed for **scalable similarity search**, which is crucial for handling large graph structures like biomedical knowledge graphs,. | Techniques like **hashing-based similarity search** enhance the overall search capabilities, and ensure that crucial data relationships within complex evidence structures are maintained. |
