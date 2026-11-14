# ADMI
ADMI also "Automated Decision Making Interface" is an intelligent embedded device, which brings specialized LLM models for decision making scenarios.

Purpose of the project is to demonstrate the idea of how a person could stay on the path of clarity in the world of infinite deviations. This device helps an individual make a decision in any specific scenarios, like making a decision for a student career path.
Why not ChatGPT or Claude or DeepSeek, etc?

# ADMI — Automated Decision-Making Interface

**An Offline-First Edge AI System for Student Counselling**

## 1. Overview

ADMI (Automated Decision-Making Interface) is a hardware-based Edge AI system designed to perform intelligent, locally executed decision-support tasks. Its initial application is a **Student Counsellor** that can interact with students, understand their queries, and provide structured academic and career-related guidance.

The project explores deploying a Large Language Model (LLM) on a resource-constrained single-board computer, enabling local AI inference without depending on cloud-based AI services.

The long-term objective is to develop an expandable edge-computing device capable of interacting with users, processing information locally, and eventually integrating with external sensors and connected devices.

## 2. Project Objectives

- Develop a functional Edge AI system using a Raspberry Pi.
- Run an LLM locally using `llama.cpp`.
- Deploy a quantized GGUF model to reduce memory and computational requirements.
- Build an offline-first student counselling application.
- Separate language generation from application-level decision-making and control logic.
- Establish a modular architecture that supports future hardware and software expansion.

## 3. Hardware Configuration

| Component | Specification | Status |
|---|---|---|
| Single-board computer | Raspberry Pi 4 Model B | Acquired |
| RAM | 4 GB LPDDR4 | Available |
| Storage | 64 GB microSD card | Available |
| Operating system | Raspberry Pi OS | Installed |
| Development access | SSH over local network | Working |
| External display | MSI portable monitor | Available |
| Touchscreen | Dedicated touchscreen display | Planned |
| Audio input/output | Microphone and speaker | Planned |

## 4. Software Stack

| Layer | Technology | Purpose |
|---|---|---|
| Operating system | Raspberry Pi OS | Hardware and system management |
| Programming | C/C++, Python | Inference and application development |
| Model runtime | llama.cpp | Local LLM inference |
| Model format | GGUF | Model storage and execution |
| Model | Qwen2.5-0.5B-Instruct | Initial language model |
| Quantization | Q4_K_M | Reduced model memory requirements |
| Backend | Python | Application logic and orchestration |
| Database | SQLite (planned) | Local data persistence |
| Interface | To be determined | User interaction |

*The table describes the selected and planned technology stack; not every component has been implemented.*

## 5. System Architecture

ADMI follows a modular architecture in which hardware, operating system, inference engine, application logic, data management, and user interface operate as separate layers.

```text
              USER
                |
                v
       USER INTERFACE
                |
                v
       APPLICATION LAYER
       - Conversation handling
       - Decision logic
       - Prompt management
       - Response validation
                |
                v
        INFERENCE ENGINE
            llama.cpp
                |
                v
         LOCAL LLM MODEL
       Qwen2.5-0.5B-Instruct
             GGUF
                |
                v
        RASPBERRY PI 4
                |
                v
       LOCAL DATA STORAGE
          SQLite (planned)
```

The language model generates responses, while the application layer manages the interaction workflow, applies structured rules, and handles application-specific operations.

## 6. Development Roadmap

| Phase | Description | Status |
|---|---|---|
| 1 | Project research and architecture planning | Completed |
| 2 | Hardware acquisition and Raspberry Pi setup | Completed |
| 3 | Linux environment and dependency preparation | In progress |
| 4 | llama.cpp compilation and configuration | Pending verification |
| 5 | GGUF model acquisition and local inference | Pending verification |
| 6 | Model evaluation and performance benchmarking | Pending |
| 7 | Backend and application logic development | Pending |
| 8 | Local database integration | Pending |
| 9 | Student counselling interface | Pending |
| 10 | Hardware integration and system testing | Pending |

