# Chapter 5: Evaluation and Optimization Strategies

This chapter explores practical approaches for evaluating and optimizing LLM applications and AI agents. You can't meaningfully improve what you can't measure, and you can't know if your optimizations are effective without robust evaluation methods.

## Overview

| Dimension | What to Measure | Key Metrics |
|-----------|-----------------|-------------|
| **Quality of Output** | Accuracy, fluency, coherence, relevance | ROUGE, BLEU, BERTScore, LLM-as-judge |
| **Task Success** | Goal achievement, efficiency | Tool call accuracy, trajectory metrics |
| **System Performance** | Latency, throughput, cost | Response time, tokens/sec, $/query |
| **Robustness** | Edge case handling, error recovery | Error rate, degradation under load |
| **Safety** | Harm avoidance, bias detection | Safety scores, red team findings |

## Code Samples

This chapter includes two hands-on ADK agent evaluation samples:

| Sample | Topic | Chapter Examples |
|--------|-------|------------------|
| [01_agent_eval/](./01_agent_eval/) | Agent tool usage and trajectory evaluation | Example 5-2 |
| [02_custom_eval/](./02_custom_eval/) | Custom metrics: LLM-as-judge + computation-based | Examples 5-3, 5-4 |

## Quick Start

```bash
# Navigate to a sample
cd chapter-5/01_agent_eval

# Set up environment
cp .env.example .env
# Edit .env with your GOOGLE_API_KEY

# Run evaluation
python run_eval.py
```

---

## Evaluation Strategies

### 1. Human-Centered Evaluation

Human evaluation is essential for assessing nuanced qualities like helpfulness, tone appropriateness, and subtle safety concerns.

#### Rubric-Based Assessment

A well-designed rubric defines specific dimensions with clear criteria for each score level:

| Score | Criteria |
|-------|----------|
| 5 (Exceptional) | Fully addresses query with optimal detail, anticipates follow-up needs |
| 4 (Very Good) | Thoroughly addresses query with appropriate detail |
| 3 (Adequate) | Directly answers but provides minimal additional context |
| 2 (Subpar) | Partially addresses query, misses key elements |
| 1 (Poor) | Fails to address core query |

#### Preference Testing & A/B Testing

Rather than absolute scoring, preference testing presents pairs of responses for direct comparison:
- Higher inter-annotator agreement
- Directly mimics user decision-making
- Identifies whether changes improve quality

#### Red Teaming

Adversarial testing to probe for failures across multiple risk categories:
- **Prompt attacks** - Jailbreaking, instruction injection
- **Training data extraction** - Attempting to extract memorized data
- **Adversarial examples** - Inputs designed to cause failures
- **Exfiltration** - Attempts to leak sensitive information

