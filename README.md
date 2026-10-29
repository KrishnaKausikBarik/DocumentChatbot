# Simple Local RAG Chatbot

Ask questions about your own documents (`.txt`, `.md`, `.pdf`) using local embeddings, ChromaDB for vector storage, and Ollama for local LLM-based answer generation. Everything runs locally without external AI APIs.

## How It Works

```text
docs/*.txt, *.md, *.pdf
        |
        v
    [ingest.py]
        |
        v
  Split documents into chunks
        |
        v
 Generate embeddings locally using Ollama
        |
        v
   Store in ChromaDB
        |
        v
     [query.py]
        |
        v
  Embed the user's question
        |
        v
 Retrieve top-k relevant chunks
        |
        v
 Send question + retrieved context to Ollama
        |
        v
 Generate an answer using a local LLM
```

## Setup

### 1. Create a virtual environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

On macOS/Linux:

```bash
source venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Install and configure Ollama

Download Ollama from https://ollama.com and install it for your operating system.

Pull a local model for answer generation:

```bash
ollama pull llama3.2
```

Pull an embedding model:

```bash
ollama pull nomic-embed-text
```

Make sure Ollama is running before executing the chatbot.

### 4. Add your documents

Place your `.txt`, `.md`, and `.pdf` files inside the `docs/` folder. A sample document can be used to test the pipeline.

## Usage

### Ingest documents

```bash
python ingest.py
```

This reads your documents, splits them into chunks, generates local embeddings, and stores them in ChromaDB.

### Ask questions

```bash
python query.py
```

Start the interactive Q&A session and ask questions about your documents.

## What to Look At and Experiment With

- **`CHUNK_SIZE` and `CHUNK_OVERLAP` in `ingest.py`:** Adjust chunk sizes to balance retrieval precision and context.
- **`TOP_K` in `query.py`:** Control how many relevant chunks are retrieved for each question.
- **Embedding model:** Experiment with different Ollama-compatible embedding models and compare retrieval quality.
- **Generation model:** Try different local Ollama LLMs and evaluate their answer quality.
- **Chunking strategy:** Compare fixed-size chunking with paragraph-based or sentence-based splitting.

## Technologies Used

- **Python** — application logic and document processing
- **Ollama** — local embedding and LLM inference
- **ChromaDB** — vector storage and similarity search
- **Local embeddings** — semantic document retrieval
- **RAG (Retrieval-Augmented Generation)** — context-grounded question answering

## Privacy and Local Execution

- Document processing, embedding generation, vector storage, retrieval, and answer generation run locally.
- No external AI API key is required.
- No paid cloud inference service is required.
- The ChromaDB database persists in `chroma_db/` between runs.
- Re-run `ingest.py` after changing your documents or chunking configuration to rebuild the vector database.

**Note:** Local execution assumes your code is configured to use Ollama for both embeddings and generation, with the required models downloaded. Hardware requirements depend on the selected models.
