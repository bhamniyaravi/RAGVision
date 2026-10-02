# RAGVision — Multimodal Document Intelligence

A multimodal Retrieval-Augmented Generation (RAG) project for asking questions about PDF documents using text, tables, charts, and visual information.

This is my first practical RAG project and was built to understand how document processing, embeddings, vector databases, retrieval, LLMs, and vision models work together.

## 🚀 Features

- 📄 Extract text from PDF documents
- 📊 Extract tables from PDF pages
- 🖼️ Extract images, charts, and diagrams
- 👁️ Generate concise visual summaries using a vision-capable LLM
- 🔎 Create semantic embeddings using Sentence Transformers
- 🗄️ Store and retrieve document information using Pinecone
- 🤖 Generate answers using Groq LLMs
- 🧠 Use retrieved visuals when a question requires visual information
- 📌 Display retrieved source pages and modalities

## 🏗️ Architecture

```text
                  PDF Document
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        Text         Tables       Images
          │            │            │
          │            │       Vision Model
          │            │            │
          └────────────┴──── Visual Summary
                       │
                       ↓
             Hugging Face Embeddings
                       │
                       ↓
                Pinecone Vector DB
                       │
                       ↓
                   Retriever
                       │
                       ↓
              Relevant Context
                       │
              ┌────────┴────────┐
              ↓                 ↓
           Text LLM          Vision LLM
              │                 │
              └────────┬────────┘
                       ↓
                 Final Answer
```

## 🛠️ Tech Stack

- Python
- Jupyter Notebook
- LangChain
- Groq
- Pinecone
- Hugging Face Sentence Transformers
- PyMuPDF
- Pandas
- Pillow
- python-dotenv

### Models

**Text generation**
```text
openai/gpt-oss-20b
```

**Vision**
```text
qwen/qwen3.8-27b
```

**Embeddings**
```text
sentence-transformers/all-MiniLM-L6-v2
```

The embedding dimension is detected automatically by the notebook.

## 📁 Project Structure

```text
RAGVision/
│
├── architecture_diagram/
│   ├── architecture.png
│   └── multimodal.png
│
├── src/
│
├── Documents/
│   └── Internship_Report.pdf       # Local/private - not included in GitHub
│
├── internship_report_extracted_images/  # Generated locally
│
├── MultiModal_RAG.ipynb
├── README.md
├── requirements.txt
├── pyproject.toml
├── uv.lock
├── python-version
├── .gitignore
└── .env                             # Local only - never commit
```

## 💻 Run the Project on Your System

### 1. Prerequisites

Install:

- Python 3.12 or a compatible Python version
- Git
- `uv`
- A Groq API key
- A Pinecone API key

Install `uv` from the official Astral documentation if it is not already installed.

### 2. Clone the repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd RAGVision
```

### 3. Create/sync the environment with uv

If the repository contains `pyproject.toml` and `uv.lock`, run:

```bash
uv sync
```

This creates the project environment and installs the dependencies.

If you want to add a dependency later:

```bash
uv add package-name
```

For example:

```bash
uv add pillow
```

### 4. Create the environment file

Create a file named:

```text
.env
```

Add your own API keys:

```env
GROQ_API_KEY=your_groq_api_key
PINECONE_API_KEY=your_pinecone_api_key
```

Do not share or commit this file.

A safe template can be created as:

```text
.env.example
```

with:

```env
GROQ_API_KEY=your_groq_api_key_here
PINECONE_API_KEY=your_pinecone_api_key_here
```

### 5. Add your PDF

The notebook currently expects:

```text
Documents/Internship_Report.pdf
```

Create the folder if necessary:

```text
Documents/
└── Internship_Report.pdf
```

The PDF is intentionally excluded from the public repository because it may contain personal/private information.

You can replace it with another PDF, but the notebook should be updated if you change the expected path or filename.

### 6. Start Jupyter

Run:

```bash
uv run jupyter notebook
```

or:

```bash
uv run jupyter lab
```

Open:

```text
MultiModal_RAG.ipynb
```

In VS Code, you can also open the notebook directly and select the Python environment created by `uv`.

### 7. Run the notebook

Run the cells from top to bottom.

The notebook will:

1. Load environment variables.
2. Initialize Groq.
3. Open the PDF.
4. Extract text.
5. Extract tables.
6. Extract images/charts.
7. Generate visual summaries.
8. Create embeddings.
9. Create/connect to the Pinecone index.
10. Upload documents to Pinecone.
11. Create the retriever.
12. Run RAG queries.

## ⚠️ Important: Pinecone Namespace

The notebook currently clears the configured Pinecone namespace before uploading the extracted documents.

This code:

```python
index.delete(
    delete_all=True,
    namespace=PINECONE_NAMESPACE,
)
```

is useful when rebuilding the index, but be careful when running the notebook against a namespace containing data you want to keep.

## 💬 Example Questions

After the RAG pipeline is initialized, try questions such as:

```text
What is the name of the company where I completed my internship?
```

```text
What were the test accuracies of the different models
compared in the Emotion Prediction project?
```

```text
What are the two main projects described in the internship report?
```

```text
What techniques were used in the content-based movie recommendation system?
```

```text
What were the major missing-value issues in the movie dataset?
```

You can also ask questions about charts and other visual information.

## 🔍 How Retrieval Works

The project uses:

```text
User Question
     ↓
Pinecone Retriever
     ↓
Relevant Document Chunks
     ↓
Context Construction
     ↓
Groq LLM
     ↓
Answer
```

For visual retrieval, relevant images are passed to the vision model along with the retrieved context.

## 📚 What I Learned

This project helped me gain practical experience with:

- Retrieval-Augmented Generation
- Document ingestion
- PDF parsing
- Table extraction
- Image and chart processing
- Multimodal LLMs
- Text embeddings
- Semantic search
- Vector databases
- Pinecone
- LangChain
- Prompt engineering
- API-based LLM integration
- Debugging retrieval quality

One of the biggest lessons from this project was that a RAG application is not only about connecting an LLM to a vector database. Document structure, chunking, metadata, retrieval quality, context selection, and visual information can all affect the final response.

## 🔮 Future Improvements

- Improve table-aware retrieval
- Add better chunking strategies
- Add metadata-aware filtering
- Add reranking
- Improve retrieval of charts and tables
- Add a Streamlit or web interface
- Add conversation history
- Add evaluation metrics for retrieval and answer quality
- Support multiple PDF documents
- Add hybrid/keyword retrieval

## ⚠️ Limitations

- Retrieval quality depends on document chunking and embeddings.
- Vision-model usage can increase API usage and latency.
- Pinecone and Groq require API credentials.
- The current notebook is designed as a learning/portfolio project rather than a production deployment.

## 👨‍💻 Author

**Ravi Bhamniya**

B.Tech — Artificial Intelligence and Data Science

This project represents my first practical implementation of a multimodal RAG pipeline.

## ⭐ If You Find This Project Useful

Feel free to explore the notebook, experiment with your own PDF documents, and improve the retrieval pipeline.
