# James AI Engineering Lab

James AI Engineering Lab is a personal AI engineering lab focused on
building reusable **self-hosted AI infrastructure** and real-world AI
systems across robotics and embedded engineering.

The goal is simple:

> **Build AI infrastructure once, then reuse it across domain
> applications.**

Applications use a reusable agent platform and standardized inference
services while remaining independent from concrete agent runtimes, model
implementations, GPU runtime details, and model-serving frameworks.

![James AI Engineering Lab
Architecture](assets/james-ai-engineering-lab-architecture.png)

## Focus Areas

### AI Agent Platform

Building a reusable, domain-agnostic **agent-service** for agent runtime
abstraction, orchestration, and **A2A agent-pool interoperability**.

The platform is designed to connect both **internal agent runtimes** and
**external A2A agents**, while domain knowledge, tools, policies, and
execution remain in the corresponding applications or adapters.

### MCP & Tool Integration

Building a reusable, domain-agnostic **mcp-service** based on the **Model Context Protocol (MCP)** so AI agents can connect to tools, data, and external systems through a standard interface.

This keeps Robot, BMC, Finance, and future domain capabilities independent from the shared agent platform.

### AI Infrastructure

Designing reusable, containerized, **self-hosted model-serving
infrastructure** for text, vision, embedding, audio, and future
multimodal workloads.

The current runtime uses **NVIDIA Triton Inference Server** on
self-managed GPU resources, with **KServe V2 / Open Inference Protocol**
as the standardized model inference boundary.

Model execution uses locally managed model weights and infrastructure.
The current architecture does **not** depend on a cloud-hosted or CSP
inference API.

### Robotics & Embodied AI

Building ROS 2 and NVIDIA Isaac Sim based robot systems that connect
perception, AI reasoning, decision making, skills, and robot operation
through clean runtime boundaries.

### Embedded / BMC AI

Applying AI agents and automation to embedded and BMC engineering
workflows, including engineering data processing, issue analysis, and
productivity tooling.

## Core Platforms

### `agent-service`

General-purpose, domain-agnostic AI agent platform shared by domain
applications.

Public architecture:

``` text
Domain Applications / Adapters
            |
            v
      agent-service
   Agent Runtime / Orchestration
          A2A Agent Pool
        /              \
       v                v
Internal Agents    External A2A Agents
        \              /
         +------+-------+
                |
                v
        inference-server
```

Key capabilities:

-   Runtime-agnostic agent execution
-   Generic agent orchestration
-   Internal agent runtime integration
-   External agent interoperability through A2A
-   A2A agent-pool foundation
-   Standardized model-provider boundary
-   Domain-independent contracts
-   Reusable across robotics, embedded/BMC, and future applications

The public profile intentionally presents the **platform capabilities
and boundaries** without exposing internal routing, delegation,
runtime-selection, prompt, or agent-pool implementation details.

### `mcp-service`

Generic MCP service shared by agent-based applications.

Its role is simple: provide a standard bridge between AI agents and reusable tools, data, and domain capabilities without putting domain-specific implementation inside the common AI platform.

### `inference-server`

General-purpose, self-hosted AI inference infrastructure shared by
multiple applications and agent runtimes.

Current architecture:

``` text
agent-service / Inference Clients
            |
            | KServe V2 / Open Inference Protocol
            v
NVIDIA Triton Inference Server
            |
            v
Inference Service / Core
            |
            v
Configuration-driven Backend
            |
            v
Self-managed GPU + Local Model Store
```

Key design principles:

-   Self-hosted inference on locally managed GPU resources
-   Application-independent model serving
-   Containerized runtime environment
-   KServe V2 / Open Inference Protocol compatible interfaces
-   NVIDIA Triton based serving runtime
-   Configuration-driven model and backend selection
-   Local/private model storage independent from Git
-   Support for text, vision, embedding, audio, and future multimodal
    capabilities
-   No dependency on a cloud-hosted/CSP inference API for the current
    runtime

The first validated runtime path is:

``` text
agent-service
    -> KServe V2
    -> NVIDIA Triton
    -> text_generation
    -> Inference Service / Core
    -> Config-driven Meta Llama backend
    -> Meta-Llama-3-8B-Instruct
    -> Self-managed NVIDIA GPU
```