## 7. Repository Structure

```text
ADMI/
│
├── README.md
│
├── docs/
│   ├── installation.md
│   ├── hardware.md
│   ├── architecture.md
│   └── model-preparation.md
│
├── models/
│   └── .gitkeep
│
├── src/
│   └── .gitkeep
│
├── data/
│   └── .gitkeep
│
├── logs/
│   └── .gitkeep
│
└── tests/
    └── .gitkeep
```

The repository structure will evolve as individual software components are implemented.

## 8. Installation and Setup

### Prerequisites

- Raspberry Pi 4 Model B (4 GB RAM)
- Raspberry Pi OS
- Internet connection for initial setup and model acquisition
- SSH access or a directly connected display and keyboard

### Initial system setup

Update the operating system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install development dependencies:

```bash
sudo apt install -y \
git cmake build-essential pkg-config \
python3 python3-venv python3-pip \
curl wget htop
```

### Clone llama.cpp

```bash
cd ~/ADMI/src
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
```

### Compile llama.cpp

```bash
cmake -B build \
  -DCMAKE_BUILD_TYPE=Release \
  -DGGML_NATIVE=ON

cmake --build build --config Release -j2
```

Verify the executable:

```bash
./build/bin/llama-cli --version
```

For the complete installation procedure, refer to [`docs/installation.md`](docs/installation.md).

## 9. Model Configuration

The initial model selected for ADMI is:

| Parameter | Configuration |
|---|---|
| Model | Qwen2.5-0.5B-Instruct |
| Format | GGUF |
| Quantization | Q4_K_M |
| Runtime | llama.cpp |
| Execution | Local CPU inference |
| Target hardware | Raspberry Pi 4, 4 GB RAM |

The initial objective is to establish reliable local inference before exploring larger models, fine-tuning, or advanced optimization techniques.

Refer to [`docs/model-preparation.md`](docs/model-preparation.md) for model acquisition, quantization, evaluation, and optimization details.

## 10. Design Principles

**Offline-first:** Core inference and application functionality should operate locally without requiring continuous internet connectivity.

**Resource efficiency:** Model selection and optimization must account for the Raspberry Pi's limited CPU, RAM, and storage resources.

**Modularity:** Hardware, inference, application logic, and user-interface components should remain independently maintainable.

**Controlled decision-making:** The LLM should not independently control every application operation. Structured application logic should manage workflows and validate relevant outputs.

**Expandability:** The architecture should accommodate future peripherals, sensors, and edge-computing applications.

## 11. Future Scope

- Local conversational interface for student counselling.
- Structured student profiles and local database integration.
- Context-aware academic and career guidance.
- Retrieval-Augmented Generation (RAG) using locally stored resources.
- Model performance optimization for low-resource hardware.
- Touchscreen and audio interaction.
- Integration with IoT sensors and external devices.
- Expansion into a general-purpose intelligent edge-computing system.

## 12. Documentation

| Document | Description |
|---|---|
| [Installation](docs/installation.md) | Raspberry Pi environment and software installation |
| [Hardware](docs/hardware.md) | Hardware specifications and peripheral planning |
| [Architecture](docs/architecture.md) | System architecture and component responsibilities |
| [Model Preparation](docs/model-preparation.md) | Model selection, GGUF, quantization, and evaluation |

## 13. Project Status

ADMI is currently in its hardware setup and local inference development stage. The Raspberry Pi 4 environment has been installed and configured. The next major milestone is establishing and evaluating local LLM inference using `llama.cpp`.

Implementation claims and performance figures will be updated as individual components are developed and tested.

---

**Project:** ADMI — Automated Decision-Making Interface  
**Initial Application:** Edge AI Student Counsellor  
**Platform:** Raspberry Pi 4 Model B  
**Development Approach:** Offline-first, modular Edge AI
