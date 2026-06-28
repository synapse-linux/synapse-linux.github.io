---
layout: default
title: Informazioni
permalink: /it/about/
lang: it
---

# Informazioni su Synapse Linux

**Synapse Linux** è una distribuzione basata su Arch Linux mantenuta da **Mario Giustiniani**, focalizzata sul supporto **AMD Strix Halo AI** — accelerazione GPU ROCm, driver XDNA AI e controllo ventole/potenza custom per MiniPC e laptop Strix Halo.

## Cosa rende Synapse diverso?

| Funzione | Descrizione |
|----------|-------------|
| **Base CachyOS** | Costruita sui kernel e toolchain ottimizzati di CachyOS con auto-tuning |
| **Integrazione AI** | Installer Calamares custom con package chooser AI (stack ROCm + XDNA AI) |
| **Stack ROCm** | Installa `rocm-hip-sdk`, `rocm-opencl-sdk`, `rocm-ml-sdk`, `miopen-hip`, `rocblas` e 20+ pacchetti ROCm |
| **Driver XDNA AI** | `xrt-plugin-amdxdna` — driver kernel AMD XDNA AI per NPU Strix Halo |
| **Controllo ventole** | `synapse-ec-su-axb35-linux` — modulo kernel DKMS per controllo ventole e potenza su MiniPC AXB35 (profilo Bosgame M5) |
| **Supporto ASUS** | `asusctl` + `rog-control-center` per il controllo hardware su laptop ROG Flow Z13-KJP |
| **Config Strix Halo** | `synapse-strixhalo-config` — tuning di IOMMU, GTT size e limiti TTM pages |
| **Modulo Server** | Configurazione SSH, VNC e LLM (DS4) durante l'installazione |
| **Repo custom** | Repository pacman `[synapse-linux]` ospitato su GitHub |

## AI Package Chooser

L'installer offre **quattro opzioni** durante la configurazione:

| Opzione | Descrizione | Pacchetti |
|---------|-------------|-----------|
| **No AI** | Nessuno stack AI/LLM configurato | nessuno |
| **AMD Strix Halo AI** | Accelerazione GPU ROCm + driver XDNA AI per Strix Halo (Ryzen AI Max 395/385/370) | `synapse-strixhalo-config`, ROCm SDK, `xrt-plugin-amdxdna` |
| **Bosgame M5** | Profilo completo ventole + potenza per Bosgame M5 MiniPC | Stack Strix Halo core + `synapse-ec-su-axb35-linux` |
| **ROG Flow Z13-KJP** | Controllo ventole + potenza per ASUS ROG Flow Z13 laptop | Stack Strix Halo core + `asusctl` + `rog-control-center` |

## Bootloader Chooser

GRUB, rEFInd, systemd-boot o Limine — seleziona durante l'installazione.

## Desktop Chooser

Scegli tra Plasma (default), GNOME, Cosmic, Niri, Cinnamon, Budgie, MATE, Xfce, LXQt, LXDE, Hyprland, MangoWM, Sway, Wayfire, i3, Qtile, bspwm, Openbox o nessun desktop.

## Struttura dei repository

```
CachyOS-PKGBUILDS  ───────► cachyos-calamares (PKGBUILD → installer custom)
       ▲                         │
       │                         ▼
       │              CachyOS-Live-ISO (builder ISO + calamares-online.sh)
       │                         │
       │                         ▼
       │              synapse-pacman-repository (pacchetti precompilati)
       └─────────────────────────┘
          build-synapse-pkgs.sh compila i pacchetti da PKGBUILDS
```

## Sequenza di installazione Calamares

```
welcome → locale → keyboard → packagechooser@ai → packagechooser@bootloader
→ partition → packagechooser@desktop → netinstall → server → users → summary
```
Seguita da exec: partition → mount → shellprocess hooks → pacstrap → packages → bootloader → server services → cleanup.

## Team

Mantenuto da **Mario Giustiniani** ([mgiustiniani](https://github.com/mgiustiniani)) con contributi dalla comunità CachyOS.

## Link

- [GitHub Organization](https://github.com/synapse-linux)
- [CachyOS](https://cachyos.org)
- [PKGBUILDS](https://github.com/mgiustiniani/CachyOS-PKGBUILDS/tree/cachyos-ai-integration)
- [Live ISO](https://github.com/mgiustiniani/CachyOS-Live-ISO/tree/cachyos-ai-integration)
- [Calamares](https://github.com/mgiustiniani/cachyos-calamares/tree/cachyos-ai-integration)
