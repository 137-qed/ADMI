# ADMI — Model Preparation and Deployment

## 1. Introduction

This document describes the model preparation workflow for ADMI (Automated Decision-Making Interface), covering model selection, acquisition, format compatibility, quantization, memory considerations, and local inference preparation.

ADMI is designed to operate as an offline-first edge AI system on a Raspberry Pi 4 Model B with 4 GB RAM.

Due to the computational and memory limitations of the target hardware, model selection and optimization are essential to achieving practical local inference performance.

The initial implementation uses a lightweight instruction-tuned language model in GGUF format, executed through the llama.cpp inference runtime.

## 2. Model Selection Criteria

The language model is selected according to the following technical considerations:

### 2.1 Hardware Compatibility

The selected model must be compatible with the Raspberry Pi's ARM-based CPU architecture and available system memory.

### 2.2 Model Size

The parameter count and resulting model file size directly influence storage requirements and inference memory consumption.

Smaller models are preferred during initial development to establish reliable local inference.

### 2.3 Quantization Support

The model should support suitable quantized formats to reduce memory requirements and improve deployment feasibility.

### 2.4 Instruction Following

Since ADMI is intended to perform student counseling, instruction-tuned models are preferred over base pretrained language models.

### 2.5 Licensing

Model licenses and usage restrictions must be evaluated before selecting a model for distribution or deployment.

## 3. Initial Model Selection

The initial experimental model selected for ADMI is:

**Qwen2.5-0.5B-Instruct**

| Parameter | Specification |
|---|---|
| Model Family | Qwen2.5 |
| Parameters | Approximately 0.5 billion |
| Model Type | Instruction-tuned causal language model |
| Model Format | GGUF |
| Quantization | Q4_K_M |
| Inference Runtime | llama.cpp |
| Execution Hardware | Raspberry Pi 4 Model B |
| Execution Mode | CPU-based local inference |

The model provides a lightweight starting point for evaluating local language model execution on resource-constrained hardware.

Its selection is intended for initial functionality testing rather than establishing final counseling quality.

## 4. Model Acquisition

The model is obtained from Hugging Face, a platform for hosting and distributing machine learning models and datasets.

Repository:

https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF

The selected model file is:

`qwen2.5-0.5b-instruct-q4_k_m.gguf`

Download command:

```bash
cd ~/ADMI/models

wget -O qwen2.5-0.5b-instruct-q4_k_m.gguf \
"https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/resolve/main/qwen2.5-0.5B-Instruct-Q4_K_M.gguf"
```

Verify the downloaded file:

```bash
ls -lh ~/ADMI/models/
```

The downloaded file is stored locally to enable inference without repeatedly retrieving model weights from an external server.

## 5. Understanding the GGUF Format

GGUF (GPT-Generated Unified Format) is a binary file format designed for storing model weights and associated metadata for efficient model loading and inference.

It is commonly used with llama.cpp.

A GGUF model file can contain:

- Model architecture metadata.
- Model parameters and tensor information.
- Quantized model weights.
- Tokenizer information.
- Additional model configuration metadata.

GGUF simplifies model distribution and loading by packaging the required model information into a structured file.

## 6. Model Quantization

Quantization reduces the numerical precision used to represent model weights.

Conventional neural network weights may use FP32 or FP16 representations. Quantization reduces the number of bits used for weight storage, decreasing memory requirements.

### 6.1 Quantization Comparison

| Format | Approximate weight precision | General characteristic |
|---|---|---|
| FP32 | 32-bit | High memory requirement |
| FP16 | 16-bit | Reduced memory requirement |
| Q8_0 | 8-bit-class | Higher precision among common GGUF quantizations |
| Q4_K_M | 4-bit-class mixed quantization | Memory-efficient compromise |
| Q2_K | 2-bit-class | More aggressive compression |

Actual file sizes and memory consumption depend on the model architecture, tensor metadata, quantization scheme, and runtime overhead.

### 6.2 Q4_K_M Quantization

The initial ADMI model uses Q4_K_M quantization.

The K-quantization family uses block-based quantization schemes. The M variant uses mixed quantization choices across tensors rather than representing every weight identically.

Its purpose is to provide a practical balance between:

- Model storage requirements.
- Runtime memory consumption.
- Inference performance.
- Model output quality.

## 7. Model Storage Organization

Model files are maintained separately from application source code.

