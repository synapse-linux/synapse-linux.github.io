---
layout: default
title: AMD Strix Halo Architecture
permalink: /en/strix-halo/
lang: en
---

# AMD Strix Halo — Technical Architecture

> ⚠️ **Experimental**: Synapse Linux is an experimental derivative of CachyOS. Strix Halo support is under active development.

## APU Overview

**AMD Strix Halo** (Ryzen AI Max 300 series) is a monolithic APU combining Zen 5 CPU cores, RDNA 3.5 GPU compute units, and a dedicated XDNA 2 NPU for AI inference — all connected via a unified memory fabric.

| Component | Specification |
|-----------|---------------|
| **CPU** | Zen 5 (up to 16 cores / 32 threads) |
| **GPU** | RDNA 3.5 (up to 40 compute units) |
| **NPU** | XDNA 2 (up to 50+ TOPS) |
| **Memory** | Unified UMA — CPU, GPU, NPU share a single memory pool |
| **Cache** | 32 MB MALL (Memory Area Local to Lanes) |

## Software Stack

Synapse Linux provides a complete software stack for Strix Halo:

```
┌─────────────────────────────────────┐
│         Userspace (ROCm)            │
│  rocm-hip-sdk   rocm-opencl-sdk     │
│  rocm-ml-sdk    miopen-hip          │
│  rocblas        rocminfo            │
│  rocprofiler    roctracer           │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│         Kernel Drivers              │
│  amdgpu (gttsize=131072)            │
│  ttm (pages_limit=31457280)         │
│  xrt-plugin-amdxdna (XDNA AI)       │
│  ec_su_axb35 (EC fan control)       │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│         Hardware                    │
│  AMD Strix Halo APU                 │
│  Sixunited AXB35-02 EC             │
└─────────────────────────────────────┘
```

## Kernel Module Parameters

The package `synopsis-strixhalo-config` (`/etc/modprobe.d/strixhalo.conf`) tunes:

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `amdgpu gttsize` | 131072 | Increases GPU translation table size for ROCm compatibility |
| `ttm pages_limit` | 31457280 | Raises TTM pages limit for large GPU memory allocations |

## ROCm Compute Stack

The AI package chooser installs 20+ ROCm packages for full GPU compute:

| Package | Purpose |
|---------|---------|
| `rocm-hip-sdk` | HIP SDK for GPU programming |
| `rocm-opencl-sdk` | OpenCL compute framework |
| `rocm-ml-sdk` | Machine learning libraries |
| `miopen-hip` | AMD's implementation of imec (ML primitives) |
| `rocblas` | BLAS for ROCm |
| `rocsparse` | Sparse matrix library |
| `rocwmma` | Wave Matrix Multiply-Accumulate |
| `rocprofiler` / `roctracer` | Profiling and tracing |
| `comgr` | Code object management |
| `rocminfo` | ROCm system information |
| `rocal` / `rocalution` | Image processing / solvers |
| `xrt-plugin-amdxdna` | XDNA AI kernel driver |

## XDNA AI NPU

The **XDNA 2 NPU** is a dedicated neural processing unit integrated into the Strix Halo APU. It handles on-device AI inference with:

- Up to **50+ TOPS** of INT8 performance
- Support for common ML frameworks via ROCm
- Accessed through the `xrt-plugin-amdxdna` kernel driver

## Embedded Controller (EC) — Fan & Power Control

The **Sixunited AXB35-02** board uses an embedded controller for thermal management. The `synapse-ec-su-axb35-linux` DKMS module provides a sysfs interface:

### Sysfs Interface

```
/sys/class/ec_su_axb35/
├── fan1/       — CPU fan 1
│   ├── rpm           (RO) current speed
│   ├── mode          (RW) [auto, fixed, curve]
│   ├── level         (RW) [0-5] (0=0%, 1=20%, ..., 5=100%)
│   ├── rampup_curve  (RW) 5 temp thresholds for level increase
│   └── rampdown_curve(RW) 5 temp thresholds for level decrease
├── fan2/       — CPU fan 2 (same structure)
├── fan3/       — System fan (same structure)
├── temp1/      — CPU temperature (°C)
└── apu/
    └── power_mode   (RW) [quiet, balanced, performance]
```

### Power Modes

| Mode | TDP | Use Case |
|------|-----|----------|
| **quiet** | ~65W | Silent operation, lower performance |
| **balanced** | ~85W | Default — good balance of thermals and performance |
| **performance** | ~120W | Maximum throughput, higher fan speeds |

### Fan Curves

The default `axb35-apply-settings` script configures hysteresis-based curves:

```
Ramp up:   60°C → 70°C → 83°C → 95°C → 97°C  (level 1→5)
Ramp down: 50°C → 60°C → 80°C → 94°C → 96°C  (level 5→1)
```

This prevents rapid cycling by keeping a temperature gap between ramp-up and ramp-down thresholds.

## Hardware Compatibility

| Device | Board | Status |
|--------|-------|--------|
| **Bosgame M5** | Sixunited AXB35-02 | ✅ Supported (fan + power) |
| **GMKtec EVO-X2** | Sixunited AXB35-02 | ✅ Supported |
| **FEVM FA-EX9** | Sixunited AXB35-02 | ✅ Supported |
| **Peladn YO1** | Sixunited AXB35-02 | ✅ Supported |
| **NIMO AI MiniPC** | Sixunited AXB35-02 | ✅ Supported |
| **ASUS ROG Flow Z13-KJP** | ASUS motherboard | ✅ Supported (asusctl + rog-control-center) |

## IOMMU Configuration

For ROCm compatibility, the IOMMU can be disabled to improve GPU performance. The `synapse-strixhalo-config` package provides the tuning, but the IOMMU disable is left as a configurable option:

```bash
# Enable IOMMU disable (improves GPU throughput):
# Uncomment in /etc/modprobe.d/strixhalo.conf:
# options amd_iommu=off
```

## Monitoring

Use the included `su_axb35_monitor` tool for real-time telemetry:

```bash
sudo su_axb35_monitor
```

Output:

```
+--------------------------------------------------------------+
| CPU-Temp: 45 C                 Power mode: balanced           |
+-----+-------+-------+------+----------------+----------------+
| FAN | MODE  | LEVEL | RPM  | RAMPUP         | RAMPDOWN       |
| 1   | curve | 0     |  950 | 60,70,83,95,97 | 50,60,80,94,96 |
| 2   | curve | 0     |  900 | 60,70,83,95,97 | 50,60,80,94,96 |
| 3   | curve | 0     |  800 | 60,70,83,95,97 | 50,60,80,94,96 |
+-----+-------+-------+------+----------------+----------------+
```
