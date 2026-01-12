<div align="center">
  <img src="../images/owl-front.png" alt="Chapter Guide" width="100"/>

  # Chapter 2: Data Readiness and Accessibility
</div>

---

## Overview

This chapter covers the foundational data readiness concepts required for building production-grade LLM applications on Google Cloud:

- **Data Readiness Dimensions**: Discoverability, accessibility, quality, format, governance
- **Unified Data Platform**: BigQuery, Cloud Storage, Vertex AI, Dataplex
- **Document Processing**: Extracting structured data from unstructured documents
- **RAG Patterns**: From Naive RAG to Advanced RAG architectures
- **Vector Search**: Embeddings, indexing, and semantic search
- **Security & Governance**: DLP, access controls, and data protection

## Learning Objectives

Upon completing this chapter's exercises, you will be able to:
- Explore and assess data quality using BigQuery
- Process unstructured documents with Document AI
- Generate text embeddings and perform semantic search
- Build a complete RAG pipeline with context assembly
- Use Vertex AI RAG Engine for managed retrieval-augmented generation

### Learning Path

```mermaid
flowchart LR
    A[Data Exploration] --> B[Document Processing]
    B --> C[Embeddings]
    C --> D[RAG Pipeline]
    D --> E[RAG Engine]
```

## Notebooks

| # | Notebook | Description | Time |
|---|----------|-------------|------|
| 1 | [01_data_exploration_bigquery.ipynb](colabs/01_data_exploration_bigquery.ipynb) | Data discovery and quality assessment with BigQuery | ~10 min |
| 2 | [02_document_processing.ipynb](colabs/02_document_processing.ipynb) | Document AI and structured extraction | ~15 min |
| 3 | [03_embeddings_vector_search.ipynb](colabs/03_embeddings_vector_search.ipynb) | Text embeddings and semantic search | ~15 min |
| 4 | [04_rag_context_assembly.ipynb](colabs/04_rag_context_assembly.ipynb) | Building a complete RAG pipeline | ~15 min |
| 5 | [05_vertex_ai_rag_engine.ipynb](colabs/05_vertex_ai_rag_engine.ipynb) | Managed RAG with Vertex AI RAG Engine | ~15 min |

## Prerequisites

Before running the notebooks, ensure you have:

### 1. Google Cloud Project
- A Google Cloud project with billing enabled
- Project ID noted for configuration

### 2. Enable Required APIs
```bash
gcloud services enable \
    aiplatform.googleapis.com \
    bigquery.googleapis.com \
    bigquerystorage.googleapis.com \
    storage.googleapis.com \
    documentai.googleapis.com
```

### 3. IAM Permissions
Ensure your account has the following roles:
- `roles/bigquery.dataEditor`
- `roles/bigquery.jobUser`
- `roles/aiplatform.user`
- `roles/storage.objectViewer`

## Quick Start

1. **Open in Colab**: Click the "Open in Colab" badge at the top of each notebook
2. **Authenticate**: Run the authentication cell when prompted
3. **Set Project ID**: Enter your Google Cloud project ID in the configuration cell
4. **Run All Cells**: Execute cells in order from top to bottom

## Directory Structure

```
chapter-2/
├── README.md                 # This file
├── colabs/                   # Jupyter notebooks
│   ├── 01_data_exploration_bigquery.ipynb
│   ├── 02_document_processing.ipynb
│   ├── 03_embeddings_vector_search.ipynb
│   ├── 04_rag_context_assembly.ipynb
│   └── 05_vertex_ai_rag_engine.ipynb
├── data/
│   └── sample_documents/     # Sample files for exercises
└── solutions/                # Exercise solutions
```

## Sample Data

The notebooks use two data sources:

1. **BigQuery Public Datasets**: Pre-loaded datasets for immediate experimentation
   - `bigquery-public-data.samples.wikipedia`
   - `bigquery-public-data.patents_view.patent`

2. **Sample Documents**: Located in `data/sample_documents/` for document processing exercises

## Troubleshooting

### Authentication Issues
```python
# If authentication fails, try:
from google.colab import auth
auth.authenticate_user()
```

### Quota Errors
- Check your project quotas in the Cloud Console
- Consider using a different region if `us-central1` is at capacity

### Permission Denied
- Verify API is enabled: `gcloud services list --enabled`
- Check IAM roles: `gcloud projects get-iam-policy PROJECT_ID`

## Related Resources

- [Vertex AI Documentation](https://cloud.google.com/vertex-ai/docs)
- [BigQuery ML Documentation](https://cloud.google.com/bigquery/docs/bqml-introduction)
- [RAG Engine Documentation](https://cloud.google.com/vertex-ai/generative-ai/docs/rag-overview)

---

[← Previous Chapter](../chapter-1/) | [Home](../) | [Next Chapter →](../chapter-3/)
