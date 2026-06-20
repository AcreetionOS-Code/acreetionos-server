# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

AcreetionOS Server is a server variant of AcreetionOS — an Arch Linux-based distribution that creates a bootable ISO using the archiso framework. Unlike the desktop variant, this ISO **auto-installs itself** to disk on boot. After installation, the user is presented with a **Cinnamon desktop environment** pre-loaded with server-oriented tooling — similar to Windows Server with Desktop Experience.

Think of it as: boot → wait 10 minutes → reboot → server desktop ready.

## Key Concept: Auto-Installation

When the ISO boots:
1. The live environment starts with root autologin on tty1
2. A systemd service (`acreetionos-server-installer.service`) triggers automatically
3. The installer (`/usr/local/bin/acreetionos-server-installer`) runs:
   - Detects the first available non-ISO disk
   - Creates GPT partition layout (1GB EFI + rest as ext4)
   - Installs base system via pacstrap
   - Configures locale, hostname, users, bootloader, services
   - Installs all packages (Cinnamon desktop + server tools)
   - Enables lightdm and graphical.target
   - Reboots

After reboot, login with: `admin` / `acreetion` (or `root` / `acreetion`)

## Build Commands

### Primary Build Process
- **Full build**: `./build.sh` — Cleans workspace and builds ISO
- **Manual build**: `./mkarchiso.sh` — Runs mkarchiso directly
- **Clean workspace**: `./refresh.sh` — Removes work/ and out/ directories

### Build Process Details
1. `refresh.sh` removes previous build artifacts (work/, out/)
2. `mkarchiso.sh` calls the archiso build system with AcreetionOS-Server label
3. Final ISO is output to `../ISO/` directory
4. Build uses custom `pacman.conf` for package management

## Architecture

### Key Configuration Files
- **profiledef.sh**: ISO metadata, boot modes, file permissions
- **packages.x86_64**: Package list — Cinnamon desktop + server tooling
- **pacman.conf**: Custom Pacman configuration
- **bootstrap_packages.x86_64**: Bootstrap packages for initial system

### Auto-Installer Components
- **airootfs/usr/local/bin/acreetionos-server-installer**: Main install script (partition, pacstrap, configure, reboot)
- **airootfs/usr/local/bin/acreetionos-server-display**: tty1 display script (shows progress during install)
- **airootfs/etc/systemd/system/acreetionos-server-installer.service**: Systemd service that triggers installer on boot
- **airootfs/root/.automated_script.sh**: Archiso script mechanism (can also trigger installer via kernel param)

### What's Included
- **Desktop**: Cinnamon (core components), LightDM, Xorg, PipeWire
- **Web**: nginx-mainline, apache, certbot
- **Databases**: mariadb, postgresql, redis, sqlite
- **Containers**: docker, docker-compose, containerd, buildkit
- **Monitoring**: prometheus, grafana, node_exporter, netdata
- **Networking**: bind, dnsmasq, wireguard-tools, nftables
- **Cloud**: cloud-init, cloud-guest-utils
- **Virtualization**: qemu, libvirt, edk2-ovmf
- **Backup**: restic, borg
- **Dev**: base-devel, git, go, rust, nodejs, python

## Development Notes

- Build process requires sudo privileges for archiso operations
- ISO builds are resource-intensive and create large work directories
- Package list can be modified by editing packages.x86_64
- The auto-installer is in `/usr/local/bin/acreetionos-server-installer` in airootfs
- Default credentials: `admin`/`acreetion` — **change on first login**
- Forked from AcreetionOS desktop — see parent for desktop variant
- Mirrored across GitHub, GitLab, and Codeberg
