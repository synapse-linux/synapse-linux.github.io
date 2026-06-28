---
layout: default
title: Documentation
permalink: /en/docs/
lang: en
---

# Documentation

## Building the ISO

```bash
# Clone the live ISO repository
git clone https://github.com/mgiustiniani/CachyOS-Live-ISO.git
cd CachyOS-Live-ISO
git checkout cachyos-ai-integration

# Install dependencies
sudo pacman -S archiso mkinitcpio-archiso git squashfs-tools grub --needed

# Build the desktop ISO
sudo ./buildiso.sh -p desktop -v -w
```

## Building packages

```bash
# Clone PKGBUILDS
git clone https://github.com/mgiustiniani/CachyOS-PKGBUILDS.git
cd CachyOS-PKGBUILDS
git checkout cachyos-ai-integration

# Build a specific package
cd <package-name>
makepkg -si
```

## Adding the custom repository

Add this to your `/etc/pacman.conf`:

```ini
[synapse-linux]
SigLevel = Optional TrustAll
Server = https://raw.githubusercontent.com/mgiustiniani/synapse-pacman-repository/synapse-linux/repo/$arch
```

## Synapse packages

| Package | Description |
|---------|-------------|
| `synapse-strixhalo-config` | Kernel module parameters for AMD Strix Halo (IOMMU, GTT size, TTM pages) |
| `synapse-ec-su-axb35-linux` | DKMS kernel module for fan and power control on AXB35 MiniPCs |
| `synapse-calamares` | Custom Calamares installer with AI package chooser |

## Fan control (AXB35)

After installing `synapse-ec-su-axb35-linux`:

```bash
# Enable the service
sudo systemctl enable axb35-fan-control.service
sudo systemctl start axb35-fan-control.service

# Monitor fan status
sudo su_axb35_monitor

# Apply custom settings
sudo axb35-apply-settings
```

## ISO build status

Latest build: **cachyos-desktop-linux-260626.iso** (3.0 GB) — written to Ventoy USB.
