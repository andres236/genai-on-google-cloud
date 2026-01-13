<div align="center">
  <img src="../images/owl-front.png" alt="Chapter Guide" width="100"/>

  # Chapter 4: Orchestrating Intelligent Agent Teams
</div>

---

**Estimated Time**: 2-3 hours
**Prerequisites**: Complete Chapter 3 (Building Multimodal Agents with ADK)

## Overview

This chapter teaches you how to build **multiagent systems** using Google's Agent Development Kit (ADK). You'll learn to orchestrate teams of specialized agents that collaborate to solve complex, multi-domain problems.

The journey progresses from **local orchestration** (workflow agents running in the same process) to **distributed collaboration** (agents communicating across network boundaries via A2A and MCP protocols) to **production patterns** (security, tracing, and versioning).

## The Problem: Monolithic Agents Don't Scale

Consider this customer query:

> *"My new smart thermostat is showing a wiring error, and I think it might have overcharged my last bill. Can you check if it's eligible for a warranty replacement and fix the billing issue?"*

This single query requires expertise from **three domains**: technical diagnostics, warranty policy, and billing reconciliation. A monolithic agent trying to handle all three creates:

- **Conflicting instructions** (diagnostic mindset vs. financial compliance mindset)
- **Tool selection paralysis** (50+ tools to choose from)
- **Maintenance nightmares** (every change affects the entire system)

## The Solution: Agent Teams

Instead of one agent doing everything, build a **team of specialists**:

```
                    +-----------------------------+
                    |  CustomerServiceCoordinator |
                    | (Routes queries to specialists) |
                    +-------------+---------------+
                                  |
          +-----------------------+-----------------------+
          |                       |                       |
          v                       v                       v
   +-------------+        +-------------+        +-------------+
   | Technical   |        |  Warranty   |        |  Billing    |
   | Specialist  |        |  Specialist |        |  Specialist |
   +-------------+        +-------------+        +-------------+
```

## Learning Path

| # | Sample | Pattern | What You'll Learn |
|---|--------|---------|-------------------|
| 1 | `01_sequential_agent/` | SequentialAgent | Ordered workflows, output_key, state passing |
| 2 | `02_parallel_agent/` | ParallelAgent | Concurrent execution, synthesis patterns |
| 3 | `03_loop_agent/` | LoopAgent | Iterative refinement, escalate termination |
| 4 | `04_mcp_agent/` | MCP Integration | External tool access via McpToolset |
| 5 | `05_a2a_server/` | A2A Server | Exposing agents via to_a2a() |
| 6 | `06_a2a_client/` | A2A Client | Consuming remote agents via RemoteA2aAgent |
| 7 | `07_hybrid_team/` | Hybrid Architecture | Local + Remote A2A + MCP combined |
| 8 | `08_production_agent/` | Production Patterns | Security, tracing, versioning |

## Prerequisites

Before starting, ensure you have:

- **Python 3.10+** installed
- **Google Cloud account** with Gemini API access
- **ADK installed**: `pip install google-adk`
- **Additional dependencies**:
  ```bash
  pip install uvicorn mcp python-dotenv
  ```

## Quick Start

1. **Set up API key**:
   ```bash
   export GOOGLE_API_KEY=your-api-key-here
   ```

2. **Run any sample**:
   ```bash
   cd chapter-4/01_sequential_agent
   adk web .
   ```

3. **Open browser**: Navigate to http://localhost:8000

## Key Concepts

### Workflow Agents (Local Orchestration)

ADK provides three workflow agents for deterministic orchestration:

| Agent | Pattern | When to Use |
|-------|---------|-------------|
| `SequentialAgent` | Assembly Line | Tasks with dependencies (A must complete before B) |
| `ParallelAgent` | Independent Taskforce | Independent tasks that can run concurrently |
| `LoopAgent` | Iterative Refiner | Trial-and-error, progressive refinement |

### Communication Patterns

**State Passing with output_key**:
```python
agent_a = Agent(
    name="AgentA",
    output_key="result_a",  # Saves output to state["result_a"]
    instruction="Analyze the issue..."
)

agent_b = Agent(
    name="AgentB",
    instruction="Use {result_a} to..."  # Reads from state["result_a"]
)
```

### Distributed Protocols

| Protocol | Purpose | When to Use |
|----------|---------|-------------|
| **MCP** | Tool access | Stateless operations (database queries, API calls) |
| **A2A** | Agent delegation | Stateful, conversational delegation to specialists |

## Sample Dependencies

Some samples work together:

```
+----------------+     +----------------+     +----------------+
| 05_a2a_server  |---->| 06_a2a_client  |---->| 07_hybrid_team |
| (BillingAgent) |     | (Coordinator)  |     | (Full System)  |
+----------------+     +----------------+     +----------------+
        |                                             |
        +---------------------------------------------+

+----------------+
| 04_mcp_agent   |----> Built-in MCP server (mcp_server.py)
| (SupportAgent) |
+----------------+
```

To run the full A2A demo:
```bash
# Terminal 1: Start A2A server
cd 05_a2a_server && uvicorn agent:a2a_app --port 8001

# Terminal 2: Run A2A client
cd 06_a2a_client && adk web .
```

## Chapter Examples Reference

Each sample implements specific examples from the chapter text:

| Sample | Chapter Examples | Description |
|--------|------------------|-------------|
| 01_sequential_agent | Example 4-5 | SequentialAgent workflow |
| 02_parallel_agent | Example 4-6 | ParallelAgent workflow |
| 03_loop_agent | Examples 4-7, 4-8 | LoopAgent with escalate |
| 04_mcp_agent | Example 4-9 | MCPToolset integration |
| 05_a2a_server | Examples 4-10, 4-11 | to_a2a() and uvicorn |
| 06_a2a_client | Example 4-12 | RemoteA2aAgent |
| 07_hybrid_team | Examples 4-13, 4-14 | Full hybrid architecture |
| 08_production_agent | Examples 4-15 to 4-22 | Security, tracing, versioning |

## Next Steps

After completing this chapter:

1. **Chapter 5**: Evaluation and Optimization - Measure and improve agent quality

2. **External Codelabs**:
   - [Build and deploy an ADK agent with MCP on Cloud Run](https://codelabs.developers.google.com/codelabs/cloud-run/use-mcp-server-on-cloud-run-with-an-adk-agent)
   - [Build a Travel Agent using MCP Toolbox for Databases](https://codelabs.developers.google.com/travel-agent-mcp-toolbox-adk)
   - [The Summoner's Concord - Architecting multiagent systems](https://codelabs.developers.google.com/agentverse-architect/instructions)

## Related Resources

- [ADK Multi-Agent Documentation](https://google.github.io/adk-docs/agents/multi-agents/)
- [A2A Protocol Specification](https://a2a-protocol.org/latest/)
- [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro)
- [MCP Toolbox for Databases](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)

---

[<- Previous Chapter](../chapter-3/) | [Home](../) | [Next Chapter ->](../chapter-5/)
