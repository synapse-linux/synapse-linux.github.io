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
| `synapse-calamares` | Custom Calamares installer with AI package chooser, server module, bootloader/desktop chooser |

## Calamares installer modules

| Module | Config | Purpose |
|--------|--------|---------|
| `packagechooser@ai` | `packagechooser_ai.conf` | AI/LLM optimization chooser (No AI / Strix Halo / Bosgame M5 / ROG Z13) |
| `packagechooser@bootloader` | `packagechooser_bootloader.conf` | Bootloader selection (GRUB, rEFInd, systemd-boot, Limine) |
| `packagechooser@desktop` | `packagechooser_desktop.conf` | Desktop environment selection |
| `netinstall` | `netinstall.yaml` | Package groups (CachyOS required, base-devel, desktop-specific, gaming, etc.) |
| `server` | `server.conf` + `server.qml` | SSH, VNC, and LLM (DS4) configuration |
| `shellprocess` | various `.conf` | Pre/post-install hooks (pacman init, Btrfs snapshot, UFW, cleanup) |

## Installation sequence

```
show:   welcome → locale → keyboard → packagechooser@ai → packagechooser@bootloader
        → partition → packagechooser@desktop → netinstall → server → users → summary
exec:   partition → zfs → mount → shellprocess hooks → pacstrap → machineid → locale
        → keyboard → localecfg → chwd → packages@online → fstab → plymouthcfg
        → initcpiocfg → initcpio → users → networkcfg → displaymanager → hwclock
        → bootloader → server services → services-systemd → UFW → Btrfs snapshot → cleanup
show:   finished
```

## AI Package Chooser details

The `packagechooser_ai.conf` offers four options:

1. **No AI** — no AI/LLM stack configured
2. **AMD Strix Halo AI** — installs ROCm compute stack (20+ packages) + `synapse-strixhalo-config` + `xrt-plugin-amdxdna`
3. **Bosgame M5** — full fan/power control profile: core ROCm stack + `synapse-ec-su-axb35-linux`
4. **ROG Flow Z13-KJP** — full ASUS hardware control: core ROCm stack + `asusctl` + `rog-control-center`

## Server module

The QML-based server module (`server.qml`) dynamically shows SSH and VNC configuration sections based on which packages are selected in netinstall (`openssh` → SSH, `wayvnc`/`x11vnc` → VNC). An LLM (DS4) section appears when ROCm/AI packages are selected.

## Fan control (AXB35)

After installing `synopsis-ec-su-axb35-linux`:

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
