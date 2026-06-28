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
| `synapse-calamares` | Installer Calamares custom con package chooser AI |

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
