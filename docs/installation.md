# ADMI — Installation Guide

## 1. Overview

This document includes the hardware preparation, operating system configuration, software dependencies, and local AI runtime installation required to deploy ADMI (Automated Decision-Making Interface) on a Raspberry Pi 4 Model B.

The installation process establishes a local computing environment capable of running quantized Large Language Models (LLMs) using CPU-based inference.

## 2. Hardware Requirements

| Component | Specification |
|---|---|
| SBC | Raspberry Pi 4 Model B |
| RAM | 4 GB |
| Storage | 64 GB microSD card |
| Operating System | Raspberry Pi OS |
| Remote Access | SSH |
| Display | External portable monitor (optional) |
| Power Supply | Compatible Raspberry Pi 4 power supply |

## 3. Software Requirements

The following software packages and tools are required:

- Raspberry Pi OS
- Git
- CMake
- GCC/G++
- Build Essential
- Python 3
- pkg-config
- llama.cpp inference runtime

## 4. Operating System Preparation

Update the package repositories and upgrade installed packages:

```bash
sudo apt update
sudo apt upgrade -y
```

Install the required development dependencies:

```bash
sudo apt install -y \
    git \
    cmake \
    build-essential \
    pkg-config \
    python3 \
    python3-venv \
    python3-pip \
    curl \
    wget \
    htop
```

## 5. Project Directory Configuration

Create the ADMI project directory:

```bash
mkdir -p ~/ADMI/{models,src,docs,logs,data}
cd ~/ADMI
```

Directory structure:

```text
ADMI/
├── models/     # Local LLM model files
├── src/        # Source code and runtime
├── docs/       # Technical documentation
├── logs/       # Runtime and diagnostic logs
└── data/       # Application data
```

## 6. Local LLM Runtime Installation

ADMI uses llama.cpp as its initial local inference runtime.

Clone the official repository:

```bash
cd ~/ADMI/src

git clone https://github.com/ggml-org/llama.cpp.git

cd llama.cpp
```

Configure the build:

```bash
cmake -B build \
    -DCMAKE_BUILD_TYPE=Release \
    -DGGML_NATIVE=ON
```

Compile the runtime:

```bash
cmake --build build --config Release -j2
```

Verify the installation:

```bash
./build/bin/llama-cli --version
```

## 7. Language Model Installation

The initial experimental model is:

**Qwen2.5-0.5B-Instruct — Q4_K_M (GGUF)**

Download the model:

```bash
cd ~/ADMI/models

wget -O qwen2.5-0.5b-instruct-q4_k_m.gguf \
"https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/resolve/main/qwen2.5-0.5B-Instruct-Q4_K_M.gguf"
```

Verify the downloaded file:

```bash
ls -lh ~/ADMI/models/
```

## 8. Model Execution

Execute the model through llama.cpp:

```bash
cd ~/ADMI/src/llama.cpp

./build/bin/llama-cli \
    -m ~/ADMI/models/qwen2.5-0.5b-instruct-q4_k_m.gguf \
    -c 1024 \
    -t 4 \
    -n 150
```

Command-line parameters:

| Parameter | Description |
|---|---|
| -m | Specifies the model file |
| -c | Configures the context window |
| -t | Specifies CPU inference threads |
| -n | Limits generated tokens |

Use `./build/bin/llama-cli --help` to inspect additional options supported by the installed version.

## 9. System Verification

Check the system configuration:

```bash
uname -m
free -h
df -h /
vcgencmd measure_temp
```

Verify that:

- The Raspberry Pi boots successfully.
- SSH connectivity is operational.
- Required development dependencies are installed.
- llama.cpp compiles successfully.
- The GGUF model downloads successfully.
- The model loads and generates a response locally.

## 10. Installation Status

The installation procedure establishes the initial ADMI local inference environment.

Further development stages include:

- Local inference API deployment.
- Python-based application integration.
- Student profile and conversation management.
- Offline-first operation.
- User interface development.
- Performance and resource optimization.

---

**Project:** ADMI — Automated Decision-Making Interface  
**Hardware Platform:** Raspberry Pi 4 Model B (4 GB RAM)  
**Initial Inference Runtime:** llama.cpp  
**Initial Model:** Qwen2.5-0.5B-Instruct (GGUF, Q4_K_M)
