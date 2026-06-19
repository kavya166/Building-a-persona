# Document Q&A Bot using RAG (Retrieval-Augmented Generation)

## Project Overview

This project is a Retrieval-Augmented Generation (RAG) based Document Question Answering Bot built in Python. The system allows users to ask natural language questions about a collection of documents and receive grounded answers generated from the document content.

The bot processes multiple documents, converts them into embeddings, stores them in a vector database, retrieves the most relevant document chunks for a query, and uses Google's Gemini model to generate answers with source citations.

---

# Features

* Supports PDF, DOCX, and TXT documents
* Automatic document chunking with overlap
* Semantic search using vector embeddings
* Persistent vector storage using ChromaDB
* Retrieval-Augmented Generation (RAG)
* Answer generation using Google Gemini
* Source citations included with every answer
* Handles unanswerable questions by restricting responses to retrieved context

---

# Document Collection

The knowledge base contains the following documents:

1. Artificial_Intelligence.pdf
2. Cloud_Computing.docx
3. Climate_Change.docx
4. Cybersecurity.txt
5. Startup_Funding.txt

These documents cover multiple domains to support diverse question-answering scenarios.

---

# Tech Stack

| Component               | Technology                             |
| ----------------------- | -------------------------------------- |
| Programming Language    | Python 3.11+                           |
| LLM                     | Gemini 2.5 Flash                       |
| Embedding Model         | sentence-transformers/all-MiniLM-L6-v2 |
| Vector Database         | ChromaDB                               |
| Document Loaders        | LangChain Community                    |
| PDF Processing          | PyPDF                                  |
| DOCX Processing         | docx2txt                               |
| Text Chunking           | RecursiveCharacterTextSplitter         |
| Development Environment | Google Colab / Local Python            |
| Framework               | LangChain                              |

## Libraries Used

* langchain-community
* langchain-text-splitters
* chromadb
* sentence-transformers
* pypdf
* docx2txt
* google-generativeai

---

# Architecture Overview

The system follows a standard RAG pipeline:

```text
Documents
    ↓
Document Loading
    ↓
Text Chunking
    ↓
Embedding Generation
    ↓
Chroma Vector Database
    ↓
User Question
    ↓
Similarity Search
    ↓
Top-K Relevant Chunks
    ↓
Gemini LLM
    ↓
Answer + Source Citations
```

---

# Document Ingestion

The system loads documents from the local data directory.

Supported formats:

* PDF
* DOCX
* TXT

Each document is parsed and converted into LangChain document objects with metadata.

---

# Chunking Strategy

### Strategy Used

RecursiveCharacterTextSplitter

### Configuration

```python
chunk_size = 800
chunk_overlap = 150
```

### Why This Strategy?

RecursiveCharacterTextSplitter preserves semantic structure better than fixed-size splitting. It attempts to split text at logical boundaries while maintaining context.

Chunk overlap helps preserve information that may span across chunk boundaries and improves retrieval quality.

---

# Embedding Model

### Model Used

```text
sentence-transformers/all-MiniLM-L6-v2
```

### Why This Model?

* Lightweight and efficient
* Free and open-source
* Fast inference
* Strong semantic similarity performance
* Widely used for beginner and production RAG systems

---

# Vector Database

### Database Used

ChromaDB

### Why ChromaDB?

* Easy setup
* Local persistence
* Tight integration with LangChain
* No external infrastructure required
* Suitable for small and medium-sized RAG applications

The vector database stores document embeddings and enables efficient similarity search during retrieval.

---

# Retrieval Process

When a user submits a question:

1. The query is converted into an embedding.
2. ChromaDB performs similarity search.
3. Top-k relevant chunks are retrieved.
4. Retrieved chunks are passed to Gemini as context.
5. Gemini generates an answer strictly based on retrieved information.

Default retrieval parameter:

```python
k = 4
```

---

# Answer Generation

### Model

Gemini 2.5 Flash

### Prompting Strategy

The model is instructed to:

* Answer only from retrieved context
* Avoid using external knowledge
* Return a fallback response when information is unavailable

Fallback response:

```text
I could not find the answer in the provided documents.
```

This helps reduce hallucinations and keeps responses grounded.

---

# Setup Instructions

## Clone Repository

```bash
git clone <repository-url>
cd rag-document-bot
```

## Create Virtual Environment

```bash
python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### Linux / Mac

```bash
source venv/bin/activate
```

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Add API Key

Create a `.env` file:

```env
GEMINI_API_KEY=YOUR_API_KEY
```

## Run the Project

Run indexing:

```bash
python index.py
```

Run the chatbot:

```bash
python app.py
```

---

# Environment Variables

Required:

```env
GEMINI_API_KEY=YOUR_API_KEY
```

Never commit API keys to GitHub.

---

# Example Queries

### Query 1

```text
What are the benefits of cloud computing?
```

Expected Theme:

* Scalability
* Cost efficiency
* Reliability

### Query 2

```text
What are common cybersecurity threats?
```

Expected Theme:

* Malware
* Phishing
* Ransomware

### Query 3

```text
How does startup funding work?
```

Expected Theme:

* Venture capital
* Angel investors
* Funding rounds

### Query 4

```text
What causes climate change?
```

Expected Theme:

* Greenhouse gases
* Carbon emissions
* Human activities

### Query 5

```text
How is AI used in healthcare?
```

Expected Theme:

* Medical imaging
* Diagnostics
* Drug discovery

### Query 6

```text
Who won the FIFA World Cup in 1998?
```

Expected Result:

```text
I could not find the answer in the provided documents.
```

---

# Sample Output

```text
Answer:
Cloud computing provides scalability, cost efficiency,
and reliable access to computing resources.

Sources:
Cloud_Computing.docx | Page N/A
```

---

# Known Limitations

* Limited to information available in uploaded documents.
* Retrieval quality depends on chunking and embedding effectiveness.
* Small document collections may produce limited context.
* Source citations are based on document metadata and may not always contain precise section information.
* The system currently uses similarity search only and does not implement advanced reranking techniques.

---

# Future Improvements

* Streamlit web interface
* Conversational memory
* Hybrid search (keyword + vector search)
* Reranking models
* Support for larger document collections
* Multi-document summarization

---

# Author

AI Engineering Internship Assignment

Document Q&A Bot using Retrieval-Augmented Generation (RAG)
