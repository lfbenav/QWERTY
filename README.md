# QWERTY — Conversational Agent for PDF Document Analysis

QWERTY is an intelligent conversational agent designed to analyze and answer questions about class notes from the Artificial Intelligence course in PDF format.
It integrates RAG (Retrieval-Augmented Generation) and web search capabilities, enabling contextual, accurate, and memory-aware responses.

## Features
- Contextual Question Answering
Ask QWERTY about any concept or section from the uploaded course notes, and it will retrieve precise answers from the document database with their references.

- Conversational Memory
QWERTY remembers what you’ve asked within the same session — you can continue the conversation naturally.

- Web Search Integration
When needed, QWERTY can access the web to complement its responses with up-to-date information.

- Dual RAG Comparison
Implements two RAG pipelines with different text segmentation strategies to compare retrieval performance.

## Functionality
### Introducing QWERTY
QWERTY introduces itself and explains its purpose.  
<img width="1760" height="460" alt="Captura de pantalla 2025-11-10 220753" src="https://github.com/user-attachments/assets/19243215-e206-497b-8c59-7c2f0bde0def" />

### Answering Questions About the Course
Ask it anything from the uploaded notes — it retrieves and explains concepts directly from them.  
<img width="1732" height="634" alt="Captura de pantalla 2025-11-10 220809" src="https://github.com/user-attachments/assets/4db0c2fa-11e1-49f1-ae9e-385e28773a9d" />

### Memory and Context
QWERTY remembers previous exchanges to keep context across turns.
<img width="1749" height="621" alt="Captura de pantalla 2025-11-10 220823" src="https://github.com/user-attachments/assets/4a45ade1-06b2-4c71-b2cc-62dc2bba50e2" />

### Web Search Capability
When explicitly requested, QWERTY performs real-time searches on the web to expand its knowledge.
<img width="1727" height="766" alt="image" src="https://github.com/user-attachments/assets/c8c1ec8d-a85f-44b3-80e2-cd6f409d17f7" />

## Architecture

QWERTY follows a modular RAG (Retrieval-Augmented Generation) architecture implemented in Python using `LangChain`, `Streamlit`, and `Ollama`.
Its workflow can be summarized in six main stages:

1. ### Data Ingestion & Cleaning  
Class notes in PDF format are read using `pdfminer` and converted into plain text. Each document is normalized; accents, special characters, and extra spaces are removed to ensure clean input for embedding generation.

2. ### Text Segmentation  
Two segmentation strategies are implemented for empirical comparison:

  - **Fixed segmentation**: splits the text into fixed-length chunks with overlap.

  - **Semantic segmentation**: groups sentences using nltk sentence tokenization, preserving contextual coherence.

3. ### Embeddings & Vector Databases  
Each chunk is converted into a 768-dimensional semantic vector using the `intfloat/multilingual-e5-base model`. Embeddings are stored in `Chroma` vector databases (one per segmentation type) for efficient similarity-based retrieval.

4. ### Tools Layer

- **RAG Tools**: Two retrieval tools access the corresponding Chroma databases (fixed and semantic).  

- **WebSearch Tool**: Uses SerpAPI to query the web and format relevant snippets with sources and links.  

These tools are orchestrated dynamically depending on the user’s query intent.

5. ### Agent Orchestration & Memory

The main conversational agent is orchestrated through `LangChain` and powered by `LLaMA 3.2` (Ollama). A lightweight temporary memory buffer stores the last conversation turns, allowing QWERTY to recall and follow up on previous exchanges naturally.

6. ### User Interface
The agent runs through a `Streamlit` web app, providing a clean and responsive chat interface. Users can choose the segmentation mode (semantic or fixed), view contextual answers, and request web searches interactively.