## Projects

  -----------------------------------------------------------------------
  Repository                          Purpose
  ----------------------------------- -----------------------------------
  `agent-service`                     Generic AI agent runtime,
                                      orchestration, and A2A agent-pool
                                      platform

  `mcp-service`                       Generic MCP service for reusable
                                      tools, data, and capability access

  `inference-server`                  Self-hosted AI model serving and
                                      reusable inference infrastructure

  `ai-robot-system`                   ROS 2 based robot intelligence and
                                      runtime architecture

  `isaac-projects`                    NVIDIA Isaac Sim projects,
                                      simulation environments, and
                                      validation assets

  `bmc-job-agent`                     AI-assisted BMC engineering
                                      workflow and automation

  `finance-agent`                     Future AI-assisted financial and
                                      investment analysis
  -----------------------------------------------------------------------

## System Architecture

``` text
                           Domain Applications
               +------------------+------------------+
               |                  |                  |
               v                  v                  v
        AI Robot System      BMC Job Agent     Future Applications
               |                  |                  |
               +------------------+------------------+
                                  |
                                  v
                         agent-service
                  Agent Runtime / A2A Platform
                         /           \
                        /             \
                       v               v
                mcp-service      inference-server
               Tools / Data       Model Serving
                    |                  |
                    v                  v
             Domain Systems      Self-Hosted GPU
             & Capabilities       + Model Store
```

This public architecture intentionally shows **what the platform does
and how the major systems relate**, while keeping internal agent-pool
routing, delegation, runtime selection, prompt design, and execution
mechanics private.

For robotics:

``` text
Natural Language / Sensors
          |
          v
    ai-robot-system
          |
          v
     agent-service
 Agent Runtime / A2A Pool
          |
          | KServe V2
          v
    inference-server
          |
          v
     AI Model Execution
          |
          v
    ai-robot-system
 Decision / Skills / Operation
          |
          v
      isaac-projects
 Simulation / Validation
```

## Architecture Principles

1.  **Separate domain execution from agent infrastructure**\
    Robot, BMC, FinTech, and future applications own domain context,
    capabilities, policies, and execution.

2.  **Keep the agent platform domain-agnostic**\
    `agent-service` owns generic contracts, orchestration, runtime
    abstraction, and agent interoperability.

3.  **Support internal and external agent pools**\
    Internal runtimes and external agents can participate through a
    common agent platform, with A2A used for agent-to-agent
    interoperability.

4.  **Use MCP for reusable tool and data integration**\
    `mcp-service` provides a standard bridge from agents to tools, data,
    and domain capabilities.

5.  **Separate agent orchestration from model serving**\
    `agent-service` handles agent behavior; `inference-server` handles
    model execution.

6.  **Standardize the inference boundary**\
    KServe V2 / Open Inference Protocol keeps model clients independent
    from concrete model implementations.

7.  **Keep model selection configuration-driven**\
    Current and planned model-family direction includes Meta --- Llama,
    Google --- Gemma, and NVIDIA --- Cosmos.

8.  **Self-host model execution**\
    Models run on self-managed compute with locally managed model
    weights.

## Platform Boundaries

``` text
Domain Application / Adapter
    = context + capabilities + execution

agent-service
    = generic agent runtime + orchestration
      + internal/external A2A agent pool

mcp-service
    = MCP-based tools + data + capability access

inference-server
    = model execution + lifecycle + GPU serving

isaac-projects
    = simulation + digital-twin validation
```

Protocol boundaries:

``` text
Application / Adapter -> agent-service
    Generic application boundary

Agent <-> Agent
    A2A

agent-service -> mcp-service
    MCP (Model Context Protocol)

agent-service -> inference-server
    KServe V2 / Open Inference Protocol

mcp-service -> Tool / Data / Domain System
    Domain-owned capability integration
```

## Repository Boundaries

The repositories are validated together through system milestones but
are designed to evolve as independently maintained platform components:

``` text
agent-service
    generic agent runtime and A2A interoperability platform

mcp-service
    reusable MCP tool and capability integration

inference-server
    reusable AI inference infrastructure

ai-robot-system
    ROS 2 robot application/runtime stack

isaac-projects
    simulation assets and Isaac integration
```

Stable interfaces allow each component to be versioned and maintained
independently.

## Technology Direction

`Docker` · `AI Agent Runtime` · `A2A` · `MCP` · `Agent Pool` · `NVIDIA Triton` ·
`KServe V2` · `Open Inference Protocol` · `Self-Hosted GPU Inference` ·
`ROS 2` · `NVIDIA Isaac Sim` · `Meta Llama` · `Computer Vision`
