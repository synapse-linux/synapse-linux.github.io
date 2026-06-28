---
layout: default
title: Documentazione
permalink: /it/docs/
lang: it
---

# Documentazione

## Build dell'ISO

```bash
# Clona il repository live ISO
git clone https://github.com/mgiustiniani/CachyOS-Live-ISO.git
cd CachyOS-Live-ISO
git checkout cachyos-ai-integration

# Installa le dipendenze
sudo pacman -S archiso mkinitcpio-archiso git squashfs-tools grub --needed

# Build dell'ISO desktop
sudo ./buildiso.sh -p desktop -v -w
```

## Compilazione pacchetti

```bash
# Clona PKGBUILDS
git clone https://github.com/mgiustiniani/CachyOS-PKGBUILDS.git
cd CachyOS-PKGBUILDS
git checkout cachyos-ai-integration

# Compila un pacchetto specifico
cd <nome-pacchetto>
makepkg -si
```

## Aggiungere il repository custom

Aggiungi questo al tuo `/etc/pacman.conf`:

```ini
[synapse-linux]
SigLevel = Optional TrustAll
Server = https://raw.githubusercontent.com/mgiustiniani/synapse-pacman-repository/synapse-linux/repo/$arch
```

## Pacchetti Synapse

| Pacchetto | Descrizione |
|-----------|-------------|
| `synapse-strixhalo-config` | Parametri modulo kernel per AMD Strix Halo (IOMMU, GTT size, TTM pages) |
| `synapse-ec-su-axb35-linux` | Modulo kernel DKMS per controllo ventole e potenza su MiniPC AXB35 |
| `synapse-calamares` | Installer Calamares custom con package chooser AI e modulo server |

## Moduli Calamares

| Modulo | Config | Scopo |
|--------|--------|-------|
| `packagechooser@ai` | `packagechooser_ai.conf` | Scelta ottimizzazione AI/LLM (No AI / Strix Halo / Bosgame M5 / ROG Z13) |
| `netinstall` | `netinstall.yaml` | Gruppi di pacchetti (CachyOS required, base-devel, desktop-specific, gaming, ecc.) |
| `server` | `server.conf` + `server.qml` | Configurazione SSH, VNC e LLM (DS4) |
| `shellprocess` | vari `.conf` | Hook pre/post-install (pacman init, snapshot Btrfs, UFW, cleanup) |

## Sequenza di installazione

```
show:   welcome → locale → keyboard → packagechooser@ai → partition → netinstall → server → users → summary
exec:   partition → zfs → mount → shellprocess hooks → pacstrap → machineid → locale
        → keyboard → localecfg → chwd → packages@online → fstab → plymouthcfg
        → initcpiocfg → initcpio → users → networkcfg → displaymanager → hwclock
        → bootloader → server services → services-systemd → UFW → snapshot Btrfs → cleanup
show:   finished
```

## Dettagli AI Package Chooser

Il `packagechooser_ai.conf` offre quattro opzioni:

1. **No AI** — nessuno stack AI/LLM configurato
2. **AMD Strix Halo AI** — installa lo stack ROCm (20+ pacchetti) + `synapse-strixhalo-config` + `xrt-plugin-amdxdna`
3. **Bosgame M5** — profilo completo ventole/potenza: stack ROCm core + `synapse-ec-su-axb35-linux`
4. **ROG Flow Z13-KJP** — controllo hardware ASUS completo: stack ROCm core + `asusctl` + `rog-control-center`

## Modulo Server

Il modulo QML (`server.qml`) mostra dinamicamente le sezioni SSH e VNC in base ai pacchetti selezionati in netinstall (`openssh` → SSH, `wayvnc`/`x11vnc` → VNC). Una sezione LLM (DS4) appare quando sono selezionati pacchetti ROCm/AI.

## Controllo ventole (AXB35)

Dopo aver installato `synapse-ec-su-axb35-linux`:

```bash
# Abilita il servizio
sudo systemctl enable axb35-fan-control.service
sudo systemctl start axb35-fan-control.service

# Monitora lo stato delle ventole
sudo su_axb35_monitor

# Applica impostazioni custom
sudo axb35-apply-settings
```

## Stato build ISO

Ultima build: **cachyos-desktop-linux-260626.iso** (3.0 GB) — scritta su USB Ventoy.