**Tools**: [Argilla](https://argilla.io/), [Langfuse](https://langfuse.com/), [W&B Weave](https://wandb.ai/site/weave/)

---

### 2. Automated Evaluation Metrics

#### Reference-Based Metrics

Compare generated outputs against "gold standard" reference texts:

| Metric | What it Measures | Best For |
|--------|------------------|----------|
| **ROUGE** | N-gram recall overlap | Summarization |
| **BLEU** | N-gram precision | Translation |
| **BERTScore** | Semantic similarity via embeddings | General text quality |

**Limitations**: Penalizes valid alternative phrasings, struggles with creative tasks.

#### Domain-Specific Metrics

- **Exact Match (EM)** - For Q&A with definitive answers
- **F1 Score** - For information extraction/classification
- **Functional Correctness** - For code generation (does it run?)

---

### 3. Agent Evaluation Metrics

For agentic systems, evaluate both the *what* and the *how*:

#### Tool Usage Metrics

| Metric | Description |
|--------|-------------|
| `tool_call_valid` | Binary: Is the tool call syntactically correct? |
| `tool_name_match` | Did the agent select the appropriate tool? |
| `tool_parameter_key_match` | Are the parameter names correct? (0-1 proportion) |
| `tool_parameter_kv_match` | Are both parameter names AND values correct? |

#### Trajectory Metrics

| Metric | Description |
|--------|-------------|
| `trajectory_exact_match` | Does the action sequence precisely match the golden path? |
| `trajectory_in_order_match` | Correct sequence with possible additional steps |
| `trajectory_any_order_match` | Required actions performed in any order |
| `trajectory_precision` | Ratio of necessary to actual actions (avoid superfluous steps) |
| `trajectory_recall` | Ratio of required actions successfully completed |

See [01_agent_eval/](./01_agent_eval/) for a hands-on example.

---

### 4. Custom Metrics

When built-in metrics don't capture your specific quality criteria:

#### LLM-as-Judge (Example 5-3)

Use an LLM to evaluate responses against a custom rubric:

```python
from vertexai.evaluation import PointwiseMetricPromptTemplate

helpfulness_metric = PointwiseMetricPromptTemplate(
    name="helpfulness",
    prompt="""Evaluate the helpfulness of the response.
    Query: {query}
    Response: {response}

    Rate: 1 (not helpful), 0.5 (partial), 0 (fully helpful)""",
    rubric={"1": "Fails to address needs", "0.5": "Partial", "0": "Fully helpful"}
)
```

#### Computation-Based Metrics (Example 5-4)

For precise business logic validation:

```python
def portfolio_compliance_metric(response: dict, context: dict) -> dict:
    """Check if recommendation meets compliance rules."""
    violations = []

    if response.get("risk_score", 0) > context.get("max_risk_tolerance", 7):
        violations.append("Risk score exceeds tolerance")

    return {"compliant": len(violations) == 0, "score": 1.0 if not violations else 0.0}
```

See [02_custom_eval/](./02_custom_eval/) for complete examples.

---

## Optimization Strategies

### Prompt Refinement

Effective patterns for improving prompts:
- **Explicit role and context** - Activate domain-specific knowledge
- **Task decomposition** - Break into explicit steps
- **Few-shot examples** - Concrete demonstrations
- **Output structuring** - Explicit response format
- **Thinking process guidance** - Direct reasoning approach

### Agent Performance Elevation

1. **Robust Tool Design**
   - Clear specifications with detailed descriptions
   - Input validation for graceful failure
   - Idempotent design (multiple calls = same result)
   - Informative error messages

2. **Improved Reasoning**
   - Chain-of-Thought prompting
   - ReAct (Reasoning and Acting)
   - Tree of Thoughts for complex decisions
   - Dynamic planning with goal hierarchies

3. **Memory Systems**
   - Working memory for current task context
   - Episodic memory for past experiences
   - Reflection on tool outputs and approach

---

## Hands-On Notebooks

The Google Cloud Platform maintains comprehensive evaluation notebooks. These are actively updated and cover the Vertex AI Gen AI Evaluation SDK in depth.

### Getting Started

- [Quick Start: Gen AI Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/quick_start_gen_ai_eval.ipynb) - Introduction to the evaluation SDK with rubric-based assessment

### Agent Evaluation

- [Evaluating ADK Agents](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/evaluating_adk_agent.ipynb) - Agent Development Kit evaluation patterns
- [Create Agent and Run Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/create_agent_and_run_evaluation.ipynb) - End-to-end agent evaluation workflow
- [GenAI Agent Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/create_genai_agent_evaluation.ipynb) - GenAI-specific agent evaluation
- [Multi-Agent Evaluation with Arize](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/multi_agent_evals_with_arize_and_crewai.ipynb) - Evaluating multi-agent systems

### Multimodal Evaluation

- [Image Evaluation with Gecko](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/evaluate_images_with_predefined_gecko.ipynb) - Evaluating image understanding
- [Video Evaluation with Gecko](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/evaluate_videos_with_predefined_gecko.ipynb) - Evaluating video understanding

### Custom Metrics & Advanced Topics

- [evaltask_approach/](https://github.com/GoogleCloudPlatform/generative-ai/tree/main/gemini/evaluation/evaltask_approach) - 27 notebooks covering:
  - Custom metrics (bring your own computation-based or model-based)
  - RAG pipeline evaluation with batch processing
  - Tool use validation
  - Agent evaluation (CrewAI, LangGraph)
  - LangChain chain evaluation

### Model Comparison

- [Third-Party LLM Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/evaluating_third_party_llms_vertex_ai_gen_ai_eval_sdk.ipynb) - Comparing different LLMs
- [Model Migration Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/model_migration_with_gen_ai_eval.ipynb) - Evaluating model upgrades (e.g., PaLM to Gemini)
- [Structured Output Evaluation](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/evaluate_gemini_structured_output.ipynb) - Validating structured outputs

---

## Learning Resources

### Videos

- [How to Evaluate GenAI Models with Vertex AI](https://www.youtube.com/watch?v=example) - Overview of evaluation service
- [The Agent Evaluation Revolution](https://www.youtube.com/watch?v=example) - Deep dive into agent metrics
- [Evaluating and Debugging Non-Deterministic AI Agents](https://www.youtube.com/watch?v=example) - Practical debugging techniques

### Codelabs

- [Vertex AI Gen AI Evaluation Service SDK](https://codelabs.developers.google.com/vertex-ai-eval) - Hands-on evaluation walkthrough
- [BigQuery ADK Agent Evaluation Codelab](https://codelabs.developers.google.com/adk-eval) - Data warehouse agent evaluation
- [Zero-Shot Prompt Optimizer](https://codelabs.developers.google.com/prompt-optimizer) - Automated prompt improvement
- [Data-Driven Prompt Optimizer](https://codelabs.developers.google.com/data-prompt-optimizer) - Example-based optimization

---

## Prerequisites

```bash
pip install google-adk google-cloud-aiplatform vertexai python-dotenv
```

**Environment Variables:**
```bash
GOOGLE_API_KEY=your_api_key_here
# Or for Vertex AI:
GOOGLE_CLOUD_PROJECT=your_project_id
GOOGLE_CLOUD_LOCATION=us-central1
```

---

## Chapter Examples Mapping

| Example | Description | Location |
|---------|-------------|----------|
| 5-1 | Reference-based evaluation (ROUGE, BLEU) | [GCP Notebooks](#getting-started) |
| 5-2 | Agent tool usage evaluation | [01_agent_eval/](./01_agent_eval/) |
| 5-3 | Custom LLM-as-judge metric | [02_custom_eval/](./02_custom_eval/) |
| 5-4 | Custom computation-based metric | [02_custom_eval/](./02_custom_eval/) |

---

## Next Steps

After mastering evaluation:
- **Chapter 6**: Model adaptation (LoRA, QLoRA) and infrastructure optimization
- **Chapter 7**: MLOps for continuous improvement and production monitoring

---

[← Previous Chapter](../chapter-4/) | [Home](../) | [Next Chapter →](../chapter-6/)
