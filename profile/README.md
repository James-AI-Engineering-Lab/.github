# James AI Engineering Lab

James AI Engineering Lab is a personal AI engineering lab focused on building reusable **self-hosted AI infrastructure** and real-world AI systems across robotics and embedded engineering.

The goal is simple:

> **Build AI infrastructure once, then reuse it across domain applications.**

Applications consume stable inference capabilities and remain independent from concrete model implementations, GPU runtime details, and model-serving frameworks.

![James AI Engineering Lab Architecture](assets/james-ai-engineering-lab-architecture.png)

## Focus Areas

### AI Infrastructure

Designing reusable, containerized, **self-hosted model-serving infrastructure** for text, vision, embedding, audio, and future multimodal workloads.

The current runtime uses **NVIDIA Triton Inference Server** on self-managed GPU resources, with **KServe V2 / Open Inference Protocol** as the standardized application boundary.

Model execution uses locally managed model weights and infrastructure. The current architecture does **not** depend on a cloud-hosted or CSP inference API.

### Robotics & Embodied AI

Building ROS 2 and NVIDIA Isaac Sim based robot systems that connect perception, AI reasoning, decision making, skills, and robot operation through clean runtime boundaries.

The robot stack consumes AI capabilities through standardized inference interfaces rather than embedding model frameworks directly into ROS application packages.

### Embedded / BMC AI

Applying AI agents and automation to embedded and BMC engineering workflows, including engineering data processing, issue analysis, and productivity tooling.

## Core Infrastructure

### `inference-server`

General-purpose, self-hosted AI inference infrastructure shared by multiple applications.

Current architecture:

```text
Applications
    |
    | KServe V2 / Open Inference Protocol
    v
NVIDIA Triton Inference Server
    |
    v
Thin Triton Model Adapter
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

- Self-hosted inference on locally managed GPU resources
- Application-independent model serving
- Containerized runtime environment
- KServe V2 / Open Inference Protocol compatible interfaces
- NVIDIA Triton based serving runtime
- Configuration-driven model and backend selection
- Local/private model storage independent from Git
- Support for text, vision, embedding, audio, and future multimodal capabilities
- Applications depend on AI capabilities rather than specific model implementations
- No dependency on a cloud-hosted/CSP inference API for the current runtime

The first validated runtime path is:

```text
KServe V2 Client
    -> NVIDIA Triton
    -> text_generation
    -> Inference Service / Core
    -> Config-driven Meta Llama backend
    -> Meta-Llama-3-8B-Instruct
    -> Self-managed NVIDIA GPU
```

## Projects

| Repository | Purpose |
| --- | --- |
| `inference-server` | Self-hosted AI model serving and reusable inference infrastructure |
| `ai-robot-system` | ROS 2 based robot intelligence, orchestration, and runtime architecture |
| `isaac-projects` | NVIDIA Isaac Sim projects, simulation environments, and validation assets |
| `bmc-job-agent` | AI-assisted BMC engineering workflow and automation |
| `finance-agent` | Future AI-assisted financial and investment analysis |

## System Architecture

```text
                         Domain Applications
             +------------------+------------------+
             |                  |                  |
             v                  v                  v
      AI Robot System      BMC Job Agent     Future Applications
             |                  |                  |
             +------------------+------------------+
                                |
                   KServe V2 / Open Inference
                                |
                                v
                 +-----------------------------+
                 |      inference-server       |
                 |                             |
                 |      NVIDIA Triton          |
                 |            |                |
                 |   Inference Service/Core    |
                 |            |                |
                 |      Backend Factory        |
                 +------------+----------------+
                              |
                    Configuration-driven
                       Model Backends
                              |
                              v
                 +-----------------------------+
                 | Self-Hosted Compute         |
                 | Self-managed NVIDIA GPU     |
                 | Local / Private Model Store |
                 +-----------------------------+
```

For robotics, the inference infrastructure is one component of the larger end-to-end system:

```text
Natural Language / Sensors
          |
          v
    ai-robot-system
          |
          | standardized inference boundary
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

### Separate infrastructure from application semantics

Inference infrastructure owns:

- Model execution
- GPU acceleration
- Model loading and lifecycle
- Backend selection
- Standardized inference interfaces

Applications own:

- Domain-specific workflows
- Robot or engineering semantics
- Reasoning and orchestration
- Tool definitions and validation
- Business logic

### Standardize the inference boundary

Applications communicate with the inference runtime through **KServe V2 / Open Inference Protocol**.

This keeps clients independent from the concrete model implementation and allows model families or inference backends to evolve without requiring application architecture changes.

### Keep model selection configuration-driven

Applications request logical capabilities. Concrete model families and runtime settings are selected by infrastructure configuration.

Current and planned model-family direction includes:

- Meta — Llama
- Google — Gemma
- NVIDIA — Cosmos

### Self-host model execution

The current inference architecture runs on self-managed compute with locally managed model weights.

**NVIDIA Triton is the self-hosted inference serving runtime; it is not being used here as a cloud/CSP inference API.**

KServe V2 / Open Inference Protocol defines the standardized data-plane interface between applications and the inference server.

## Repository Boundaries

During development of the complete AI Robot architecture, the repositories are validated together through system milestones.

Long term, they are intended to evolve as independently maintained platform components:

```text
inference-server
    reusable AI inference infrastructure

ai-robot-system
    ROS 2 robot application/runtime stack

isaac-projects
    simulation assets and Isaac integration
```

Stable interfaces between these repositories allow each component to be versioned and maintained independently, similar to SDK/runtime/platform dependencies.

## Technology Direction

`Docker` · `NVIDIA Triton` · `KServe V2` · `Open Inference Protocol` · `Self-Hosted GPU Inference` · `ROS 2` · `NVIDIA Isaac Sim` · `Meta Llama` · `Computer Vision`
