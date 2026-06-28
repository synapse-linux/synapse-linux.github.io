---
layout: default
title: About
permalink: /en/about/
lang: en
---

# About Synapse Linux

**Synapse Linux** is an Arch Linux-based distribution maintained by **Mario Giustiniani**, focused on performance optimization and **AMD Strix Halo AI** support.

## What makes Synapse different?

| Feature | Description |
|---------|-------------|
| **CachyOS base** | Built on top of CachyOS's optimized kernel and toolchain |
| **AI integration** | Custom Calamares package chooser with AMD Strix Halo AI option |
| **Fan control** | `synapse-ec-su-axb35-linux` — DKMS kernel module for AXB35 MiniPC fan & power management |
| **Strix Halo config** | `synapse-strixhalo-config` — IOMMU, GTT size, and TTM pages limit tuning |
| **Custom repo** | `[synapse-linux]` pacman repository hosted on GitHub |

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

## AI Package Chooser

The installer offers two options during setup:

- **No AI** — standard CachyOS desktop packages
- **AMD Strix Halo AI** — installs `synapse-strixhalo-config` + `synapse-ec-su-axb35-linux` for MiniPCs with AMD Strix Halo APUs

## Team

Maintained by **Mario Giustiniani** ([mgiustiniani](https://github.com/mgiustiniani)) with contributions from the CachyOS community.

## Links

- [GitHub Organization](https://github.com/synapse-linux)
- [CachyOS](https://cachyos.org)
- [PKGBUILDS](https://github.com/mgiustiniani/CachyOS-PKGBUILDS/tree/cachyos-ai-integration)
- [Live ISO](https://github.com/mgiustiniani/CachyOS-Live-ISO/tree/cachyos-ai-integration)
