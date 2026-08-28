# James AI Engineering Lab

**James AI Engineering Lab** is a personal AI engineering lab focused on building reusable AI infrastructure and applying it across robotics, embedded/BMC engineering, finance, and future AI applications.

## Architecture

```text
                         James AI Engineering Lab
                                  |
                 +----------------+----------------+
                 |                                 |
         AI Infrastructure                    Applications
                 |                                 |
                 v                     +-----------+-----------+
        inference-server               v           v           v
                 |                 AI Robot     BMC Agent   Finance Agent
                 |
        KServe V2 / Open
        Inference Protocol
                 |
          NVIDIA Triton
                 |
        +--------+---------+
        v        v         v
      Llama    Vision    Embedding
             YOLO/DETR
```

## Core Infrastructure

### `inference-server`

General-purpose containerized AI model-serving infrastructure providing standardized inference APIs for text, vision, embedding, audio, and future multimodal workloads.

Key principles:

- Containerized and application-independent
- KServe V2 / Open Inference Protocol compatible
- NVIDIA Triton-based model serving
- Model and backend selection through configuration
- Shared by robotics, BMC, finance, and other AI applications
- Applications consume AI capabilities without depending on specific model implementations

## Projects

| Repository | Purpose |
|---|---|
| `inference-server` | General-purpose AI model serving and inference infrastructure |
| `ai-robot-system` | ROS 2 based robot intelligence and runtime architecture |
| `isaac-projects` | NVIDIA Isaac Sim projects, environments, and validation assets |
| `bmc-job-agent` | AI-assisted BMC engineering workflow and automation |
| `finance-agent` | Future AI-assisted investment and financial analysis |

## Design Principle

> **Build AI infrastructure once, then reuse it across domain applications.**

Applications own domain-specific logic and semantics. The inference server owns model execution, serving, acceleration, and standardized inference interfaces.

```text
AI Robot --------+
BMC Agent -------+----> inference-server ----> AI Models / GPU
Finance Agent ---+
```
