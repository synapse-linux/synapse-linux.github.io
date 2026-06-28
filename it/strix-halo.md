---
layout: default
title: Architettura AMD Strix Halo
permalink: /it/strix-halo/
lang: it
---

# AMD Strix Halo — Architettura Tecnica

> ⚠️ **Sperimentale**: Synapse Linux è una derivata sperimentale di CachyOS. Il supporto Strix Halo è in fase di sviluppo attivo.

## Panoramica APU

**AMD Strix Halo** (Ryzen AI Max 300 series) è un APU monolitico che combina core CPU Zen 5, unità GPU RDNA 3.5 e un NPU XDNA 2 dedicato per inferenza AI — tutto connesso tramite una memoria unificata.

| Componente | Specifica |
|-------------|-----------|
| **CPU** | Zen 5 (fino a 16 core / 32 thread) |
| **GPU** | RDNA 3.5 (fino a 40 compute unit) |
| **NPU** | XDNA 2 (fino a 50+ TOPS) |
| **Memoria** | UMA unificata — CPU, GPU e NPU condividono lo stesso pool |
| **Cache** | 32 MB MALL (Memory Area Local to Lanes) |

## Stack Software

Synapse Linux fornisce uno stack software completo per Strix Halo:

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
│  ec_su_axb35 (controllo EC ventole) │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│         Hardware                    │
│  AMD Strix Halo APU                 │
│  Sixunited AXB35-02 EC             │
└─────────────────────────────────────┘
```

## Parametri Modulo Kernel

Il pacchetto `synapse-strixhalo-config` (`/etc/modprobe.d/strixhalo.conf`) configura:

| Parametro | Valore | Scopo |
|-----------|--------|-------|
| `amdgpu gttsize` | 131072 | Aumenta la tabella di traduzione GPU per compatibilità ROCm |
| `ttm pages_limit` | 31457280 | Aumenta il limite pagine TTM per allocazioni GPU grandi |

## Stack Compute ROCm

Il package chooser AI installa 20+ pacchetti ROCm:

| Pacchetto | Scopo |
|-----------|-------|
| `rocm-hip-sdk` | HIP SDK per programmazione GPU |
| `rocm-opencl-sdk` | Framework compute OpenCL |
| `rocm-ml-sdk` | Librerie machine learning |
| `miopen-hip` | Implementazione AMD di imec (primitivi ML) |
| `rocblas` | BLAS per ROCm |
| `rocsparse` | Libreria matrici sparse |
| `rocwmma` | Wave Matrix Multiply-Accumulate |
| `rocprofiler` / `roctracer` | Profilazione e tracing |
| `comgr` | Code object management |
| `rocminfo` | Informazioni sistema ROCm |
| `rocal` / `rocalution` | Elaborazione immagini / solutori |
| `xrt-plugin-amdxdna` | Driver kernel XDNA AI |

## NPU XDNA AI

Il **NPU XDNA 2** è un'unità neurale dedicata integrata nell'APU Strix Halo. Gestisce l'inferenza AI on-device con:

- Fino a **50+ TOPS** di performance INT8
- Supporto per framework ML comuni tramite ROCm
- Accesso tramite il driver kernel `xrt-plugin-amdxdna`

## Controllo Ventole e Potenza (EC)

La scheda **Sixunited AXB35-02** usa un controller embedded per la gestione termica. Il modulo DKMS `synapse-ec-su-axb35-linux` fornisce un'interfaccia sysfs:

### Interfaccia Sysfs

```
/sys/class/ec_su_axb35/
├── fan1/       — CPU fan 1
│   ├── rpm           (RO) velocità corrente
│   ├── mode          (RW) [auto, fixed, curve]
│   ├── level         (RW) [0-5] (0=0%, 1=20%, ..., 5=100%)
│   ├── rampup_curve  (RW) 5 soglie °C per aumento livello
│   └── rampdown_curve(RW) 5 soglie °C per diminuzione livello
├── fan2/       — CPU fan 2 (stessa struttura)
├── fan3/       — System fan (stessa struttura)
├── temp1/      — Temperatura CPU (°C)
└── apu/
    └── power_mode   (RW) [quiet, balanced, performance]
```

### Modalità Potenza

| Modalità | TDP | Utilizzo |
|----------|-----|----------|
| **quiet** | ~65W | Operazione silenziosa, performance ridotte |
| **balanced** | ~85W | Default — buon bilanciamento termico/performance |
| **performance** | ~120W | Massimo throughput, ventole più veloci |

### Curve Ventole

Lo script `axb35-apply-settings` configura curve con isteresi:

```
Ramp up:   60°C → 70°C → 83°C → 95°C → 97°C  (livello 1→5)
Ramp down: 50°C → 60°C → 80°C → 94°C → 96°C  (livello 5→1)
```

Questo previene cicli rapidi mantenendo un gap di temperatura tra soglie di salita e discesa.

## Compatibilità Hardware

| Dispositivo | Scheda | Stato |
|-------------|--------|-------|
| **Bosgame M5** | Sixunited AXB35-02 | ✅ Supportato (ventole + potenza) |
| **GMKtec EVO-X2** | Sixunited AXB35-02 | ✅ Supportato |
| **FEVM FA-EX9** | Sixunited AXB35-02 | ✅ Supportato |
| **Peladn YO1** | Sixunited AXB35-02 | ✅ Supportato |
| **NIMO AI MiniPC** | Sixunited AXB35-02 | ✅ Supportato |
| **ASUS ROG Flow Z13-KJP** | ASUS motherboard | ✅ Supportato (asusctl + rog-control-center) |

## Configurazione IOMMU

Per compatibilità ROCm, l'IOMMU può essere disabilitato per migliorare le performance GPU. Il pacchetto `synapse-strixhalo-config` fornisce il tuning:

```bash
# Abilita la disabilitazione IOMMU (migliora throughput GPU):
# Decommenta in /etc/modprobe.d/strixhalo.conf:
# options amd_iommu=off
```

## Monitoraggio

Usa lo strumento `su_axb35_monitor` per telemetria in tempo reale:

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
