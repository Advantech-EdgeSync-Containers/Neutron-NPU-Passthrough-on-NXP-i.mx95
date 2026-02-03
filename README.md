# Neutron-NPU-Passthrough-on-NXP-i.mx95

**Version:** 1.0  
**Release Date:** Feb 2026 
**Copyright:** © 2026 Advantech Corporation. All rights reserved.

---

## Overview

The `Neutron-NPU-Passthrough-on-NXP-i.mx95` provides a ready-to-use inference environment for validating ONNX models on the **NXP i.MX95 platform** with full support for **Neutron NPU hardware acceleration**.

This container is intended for developers, system integrators, and solution architects who need a fast and reproducible way to evaluate **CPU vs NPU inference performance** on i.MX95 without dealing with complex dependency resolution, driver alignment, or runtime configuration.

All required runtimes, libraries, and demo assets are pre-configured, enabling immediate execution of AI inference workloads.

---

## Key Features

- **ONNX Runtime on i.MX95**  
  Pre-integrated ONNX Runtime optimized for NXP i.MX95 architecture.

- **Neutron NPU Acceleration**  
  Native support for i.MX95 Neutron NPU via ONNX Runtime Execution Provider.

- **Out-of-Box Inference Demo**  
  Includes a complete image classification example using MobileNetV2.

- **CPU / NPU Comparison**  
  Simple runtime switch for validating functional correctness and performance.

- **Lightweight & Reproducible**  
  Focused container design for benchmarking, validation, and PoC workflows.

---

## Hardware Specifications

| Component       | Specification      |
|-----------------|--------------------|
| Target Hardware | [Advantech AOM-5521](https://www.advantech.com/en/https://www.advantech.com/zh-tw/products/77b59009-31a9-4751-bee1-45827a844421/aom-5521/mod_75b36e99-ac3f-4801-8b2b-1706ade1025d) |
| SoC             | [NXP i.MX95](https://www.nxp.com/products/I.MX95?tab=Documentation_Tab)   |
| GPU             | Arm® Mali™ G310         |
| NPU             | eIQ neutron N3-1034S    |
| Memory          | 8 GB LPDDR5             |

---

## Operating System

| Environment        | Operating System                                    |
|--------------------|-----------------------------------------------------|
| **Device Host**    | Yocto 5.2 Walnascar                                 |
| **Container**      | Ubuntu:24.04                                        |

---

## Software Components

| Component       | Version | Description |
|-----------------|---------|-------------|
| ONNX Runtime    | 1.22.0 from NXP BSP v6.12.20  | Inference runtime |
| Python          | 3.13     | Demo application runtime |
| NumPy / Pillow  | Latest  | Image preprocessing |

---

## Included Models & Assets

### Image Classification Model

| Model | Format | Note |
|------|--------|------|
| MobileNetV2 | ONNX | Lightweight CNN for inference validation |

### Demo Assets

| File | Description |
|-----|-------------|
| `grace_hopper.bmp` | Sample RGB image |
| `labels.txt` | ImageNet classification labels |

---

## Hardware Acceleration Support

| Accelerator | Support Level | Notes |
|-------------|---------------|-------|
| NPU (Neutron) | INT8 (primary) | Best performance-per-watt on i.MX95 |
| CPU           | FP32          | Reference & fallback execution |

### Precision Support

| Precision | Supported On | Notes |
|----------|--------------|-------|
| INT8     | NPU / CPU    | Recommended for deployment |
| FP32     | CPU          | Debug & reference |

---

## Repository Structure

```
advantech-onnxruntime-imx95-container/
├── README.md
├── run.sh
└── docker-compose.yml
```

## Quick Start Guide

### Prerequisites
* Please ensure docker & docker compose are available and accessible on device host OS
* Since default eMMC boot provides only 16 GB storage which is in-sufficient to run/build the container image, it is required to boot the Host OS using a 32 GB (minimum) SD card.

### Clone the Repository (on your development machine)

> **Note for Windows Users:**  
> If you are using **Linux**, no changes are needed — LF line endings are used by default.  
> If you are on **Windows**, please follow the steps in [Windows Git Line Ending Setup](./windows-git-setup.md) before cloning to ensure scripts and configuration files work correctly on Device.

```bash
git clone https://github.com/Advantech-EdgeSync-Containers/Neutron-NPU-Passthrough-on-NXP-i.mx95.git
cd Neutron-NPU-Passthrough-on-NXP-i.mx95
```

### Transfer the `Neutron-NPU-Passthrough-on-NXP-i.mx95` folder to AOM-5521 device

If you cloned the repo on a **separate development machine**, use `scp` to transfer only the relevant folder:

```bash
# From your development machine (Ubuntu or Windows PowerShell if SCP is installed)
scp -r ./Neutron-NPU-Passthrough-on-NXP-i.mx95\ <username>@<aom5521-ip>:/home/<username>/
```

Replace:

* `<username>` – Login username on the AOM-5521 board (e.g., `root`)
* `<aom5521-ip>` – IP address of the AOM-5521 board (e.g., `192.168.1.42`)

This will copy the folder to `/home/<username>/Neutron-NPU-Passthrough-on-NXP-i.mx95`.

Then SSH into the NXP device:

```bash
ssh <username>@<aom5521-ip>  (add '-X' tag for X11 display)
cd ~/Neutron-NPU-Passthrough-on-NXP-i.mx95
```

### Installation

```bash
# Make the build script executable
chmod +x run.sh

# Launch the container
./run.sh
```

### Run Demo
Run the demo in ONNX Runtime.
```
python3 advantech-onnxruntime.py
```
To enable NPU acceleration, pass the `-n` flag 
```
python3 advantech-onnxruntime.py -n
```

## Best Practices & Known Limitations

- **Minimum storage** Storage required for running Docker containers is 32 GB
- **NPU passthrough** Neutron EP error sometimes, reboot the development board may solve this problem.
```
Neutron: NEUTRON_IOCTL_BUFFER_CREATE failed!: Cannot allocate memory
```


Copyright © 2026 Advantech Corporation. All rights reserved.