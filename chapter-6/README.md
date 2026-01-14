<div align="center">
  <img src="../images/owl-front.png" alt="Chapter Guide" width="100"/>
  
  # Chapter 6: Tuning and Infrastructure
</div>

---

## Chapter Overview

This chapter covers fine-tuning strategies for large language models and building scalable inference infrastructure on Google Cloud. You'll learn how to efficiently adapt pre-trained models like Gemma to your specific use cases using techniques like QLoRA, and understand deployment options for production workloads.

## Learning Objectives

Upon completing this chapter, you will be able to:

- **Apply the fine-tuning decision framework** - Determine when to fine-tune vs. use prompt engineering or RAG
- **Configure QLoRA for efficient fine-tuning** - Use 4-bit quantization with LoRA adapters to fine-tune large models on consumer GPUs
- **Deploy from Model Garden** - Select and deploy open-source models from Vertex AI Model Garden
- **Diagnose infrastructure bottlenecks** - Identify bandwidth, memory, compute, and network constraints
- **Optimize serving with vLLM** - Leverage PagedAttention and continuous batching for 2-3x throughput gains
- **Choose deployment platforms** - Select between Agent Engine, Cloud Run, GKE, and Vertex AI Prediction

## Key Concepts

### Fine-Tuning Decision Framework

| Criterion | When to Fine-Tune | Alternative |
|-----------|------------------|-------------|
| **Reasoning & Intuition** | Model must synthesize, not just retrieve | Tool integration (MCP) |
| **Domain Specialization** | Niche vocabulary poorly represented | RAG with domain docs |
| **Style & Consistency** | Specific communication patterns required | Few-shot prompting |
| **Response Latency** | Edge/real-time requirements | Model distillation |
| **Economics at Scale** | High-volume with retrieval costs | Continue with RAG |

### Infrastructure Bottleneck Patterns

| Pattern | Symptom | Solution |
|---------|---------|----------|
| **Pattern 1: Bandwidth** | GPU utilization 50-70% | Increase memory utilization, enable prefix caching |
| **Pattern 2: Memory** | OOM errors, model doesn't fit | Quantization, tensor parallelism |
| **Pattern 3: Compute** | Sustained 100% utilization | Faster hardware, horizontal scaling |
| **Pattern 4: Network** | Multi-GPU scaling issues | Minimize parallelism, use NVLink |

### QLoRA (Quantized Low-Rank Adaptation)

| Component | Description |
|-----------|-------------|
| **4-bit Quantization** | Reduces model memory footprint using NF4 data type |
| **LoRA Adapters** | Trains small adapter layers instead of full model weights |
| **Efficient Training** | Enables fine-tuning 7B+ models on T4/L4 GPUs |

### Fine-Tuning Workflow