```text
ADMI/
│
├── models/
│   ├── qwen2.5-0.5b-instruct-q4_k_m.gguf
│   └── README.md
│
├── src/
│   └── llama.cpp/
│
└── docs/
    └── model-preparation.md
```

This separation allows models to be replaced, compared, and evaluated without modifying the application source code.

## 8. Local Model Execution

The llama.cpp runtime is used to execute the downloaded GGUF model.

Example command:

```bash
cd ~/ADMI/src/llama.cpp

./build/bin/llama-cli \
    -m ~/ADMI/models/qwen2.5-0.5b-instruct-q4_k_m.gguf \
    -c 1024 \
    -t 4 \
    -n 150
```

### Command Parameters

| Parameter | Function |
|---|---|
| `-m` | Specifies the model file location |
| `-c` | Sets the context window size |
| `-t` | Configures CPU inference threads |
| `-n` | Limits the number of generated tokens |

Additional command-line options should be verified using:

```bash
./build/bin/llama-cli --help
```

## 9. Model Evaluation and Benchmarking

Model preparation does not end with downloading and successfully loading the weights.

The model must be evaluated against the computational limitations and functional requirements of ADMI.

The following measurements are proposed:

### 9.1 Memory Utilization

Measure total system memory consumption during model loading and inference.

```bash
free -h
```

### 9.2 CPU Utilization

Monitor CPU usage during inference.

```bash
htop
```

### 9.3 Temperature Monitoring

Monitor processor temperature during sustained inference.

```bash
vcgencmd measure_temp
```

### 9.4 Inference Throughput

Measure generated tokens per second.

This provides an indication of how quickly the model generates responses.

### 9.5 Response Quality

Evaluate the model using representative student counseling prompts.

Assessment criteria include:

- Instruction adherence.
- Context retention.
- Response relevance.
- Consistency.
- Hallucination frequency.
- Quality of career-related guidance.

## 10. Model Preparation for Student Counseling

The selected model must eventually be evaluated against ADMI's intended application.

Representative evaluation scenarios include:

1. Identifying student interests and technical strengths.
2. Understanding educational background.
3. Exploring suitable career pathways.
4. Comparing possible academic and professional directions.
5. Generating structured follow-up questions.
6. Providing contextual explanations for recommendations.

The model's generated responses should be validated through application-level logic.

Career recommendations must not be treated as objectively correct simply because they are generated by an LLM.

## 11. Future Model Optimization

Further model optimization and preparation may include:

- Evaluating smaller and larger parameter-count models.
- Comparing different GGUF quantization levels.
- Optimizing context-window allocation.
- Evaluating inference thread configurations.
- Improving prompt templates.
- Exploring retrieval-augmented generation (RAG).
- Investigating domain-specific fine-tuning when justified by evaluation results.

Model changes should be benchmarked on the actual target hardware before deployment.

## 12. Model Preparation Workflow

The intended workflow is:

```text
MODEL IDENTIFICATION
        |
        ▼
MODEL SELECTION
        |
        ▼
LICENSE AND COMPATIBILITY CHECK
        |
        ▼
MODEL ACQUISITION
        |
        ▼
FORMAT VERIFICATION
        |
        ▼
QUANTIZATION SELECTION
        |
        ▼
LOCAL MODEL STORAGE
        |
        ▼
LLAMA.CPP MODEL LOADING
        |
        ▼
INFERENCE TESTING
        |
        ▼
PERFORMANCE BENCHMARKING
        |
        ▼
COUNSELING QUALITY EVALUATION
        |
        ▼
DEPLOYMENT READINESS
```

## 13. Conclusion

The model preparation process establishes the foundation for local language model inference within ADMI.

The initial implementation uses Qwen2.5-0.5B-Instruct in GGUF Q4_K_M format, executed through llama.cpp on the Raspberry Pi 4 Model B.

The preparation workflow emphasizes hardware compatibility, memory efficiency, quantization, inference validation, and application-specific evaluation.

Future model selection and optimization will be guided by measured performance and the quality of student counseling responses.

---

**Project:** ADMI — Automated Decision-Making Interface  
**Target Hardware:** Raspberry Pi 4 Model B (4 GB RAM)  
**Inference Runtime:** llama.cpp  
**Initial Model:** Qwen2.5-0.5B-Instruct  
**Model Format:** GGUF  
**Quantization:** Q4_K_M
