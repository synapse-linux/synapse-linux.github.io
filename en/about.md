---
layout: default
title: About
permalink: /en/about/
lang: en
---

# About Synapse Linux

**Synapse Linux** is an Arch Linux-based distribution maintained by **Mario Giustiniani**, focused on **AMD Strix Halo AI** support — ROCm GPU acceleration, XDNA AI drivers, and custom fan/power control for Strix Halo MiniPCs and laptops.

## What makes Synapse different?

| Feature | Description |
|---------|-------------|
| **CachyOS base** | Built on top of CachyOS's optimized kernel and toolchain with auto-tuning |
| **AI integration** | Custom Calamares installer with AI package chooser (ROCm + XDNA AI stack) |
| **ROCm stack** | Installs `rocm-hip-sdk`, `rocm-opencl-sdk`, `rocm-ml-sdk`, `miopen-hip`, `rocblas`, and 20+ ROCm packages |
| **XDNA AI driver** | `xrt-plugin-amdxdna` — AMD XDNA AI kernel driver for Strix Halo NPU |
| **Fan control** | `synapse-ec-su-axb35-linux` — DKMS kernel module for AXB35 MiniPC fan & power management (Bosgame M5 profile) |
| **ASUS support** | `asusctl` + `rog-control-center` for ROG Flow Z13-KJP laptop hardware control |
| **Strix Halo config** | `synapse-strixhalo-config` — IOMMU, GTT size, and TTM pages limit tuning |
| **Server module** | SSH, VNC, and LLM (DS4) configuration during installation |
| **Custom repo** | `[synapse-linux]` pacman repository hosted on GitHub |

## AI Package Chooser

The installer offers **four options** during setup:

| Option | Description | Packages |
|--------|-------------|----------|
| **No AI** | No AI/LLM stack configured | none |
| **AMD Strix Halo AI** | ROCm GPU acceleration + XDNA AI driver for Strix Halo (Ryzen AI Max 395/385/370) | `synapse-strixhalo-config`, ROCm SDKs, `xrt-plugin-amdxdna` |
| **Bosgame M5** | Full fan + power control profile for Bosgame M5 MiniPC | Core Strix Halo stack + `synapse-ec-su-axb35-linux` |
| **ROG Flow Z13-KJP** | Full fan + power control for ASUS ROG Flow Z13 laptop | Core Strix Halo stack + `asusctl` + `rog-control-center` |

## Bootloader Chooser

GRUB, rEFInd, systemd-boot, or Limine — select during installation.

## Desktop Chooser

Choose from Plasma (default), GNOME, Cosmic, Niri, Cinnamon, Budgie, MATE, Xfce, LXQt, LXDE, Hyprland, MangoWM, Sway, Wayfire, i3, Qtile, bspwm, Openbox, or no desktop.

## Repository structure

```
CachyOS-PKGBUILDS  ───────► cachyos-calamares (PKGBUILD → custom installer)
       ▲                         │
       │                         ▼
       │              CachyOS-Live-ISO (ISO builder + calamares-online.sh)
       │                         │
       │                         ▼
       │              synapse-pacman-repository (precompiled packages)
       └─────────────────────────┘
          build-synapse-pkgs.sh compiles packages from PKGBUILDS
```

## Calamares installation sequence

```
welcome → locale → keyboard → packagechooser@ai → packagechooser@bootloader
→ partition → packagechooser@desktop → netinstall → server → users → summary
```
Followed by exec: partition → mount → shellprocess hooks → pacstrap → packages → bootloader → server services → cleanup.

## Team

Maintained by **Mario Giustiniani** ([mgiustiniani](https://github.com/mgiustiniani)) with contributions from the CachyOS community.

## Links

- [GitHub Organization](https://github.com/synapse-linux)
- [CachyOS](https://cachyos.org)
- [PKGBUILDS](https://github.com/mgiustiniani/CachyOS-PKGBUILDS/tree/cachyos-ai-integration)
- [Live ISO](https://github.com/mgiustiniani/CachyOS-Live-ISO/tree/cachyos-ai-integration)
- [Calamares](https://github.com/mgiustiniani/cachyos-calamares/tree/cachyos-ai-integration)
