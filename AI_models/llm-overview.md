# Large Language Model

Large Language Models (LLMs) are defined as **massive deep learning
models** that contain billions of parameters and are conventionally
implemented using transformer architectures. They are trained on
massive text corpora, often trillions of words, to probabilistically
generate responses.

The core function of an LLM, particularly the variant underlying tools
like ChatGPT, is to take an input (a piece of text, and sometimes
accompanying images or sound) and produce a prediction for what comes
next in the passage.

Here is a description of the key concepts and steps involved from a
user's input (prompt) to the generated reply:

## Key Concepts and the LLM Workflow

The processing of a user's input, or **prompt**, through an LLM
involves several essential stages:

### 1. Input Processing: Tokenization and Embedding
The first step in processing input text is to break it down.

*   **Tokenization:** This is the process of splitting the input and
	output texts into smaller units, called **tokens**, that the LLMs
	can process. Tokens typically consist of words, small pieces of
	words, punctuation, or common character combinations.
*   **Embedding:** Each token is associated with a **vector**, which
	is a high-dimensional list of numbers that encodes the piece's
	semantic meaning. This means that words or concepts with similar
	meanings tend to have vectors located close to each other in this
	high-dimensional embedding space. For instance, in GPT-3, the
	embedding dimension is 12,288. The prompt is evaluated in this
	embedding space to determine which parts are most important to
	"attend to".

### 2. Contextual Transformation: The Transformer Core
The sequence of vectors (embeddings) then passes through the layers of
the transformer network.

*   **Attention:** This operation, found in **attention blocks**,
	allows the vectors to communicate with one another to update their
	values. Attention is crucial because LLMs **selectively weight the
	importance of words** within the context of their embeddings. This
	mechanism is responsible for figuring out which words in the
	context are relevant to updating the meanings of other words,
	allowing the model to distinguish meanings based on surrounding
	text (e.g., distinguishing between "machine learning model" and
	"fashion model").
*   **Multi-Layer Perceptron (MLP) / Feed-Forward Layer:** After the
	attention block, the vectors pass through this operation. In this
	step, the vectors do not communicate with each other but undergo
	the same operation in parallel.
*   The overall goal of repeatedly passing the data through
	alternating attention blocks and MLP blocks is to enable each
	vector to "soak up a meaning that's much more rich and specific
	than what mere individual words could represent".

### 3. Output Generation: Prediction and Sampling
Once the input has been fully processed, the LLM generates the
response, one segment (token) at a time.

*   **Prediction and Logits:** The final vector in the sequence is
	used to predict the next possible token. This involves mapping the
	vector to a raw list of values, called **logits**, using the
	Unembedding matrix, where there is one value for every token in
	the vocabulary.
*   **Softmax and Probability Distribution:** The softmax function is
	used to normalize the logits into a **probability distribution**
	over all possible next tokens. This distribution ensures each
	value is between 0 and 1 and that all values sum up to 1.
*   **Sampling:** The model selects a token from this distribution. A
	setting called **temperature ($T$)** can be used to influence the
	randomness of this selection: a higher $T$ makes the distribution
	more uniform, leading to more diverse or potentially nonsensical
	outputs, while a lower $T$ makes the response more predictable by
	prioritizing the most likely token.
*   **Iteration:** The newly chosen token is appended to the input
	text, and the entire process of prediction, sampling, and
	appending is repeated to generate the subsequent tokens until the
	complete response is formed.

## Augmenting LLMs with External Information (RAG)

While LLMs are powerful based on their massive training data, a
technique called **Retrieval-Augmented Generation (RAG)** is commonly
used to enhance performance, particularly by incorporating information
that was not available in the original training data. RAG is
considered a standard way of augmenting a model to specialize in a
task without needing to retrain the underlying model.

The RAG process typically involves:

1.  **Data Encoding:** External documents (unstructured text, PDFs,
	knowledge graphs, etc.) are converted into **LLM embeddings**
	(numerical representations) and stored in a **vector database**.
2.  **Retrieval:** When a user poses a query, a document retriever
	selects the most relevant documents by comparing the prompt's
	embedding to the stored embeddings based on semantic distance.
