<div align="center">
  <img src="../images/owl-front.png" alt="Chapter Guide" width="100"/>

  # Chapter 3: Building Multimodal Agents with ADK
</div>

---

## Overview

This chapter introduces the **Agent Development Kit (ADK)**, Google's open-source framework for building production-grade AI agents. Through hands-on examples using a **SmartHome Customer Support** theme, you'll learn:

- **Agent Fundamentals**: Creating agents with instructions, tools, and multi-agent delegation
- **State Management**: Persisting data across sessions with temp, user, and app scopes
- **Semantic Memory**: Using Vertex AI Memory Bank for cross-session personalization
- **Multimodal Capabilities**: Processing images and generating artifacts
- **Real-Time Streaming**: Building voice-enabled agents with the Live API
- **Security & Guardrails**: Implementing callbacks and plugins for enterprise compliance

## Learning Objectives

Upon completing this chapter's exercises, you will be able to:

- Create and run ADK agents with `adk web`
- Build custom tools that extend agent capabilities
- Design multi-agent systems with specialist delegation
- Manage state across different scopes (temp, user, app)
- Integrate Vertex AI Memory Bank for semantic memory
- Process images and store artifacts
- Configure real-time streaming with the Live API
- Implement security callbacks and authorization patterns

## Agent Samples

All samples use a unified **SmartHome Customer Support** theme, matching the code examples from Chapter 3:

| # | Sample | Chapter Examples | Concept | Time |
|---|--------|-----------------|---------|------|
| 1 | [01_hello_agent](./01_hello_agent/) | 3-1, 3-2 | Basic Agent (7 lines) | ~5 min |
| 2 | [02_tool_agent](./02_tool_agent/) | 3-3 | Custom Function Tools | ~10 min |
| 3 | [03_multi_agent](./03_multi_agent/) | 3-5, 3-7 | Hybrid Architecture (tools + subagents) | ~10 min |
| 4 | [04_stateful_agent](./04_stateful_agent/) | 3-8, 3-10 | State Management Scopes | ~15 min |
| 5 | [05_memory_agent](./05_memory_agent/) | 3-14, 3-15, 3-16 | Vertex AI Memory Bank | ~15 min |
| 6 | [06_multimodal_agent](./06_multimodal_agent/) | 3-17, 3-19 | Image Analysis & Artifacts | ~10 min |
| 7 | [07_streaming_agent](./07_streaming_agent/) | 3-18 | Live API Voice Streaming | ~15 min |
| 8 | [08_guardrails_agent](./08_guardrails_agent/) | 3-20, 3-21, 3-22 | Callbacks & LongRunningFunctionTool | ~10 min |

## Prerequisites

### 1. Python Environment
```bash
# Python 3.10+ required
python --version

# Create virtual environment (recommended)
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# or: .venv\Scripts\activate  # Windows
```

### 2. Install ADK
```bash
pip install google-adk google-genai python-dotenv
```

### 3. Get API Key
- Visit [Google AI Studio](https://aistudio.google.com/apikey) to get a Gemini API key
- Or use Vertex AI with application default credentials

### 4. Configure Environment
Each agent folder has a `.env.example` file:
```bash
cd 01_hello_agent
cp .env.example .env
# Edit .env and add your GOOGLE_API_KEY
```

## Quick Start

1. **Navigate to any agent folder**:
   ```bash
   cd chapter-3/01_hello_agent
   ```

2. **Set up your API key**:
   ```bash
   cp .env.example .env
   # Edit .env with your key
   ```

3. **Run with ADK Web**:
   ```bash
   adk web .
   ```

4. **Open browser**: Navigate to http://localhost:8000

## Directory Structure

```
chapter-3/
├── README.md                    # This file
├── 01_hello_agent/              # Example 3-1: Basic agent
├── 02_tool_agent/               # Example 3-3: Order lookup tools
├── 03_multi_agent/              # Example 3-7: Hybrid architecture
├── 04_stateful_agent/           # Example 3-10: State with cart expiry
├── 05_memory_agent/             # Example 3-16: Memory placeholder
├── 06_multimodal_agent/         # Example 3-17: Artifact storage
├── 07_streaming_agent/          # Example 3-18: Live API
├── 08_guardrails_agent/         # Example 3-20: LongRunningFunctionTool
└── .plan/
    └── IMPLEMENTATION_PLAN.md   # Development notes
```

## Key ADK Concepts

### Agent Definition (Example 3-1)
```python
from google.adk.agents import Agent

root_agent = Agent(
    name="CustomerSupportAgent",
    model="gemini-2.5-flash",
    instruction="You help customers with their SmartHome products.",
    tools=[...],           # Function tools
    sub_agents=[...]       # Specialist agents
)
```

### Running Agents (Example 3-2)
```bash
# Interactive web interface
adk web .

# Command-line interface
adk run .

# API server for integration
adk api_server .
```

### State Scopes (Examples 3-8, 3-10)
- `temp:key` - Session-only, disappears after conversation ends
- `user:key` - Cross-session, persists for specific user
- `app:key` - System-wide, shared across all users (e.g., `app:saved_cart_expiry_days`)

### Memory Placeholder (Example 3-16)
```python
instruction="""...
Before responding, review these memories:
---
Retrieved Memories:
{memory}
---
Use this context to provide personalized support..."""
```

## Troubleshooting

### API Key Issues
```bash
# Verify your key is set
echo $GOOGLE_API_KEY

# Test with a simple request
python -c "from google import genai; client = genai.Client(); print(client.models.list())"
```

### ADK Not Found
```bash
# Ensure ADK is installed
pip install --upgrade google-adk

# Verify installation
adk --version
```

### Port Already in Use
```bash
# Use a different port
adk web . --port 8080
```

## Related Resources

- [ADK Documentation](https://google.github.io/adk-docs/)
- [ADK Python GitHub](https://github.com/google/adk-python)
- [ADK Samples Repository](https://github.com/google/adk-samples)
- [Vertex AI Agent Engine](https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/overview)

---

[← Previous Chapter](../chapter-2/) | [Home](../) | [Next Chapter →](../chapter-4/)
