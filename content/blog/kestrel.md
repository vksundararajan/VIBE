+++
title = "Kestrel: LLM-Powered Cybersecurity Research Assistant Using RAG"
date = "2025-09-24T23:28:22-04:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai","security",]
+++

Agentic AI is changing the way we design intelligent systems. Instead of treating an LLM as a single-shot text generator, Agentic AI frameworks let us structure reasoning, retrieval, and decision-making in a way that mimics how humans research and solve problems.

<mark>Kestrel</mark> is a project where I applied this approach to a <mark>cybersecurity domain</mark>. The goal was simple: create a research assistant that can query vulnerability datasets, reason over the results, and generate reliable, context-grounded answers. Under the hood, it combines LLMs, ChromaDB, LangChain, and JSON-based datasets into a Retrieval-Augmented Generation (RAG) pipeline that is configurable and extensible.

## Designing with Agentic AI

At its heart, Agentic AI is about moving beyond **static prompts** and building systems where an agent can:

- Retrieve information when needed.
- Decide how to reason (step-by-step, question decomposition, action-response).
- Use external tools (like a vector database).
- Deliver answers that are explainable and auditable.

In Kestrel, the LLM doesn’t work in isolation. It acts as a <mark>reasoning engine</mark> wrapped inside an agent framework. The agent retrieves data from the vector database, applies a reasoning strategy, and only then generates an answer. This layered approach reduces hallucinations and keeps the system grounded.

## Technical Stack

#### 1. Dataset and JSON Files
The project starts with cybersecurity data in JSON format. Each JSON entry represents a knowledge unit — e.g., a vulnerability description, mitigation detail, or framework entry. Using JSON keeps ingestion simple and makes it easy to extend with more structured sources later (like NVD feeds, CWE, or ATT&CK).

#### 2. Vector Database with ChromaDB
The JSON files are embedded into vector representations. These embeddings capture semantic meaning of each document. I chose ChromaDB as the vector store because:

- It is lightweight and fast for prototyping.
- It integrates smoothly with LangChain.
- It can run locally without cloud dependencies.

ChromaDB indexes the embeddings and makes similarity search efficient. When a query comes in, it finds the top-k relevant entries in milliseconds.

#### 3. Using LangChain

LangChain is where Agentic AI principles show up most clearly. Instead of manually wiring everything, I used LangChain to:

- Define retrievers that connect the query to ChromaDB.
- Structure prompt templates with context windows.
- Configure agents with different reasoning strategies (Chain-of-Thought, ReAct, Self-Ask, Direct).
- Manage chains where retrieval and reasoning are combined before hitting the LLM.

This orchestration layer ensures the system can flexibly switch reasoning styles or swap components without rewriting the whole pipeline.

#### 4. Large Language Model (LLM) Integration

The LLM provides the reasoning and natural language generation. It takes in:

- The user’s query.
- Retrieved documents from ChromaDB.
- Instructions on which reasoning strategy to follow.

By combining all three, it produces answers that are not just fluent but also grounded in the knowledge base.

## Workflow in Practice

Here’s the flow Kestrel follows for each query:

1. User Input – The researcher asks a security question.
2. Retrieval – Relevant JSON entries are pulled from ChromaDB.
3. Agent Reasoning – LangChain instructs the LLM on how to process the information and Direct for concise answers.
    - <mark>Chain-of-Thought</mark> for step-by-step explanations
    - <mark>ReAct</mark> when retrieval plus tool use is needed
    - <mark>Self-Ask</mark> for breaking down complex questions
4. Answer Generation – The LLM composes the response, weaving in both the query and retrieved context.
5. Output Logging – The result is saved into structured logs for later analysis or auditing.


### Why This Approach Works
> This approach works because it keeps the answers grounded in real data through retrieval, ensuring the responses are reliable rather than guesses from the model alone. At the same time, it offers flexibility in reasoning — some situations demand a detailed explanation while others call for a concise summary, and Kestrel can adapt by switching reasoning modes accordingly. The architecture is designed to be modular, so new datasets, different LLMs, or alternative vector databases can be integrated without disrupting the core pipeline, making it easier to extend and scale. Just as importantly, every response and reasoning path is logged, giving the system a level of transparency and auditability that builds trust — an essential factor when working in cybersecurity.



## The Road Ahead

Kestrel today is a proof-of-concept, but there’s plenty of scope:

- Expanding to large, authoritative datasets (NVD, CWE, ATT&CK).
- Supporting multiple vector DBs (FAISS, Pinecone, Weaviate).
- Adding an interactive web interface.
- Building evaluation metrics to score accuracy and response quality.


## Final Thoughts

Kestrel shows what’s possible when Agentic AI principles meet a focused use case. By combining JSON datasets, ChromaDB, LangChain, and LLMs into a structured RAG pipeline, we can build assistants that don’t just talk — they retrieve, reason, and support real-world decision making.

For cybersecurity research, this kind of agent is not just convenient. It’s a step toward making the overwhelming flood of information more accessible, trustworthy, and actionable.

You can explore the project here: [Kestrel on GitHub](https://github.com/vksundararajan/Kestrel) :)