# ADMI — Hardware Documentation

## 1. Hardware Overview

ADMI (Automated Decision-Making Interface) is designed as an edge AI computing device built around a Raspberry Pi single-board computer.

The hardware platform provides the computational resources required to execute quantized Large Language Models (LLMs), process user interactions, maintain locally stored information, and perform decision-support operations without depending on continuous cloud connectivity.

The initial prototype uses a Raspberry Pi 4 Model B with 4 GB RAM as its primary computing unit.

## 2. Core Hardware Components

### 2.1 Raspberry Pi 4 Model B

**Role:** Primary processing and control unit.

| Specification | Details |
|---|---|
| Board | Raspberry Pi 4 Model B |
| Processor | Broadcom BCM2711 |
| CPU Architecture | ARM Cortex-A72 |
| CPU Cores | Quad-core |
| CPU Frequency | Up to 1.5 GHz |
| RAM | 4 GB LPDDR4 |
| Connectivity | Gigabit Ethernet, Wi-Fi, Bluetooth |
| USB | 2 × USB 3.0, 2 × USB 2.0 |
| Display Output | 2 × micro-HDMI |
| GPIO | 40-pin header |
| Storage Interface | microSD card |
| Power Input | USB-C, 5V |

**Purpose within ADMI:**

The Raspberry Pi acts as the central computing platform. It hosts the operating system, local AI inference runtime, application backend, data storage, and peripheral communication interfaces.

The ARM CPU performs model inference through the llama.cpp runtime, while the available RAM accommodates the operating system, model weights, inference context, and application processes.

### 2.2 Storage — microSD Card

**Role:** Operating system and local data storage.

| Specification | Details |
|---|---|
| Storage Medium | microSD |
| Capacity | 64 GB |
| Primary Usage | Raspberry Pi OS |
| Additional Usage | Application files, model weights, logs and data |

The microSD card provides persistent storage for the operating system, source code, GGUF model files, and application-level information.

Storage allocation is organized into separate project directories to maintain a structured development environment.

### 2.3 Display Interface

**Role:** Visual interaction with the device.

| Specification | Details |
|---|---|
| Current Display | MSI Portable Monitor |
| Interface | HDMI |
| Raspberry Pi Connector | Micro-HDMI |
| Dedicated Touchscreen | Planned |

The initial prototype uses an external portable monitor for system configuration, debugging, and application testing.

A dedicated touchscreen is planned for future integration to provide a direct user interaction interface.

### 2.4 Power Supply

**Role:** Electrical power delivery.

The Raspberry Pi 4 requires a stable 5V power supply capable of supporting the board and its connected peripherals.

The recommended official Raspberry Pi 4 power supply is rated at 5.1V and 3A through USB-C.

Power delivery must account for additional peripherals, particularly USB-connected displays and input devices.

## 3. Hardware Interfaces and Connectivity

The Raspberry Pi 4 provides several interfaces for future hardware integration.

| Interface | Purpose | Potential ADMI Application |
|---|---|---|
| USB 3.0 | High-speed peripheral connectivity | External storage |
| USB 2.0 | Standard peripheral connectivity | Keyboard, mouse and accessories |
| GPIO | General-purpose digital I/O | Hardware control and sensor integration |
| I2C | Two-wire serial communication | Sensor and peripheral communication |
| SPI | Synchronous serial communication | High-speed peripheral communication |
| UART | Serial communication | External microcontrollers |
| Wi-Fi | Wireless networking | Optional network-based retrieval |
| Bluetooth | Short-range wireless communication | Compatible peripherals |
| Ethernet | Wired networking | Reliable local network connectivity |
| HDMI | Digital display output | User interface |

These interfaces provide expansion capabilities for future ADMI hardware modules.

## 4. Hardware Architecture

The Raspberry Pi serves as the central processing node.

```text
              ADMI HARDWARE PLATFORM

          ┌──────────────────────────┐
          │    Raspberry Pi 4 B      │
          │       4 GB RAM           │
          │                          │
          │  ARM Cortex-A72 CPU      │
          └────────────┬─────────────┘
                       │
          ┌────────────┼─────────────┐
          │            │             │
          ▼            ▼             ▼
      microSD       micro-HDMI     USB Ports
          │            │             │
          ▼            ▼             ▼
       Storage      Portable      Keyboard /
                    Monitor        Peripherals

                       │
                  Wi-Fi / Ethernet
                       │
                       ▼
                Optional Network
```

## 5. Hardware Expansion Roadmap

The following components are considered for subsequent development stages.

| Component | Intended Function | Status |
|---|---|---|
| Raspberry Pi 4 Model B (4 GB) | Main computing platform | Acquired |
| 64 GB microSD | OS and persistent storage | Acquired |
| MSI portable monitor | Development display | Available |
| Compatible power supply | Stable power delivery | Verify specification |
| Raspberry Pi case | Physical protection | Planned |
| Dedicated touchscreen | Direct user interaction | Planned |
| Microphone | Voice input | Planned |
| Speaker | Audio output | Planned |
| External storage | Additional model/data capacity | Optional |

## 6. Hardware Constraints and Design Considerations

### 6.1 Computational Limitations

The Raspberry Pi 4 is an ARM-based, CPU-oriented edge computing platform without a dedicated CUDA-capable NVIDIA GPU.

Consequently, local language model inference depends primarily on CPU computation and available system memory.

Quantized models are used to reduce memory requirements and improve inference feasibility.

### 6.2 Memory Constraints

The 4 GB RAM configuration must accommodate:

- Operating system processes.
- Model weights.
- Inference context and runtime buffers.
- Backend application.
- Data storage and supporting services.

Model selection and context-size configuration must therefore consider total memory consumption rather than model file size alone.

### 6.3 Thermal Management

Continuous CPU-based inference can produce sustained processor loads.

Adequate ventilation and thermal management are important to prevent excessive temperatures and CPU frequency throttling.

### 6.4 Storage Considerations

The microSD card is used for the operating system and application storage.

Frequent writes from logging and persistent application data should be managed carefully to reduce unnecessary storage wear.

## 7. Future Hardware Development

Future revisions may extend ADMI into a more capable edge computing platform through:

- Dedicated touchscreen integration.
- Voice-based interaction through microphone and speaker.
- External high-speed storage.
- Sensor and microcontroller integration.
- Optional network connectivity for external knowledge retrieval.
- Improved thermal management and enclosure design.

These additions will be evaluated according to their computational requirements, power consumption, physical compatibility, and contribution to the overall system.

## 8. Current Hardware Configuration Summary

| Category | Current Configuration |
|---|---|
| Computing Platform | Raspberry Pi 4 Model B |
| Processor | Broadcom BCM2711, Quad-core Cortex-A72 |
| Memory | 4 GB LPDDR4 |
| Storage | 64 GB microSD |
| Operating System | Raspberry Pi OS |
| Display | MSI Portable Monitor |
| Remote Access | SSH |
| Inference Platform | CPU-based llama.cpp |
| Model Format | GGUF |
| Initial Model | Qwen2.5-0.5B-Instruct, Q4_K_M |

---

**Project:** ADMI — Automated Decision-Making Interface  
**Platform:** Raspberry Pi 4 Model B  
**Architecture:** Edge AI / Local Inference  
**Documentation:** Hardware Specification and Integration