```
┌─────────────────────────────────────────────────────────┐
│                 FINE-TUNING PIPELINE                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐ │
│  │  Load Base  │→ │  Configure  │→ │  Train LoRA     │ │
│  │  Model      │  │  QLoRA      │  │  Adapters       │ │
│  └─────────────┘  └─────────────┘  └─────────────────┘ │
│          ↓                                   ↓         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐ │
│  │  Deploy to  │← │  Merge      │← │  Evaluate       │ │
│  │  GCS/Vertex │  │  Adapter    │  │  Performance    │ │
│  └─────────────┘  └─────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

## Contents

### Colab Notebooks

| Notebook | Description |
|----------|-------------|
| [`01_gemma_finetuning.ipynb`](colabs/01_gemma_finetuning.ipynb) | Fine-tune Gemma 7B with QLoRA for financial analysis |
| [`02_model_garden_deployment.ipynb`](colabs/02_model_garden_deployment.ipynb) | Deploy models from Vertex AI Model Garden |
| [`03_vllm_serving.ipynb`](colabs/03_vllm_serving.ipynb) | Efficient LLM serving with vLLM and PagedAttention |

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ayoisio/genai-on-google-cloud/blob/main/chapter-6/colabs/01_gemma_finetuning.ipynb) Fine-tuning
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ayoisio/genai-on-google-cloud/blob/main/chapter-6/colabs/02_model_garden_deployment.ipynb) Model Garden
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ayoisio/genai-on-google-cloud/blob/main/chapter-6/colabs/03_vllm_serving.ipynb) vLLM Serving

### 📚 Official Google Cloud Model Garden Notebooks

> **Recommended**: For production use cases and the latest model support, we strongly encourage using the official Google Cloud Platform notebook samples. These are actively maintained and cover a wide range of models and deployment scenarios.

**🔗 [GoogleCloudPlatform/vertex-ai-samples - Model Garden Notebooks](https://github.com/GoogleCloudPlatform/vertex-ai-samples/tree/main/notebooks/community/model_garden)**

| Official Notebook | Use Case |
|-------------------|----------|
| [`model_garden_gemma2_deployment_on_vertex.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/model_garden/model_garden_gemma2_deployment_on_vertex.ipynb) | Deploy Gemma 2 on Vertex AI |
| [`model_garden_gemma_finetuning_on_vertex.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/model_garden/model_garden_gemma_finetuning_on_vertex.ipynb) | Fine-tune Gemma on Vertex AI |
| [`model_garden_gemma_deployment_on_gke.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/model_garden/model_garden_gemma_deployment_on_gke.ipynb) | Deploy Gemma on GKE |
| [`model_garden_vllm_text_only_tutorial.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/model_garden/model_garden_vllm_text_only_tutorial.ipynb) | vLLM deployment tutorial |
| [`model_garden_huggingface_vllm_deployment.ipynb`](https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/model_garden/model_garden_huggingface_vllm_deployment.ipynb) | HuggingFace + vLLM deployment |

The official repository includes notebooks for Llama, Mistral, DeepSeek, Qwen, and many other models.

## Setup Instructions

1. Open the notebook in Google Colab using the badge above
2. Enable GPU runtime: **Runtime > Change runtime type > GPU** (T4, L4, or A100)
3. Authenticate with Google Cloud when prompted
4. Set up HuggingFace access for Gemma model weights:
   - Create account at [huggingface.co](https://huggingface.co/join)
   - Accept Gemma license at [huggingface.co/google/gemma-7b-it](https://huggingface.co/google/gemma-7b-it)
   - Create access token at [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
5. Prepare your training dataset (`dataset.jsonl`)

## Prerequisites

- Completion of Chapters 1-5 (recommended)
- Google Cloud project with billing enabled
- Vertex AI API enabled
- HuggingFace account with Gemma model access
- Basic understanding of transformers and fine-tuning concepts

## Deployment Options

| Option | Use Case | Cost | Complexity |
|--------|----------|------|------------|
| **Agent Engine** | Production agents, minimal ops | Managed pricing | Low |
| **Cloud Run** | Variable traffic, serverless | Pay per request | Medium |
| **GKE** | Complex requirements, max control | ~$0.50-$1.00/hour | High |
| **Vertex AI Prediction** | Enterprise, SLA requirements | ~$0.50-$1.00/hour (T4 GPU) | Medium |

## Learning Resources

### Hands-on Code Examples
- [Fine-tune Gemma with Keras and LoRA](https://ai.google.dev/gemma/docs/lora_tuning)
- [Vertex AI Model Garden Community Notebooks](https://github.com/GoogleCloudPlatform/vertex-ai-samples/tree/main/notebooks/community/model_garden)
- [Serving Gemma with vLLM on Cloud Run](https://codelabs.developers.google.com/)
- [Serve Gemma on GKE with vLLM](https://cloud.google.com/kubernetes-engine/docs/tutorials/serve-gemma-gpu-vllm)

### Video Tutorials
- [Optimize model serving with GKE Inference Gateway](https://www.youtube.com/watch?v=example)
- [Scaling AI with Google Cloud's TPUs](https://www.youtube.com/watch?v=example)
- [AI workload orchestration options](https://www.youtube.com/watch?v=example)

## Need Help?

<img src="../images/owl-right.png" alt="Helper Owl" width="80" align="left"/>

<br/>

If you encounter issues with the notebooks, check the error messages carefully and ensure:
- GPU runtime is enabled in Colab
- All required APIs are enabled in your GCP project
- HuggingFace authentication is complete with Gemma access approved

<br clear="left"/>

## What's Next

In **Chapter 7**, you'll learn about monitoring, observability, and maintaining LLM applications in production.

---

[← Previous Chapter: Building a Chatbot](../chapter-5/) | [Home](../) | [Next Chapter: Monitoring & Observability →](../chapter-7/)