3.  **Augmentation:** This relevant retrieved content is then
	integrated into the LLM's prompt, a process sometimes referred to
	as "prompt stuffing," which guides the model's response
	generation.
4.  **Generation:** The LLM generates the final output using both the
	initial query and the augmented context from the retrieved
	documents.

By incorporating RAG, LLMs can access domain-specific or updated
information, helping to reduce common issues like AI hallucinations
and allowing for the inclusion of verifiable citations in
responses. Tools like Google's NotebookLM utilize RAG principles by
digesting and analyzing user-uploaded sources (like PDFs, Google Docs,
websites, and YouTube videos) using multimodal capabilities (Gemini
1.5) to glean and synthesize insights, and provide citations linked
back to the original passages.

## Why has RAG died?

The difference between traditional Retrieval-Augmented Generation
(RAG) and newer agentic approaches lies primarily in the **control
flow** and the model's ability to selectively utilize capabilities
(referred to as **tools**) to accomplish a task. Agentic strategies
are considered crucial steps beyond earlier forms of RAG, such as
"naive chunk retrieval".

Here is a description of how these two approaches differ based on the
material provided:

### The old Retrieval-Augmented Generation (RAG)

RAG is defined as a standard way of augmenting an LLM to specialize in
a task without needing to retrain the underlying model. The core
mechanism of RAG is focused on efficiently retrieving relevant
external data and integrating it directly into the LLM's input (the
prompt) for generation.

*   **Process Flow:** RAG is typically a multiphase process. It
	involves converting external data into vector embeddings and
	storing them in a **vector database**. When a user submits a
	query, a document retriever identifies the most relevant
	information based on the semantic distance between the query
	embedding and the stored data.
*   **Context Injection:** The identified content is then fed into the
	LLM via **prompt engineering** of the user's original query, a
	process sometimes called "prompt stuffing". The LLM generates a
	response based on this augmented prompt and its internal training
	data.
*   **Purpose:** RAG helps the LLM use domain-specific or updated
	information, which helps mitigate issues like hallucinations and
	reduces the need for expensive model retraining.

### The new Agentic Approach

Agentic approaches elevate the LLM from a simple sequence generator to
an **orchestrator** capable of complex reasoning and taking actions
using various external components or "tools".

*   **Core Mechanism: Tool Use:** In agentic workflows, LLMs are
	equipped with **tool interfaces**. These tools are programs that
	the LLM can choose to run for external context or computational
	tasks. Examples of such tools include multiplication functions (a
	calculator) or a dedicated document search function.
*   **Modular Control Flow (The Key Difference):** Unlike RAG, where
	retrieval happens outside the model's reasoning loop and the
	retrieved text is forced into the prompt, an agent decides when
	and how to invoke external functionality.
	*   The agent is given a question along with the schema (name,
		description, arguments) of available tools.
	*   The agent decides if a tool is necessary, selects the
		appropriate tool, and generates the arguments to be passed to
		it.
	*   The agent receives the result from the tool and interpolates
		it into the final response.
	*   This allows the agent to **seamlessly switch** between
		different types of actions, such as performing calculations or
		searching documents, within a single task.
*   **RAG as a Tool:** In an agentic workflow, RAG itself is
	frequently implemented as one of the specialized tools the agent
	can call. For instance, a LlamaIndex agent can be equipped with a
	`search_documents` function powered by a `VectorStoreIndex` (RAG)
	and a separate `multiply` function (a calculation tool).
*   **Orchestration and Persistence:** Agentic systems facilitate more
	complex application characteristics such as cycles (loops), the
	maintenance of short-term and long-term memory (**persistence**),
	and even **human-in-the-loop** interactions. Frameworks like
	LangGraph are designed specifically for expressing these complex
	control flows.
*   **Standardization:** Protocols like the **Model Context Protocol
	(MCP)**, introduced by Anthropic, offer a standardized way for LLM
	Agents to communicate with and selectively access external tools,
	data, and services securely. This enhances the agent's modularity
	by defining a uniform way to access local knowledge bases, web
	searches, and other context providers.

In essence, RAG focuses primarily on **augmenting the context input**
with retrieved information, while agentic approaches focus on
**orchestrating multiple steps and tools**, one of which can be a RAG
system, to generate a comprehensive and reasoned reply.
