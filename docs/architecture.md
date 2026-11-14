# ADMI — System Architecture

## 1. Introduction

ADMI (Automated Decision-Making Interface) is an edge AI-based computing system designed to provide locally accessible, intelligent student counseling and decision-support capabilities.

The system is built around a Raspberry Pi 4 Model B, which acts as the central processing unit for local language model inference, application execution, data management, and user interaction.

The architecture follows a modular design in which individual components perform specialized functions while communicating through well-defined interfaces.

The primary design objective is to establish an offline-first AI system that can process user interactions and generate contextually relevant responses without requiring continuous cloud-based inference.

## 2. Architectural Design Principles

The system follows the following principles:

- **Edge Computing:** AI inference and application processing are performed locally on the Raspberry Pi.
- **Modularity:** The inference runtime, application backend, decision logic, and interface are maintained as separate components.
- **Offline-first Operation:** Core counseling functionality should remain available without internet connectivity.
- **Resource Efficiency:** Quantized language models and lightweight software components are used to accommodate hardware limitations.
- **Data Persistence:** Student profiles and conversation history are stored locally.
- **Extensibility:** The architecture allows future integration of sensors, audio peripherals, external storage, and network-based knowledge retrieval.

## 3. High-Level System Architecture

ADMI is organized into six logical layers.

1. Hardware Layer
2. Operating System Layer
3. AI Inference Layer
4. Application and Decision Layer
5. Data Management Layer
6. User Interface Layer

### 3.1 System Architecture Diagram

```text
                 ADMI SYSTEM ARCHITECTURE

┌───────────────────────────────────────────────┐
│               USER INTERFACE                  │
│                                               │
│       Text Input / Display / Future Audio     │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│          APPLICATION / DECISION LAYER         │
│                                               │
│       Student Profile Management              │
│       Input Processing                        │
│       Counseling Workflow                     │
│       Decision Logic                          │
│       Response Formatting                     │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│               AI INFERENCE LAYER              │
│                                               │
│       llama.cpp Runtime                       │
│       GGUF Model Loading                      │
│       Prompt Processing                       │
│       Token Generation                        │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│              LANGUAGE MODEL                   │
│                                               │
│       Qwen2.5-0.5B-Instruct                    │
│       Q4_K_M Quantization                      │
└───────────────────────────────────────────────┘

       ┌───────────────────────────────┐
       │       DATA MANAGEMENT         │
       │                               │
       │ Student Profiles              │
       │ Conversation History          │
       │ Local Knowledge Base          │
       └───────────────────────────────┘

       ┌───────────────────────────────┐
       │       OPERATING SYSTEM        │
       │                               │
       │ Raspberry Pi OS               │
       │ Linux Processes               │
       │ Memory and Storage Management │
       └───────────────────────────────┘

       ┌───────────────────────────────┐
       │         HARDWARE              │
       │                               │
       │ Raspberry Pi 4 Model B        │
       │ ARM Cortex-A72 CPU            │
       │ 4 GB RAM                      │
       │ 64 GB microSD                 │
       └───────────────────────────────┘
```

*Note: This represents the intended logical architecture. The application, database and interface layers are implementation targets, not all completed components.*

## 4. Hardware Layer

The hardware layer provides the computational and physical infrastructure required by ADMI.

The primary computing platform is the Raspberry Pi 4 Model B with 4 GB RAM.

Its responsibilities include:

- Executing the Linux operating system.
- Providing CPU resources for local model inference.
- Maintaining persistent storage.
- Managing communication with external peripherals.
- Hosting the application backend and user interface.

The hardware layer provides the foundation for edge-based computation without depending on a dedicated external GPU.

## 5. Operating System Layer

Raspberry Pi OS provides the software environment in which all ADMI components execute.

It manages:

- CPU scheduling and process execution.
- Memory allocation and virtual memory.
- Filesystem operations.
- Network communication.
- Peripheral and device interfaces.
- Application execution and system services.

Linux provides the underlying process and resource-management mechanisms required to run the inference runtime and application services concurrently.

## 6. AI Inference Layer

The inference layer is responsible for executing the pretrained language model locally.

### 6.1 Inference Runtime

ADMI uses llama.cpp as its initial inference runtime.

llama.cpp is responsible for:

- Loading GGUF model files.
- Initializing model parameters.
- Managing inference operations.
- Processing input tokens.
- Performing autoregressive token generation.
- Executing model computations through CPU threads.

### 6.2 Language Model

The initial experimental model is Qwen2.5-0.5B-Instruct.

Its weights are stored in GGUF format using Q4_K_M quantization.

Quantization reduces model memory requirements by lowering the numerical precision used to represent model weights.

This makes local inference more practical within the memory constraints of the Raspberry Pi 4.

### 6.3 Inference Workflow

The basic inference process follows these stages:

1
