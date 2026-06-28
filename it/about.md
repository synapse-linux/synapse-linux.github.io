---
layout: default
title: Informazioni
permalink: /it/about/
lang: it
---

# Informazioni su Synapse Linux

**Synapse Linux** è una distribuzione basata su Arch Linux mantenuta da **Mario Giustiniani**, focalizzata sull'ottimizzazione delle performance e sul supporto **AMD Strix Halo AI**.

## Cosa rende Synapse diverso?

| Funzione | Descrizione |
|----------|-------------|
| **Base CachyOS** | Costruita sui kernel e toolchain ottimizzati di CachyOS |
| **Integrazione AI** | Package chooser Calamares custom con opzione AMD Strix Halo AI |
| **Controllo ventole** | `synapse-ec-su-axb35-linux` — modulo kernel DKMS per la gestione di ventole e potenza su MiniPC AXB35 |
| **Config Strix Halo** | `synapse-strixhalo-config` — tuning di IOMMU, GTT size e limiti TTM pages |
| **Repo custom** | Repository pacman `[synapse-linux]` ospitato su GitHub |

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

## AI Package Chooser

L'installer offre due opzioni durante la configurazione:

- **No AI** — pacchetti standard CachyOS desktop
- **AMD Strix Halo AI** — installa `synapse-strixhalo-config` + `synapse-ec-su-axb35-linux` per MiniPC con APU AMD Strix Halo

## Team

Mantenuto da **Mario Giustiniani** ([mgiustiniani](https://github.com/mgiustiniani)) con contributi dalla comunità CachyOS.

## Link

- [GitHub Organization](https://github.com/synapse-linux)
- [CachyOS](https://cachyos.org)
- [PKGBUILDS](https://github.com/mgiustiniani/CachyOS-PKGBUILDS/tree/cachyos-ai-integration)
- [Live ISO](https://github.com/mgiustiniani/CachyOS-Live-ISO/tree/cachyos-ai-integration)
