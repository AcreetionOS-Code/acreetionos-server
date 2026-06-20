# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AcreetionOS Server is a headless server variant of AcreetionOS — an Arch Linux-based distribution that creates bootable ISO images using the archiso framework. It strips away all desktop environment components (Cinnamon, Xorg, audio, Calamares GUI) and replaces them with server-oriented tooling: web servers, databases, container runtimes, monitoring stacks, and cloud-init provisioning. Designed for deployment on bare metal, VMs, and cloud infrastructure.

## Build Commands

### Primary Build Process
- **Full build**: `./build.sh` - Cleans workspace and builds ISO
- **Manual build**: `./mkarchiso.sh` - Runs mkarchiso directly
- **Clean workspace**: `./refresh.sh` - Removes work/ and out/ directories

### Build Process Details
1. `refresh.sh` removes previous build artifacts (work/, out/)
2. `mkarchiso.sh` calls the archiso build system with AcreetionOS-Server label
3. Final ISO is output to `../ISO/` directory
4. Build uses custom `pacman.conf` for package management

## Architecture

### Key Configuration Files
- **profiledef.sh**: Main archiso profile configuration defining ISO metadata, boot modes, and file permissions
- **packages.x86_64**: Server-optimized package list (no desktop, focused on server/deployment tooling)
- **pacman.conf**: Custom Pacman configuration for package management
- **bootstrap_packages.x86_64**: Bootstrap packages for initial system

### Directory Structure
- **airootfs/**: Root filesystem overlay that becomes the live server system
  - `etc/`: System configuration files
  - `usr/`: User binaries, scripts, and customizations
  - `root/`: Root user files and installation scripts
- **grub/**: GRUB bootloader configuration
- **syslinux/**: SYSLINUX bootloader configuration
- **efiboot/**: EFI boot configuration

### Server Features
- Minimal headless ISO with no X11/Wayland or desktop components
- Built-in: nginx, apache, mariadb, postgresql, redis, docker, docker-compose
- Monitoring: prometheus, grafana, node_exporter, netdata
- Networking: bind, dnsmasq, wireguard, frr, bird, nftables
- Cloud-ready: cloud-init, cloud-guest-utils
- Remote management: openssh, mosh, tigervnc
- Backup: restic, borg

## Deployment Targets
- Bare metal servers
- KVM/QEMU/libvirt VMs
- Cloud providers (via cloud-init)
- Docker/container hosts
- Database servers
- Web/application servers
- Network appliances (routers, firewalls)
- Monitoring infrastructure

## Development Notes

- Build process requires sudo privileges for archiso operations
- ISO builds are resource-intensive and create large work directories
- Package list can be modified by editing packages.x86_64
- Custom scripts and configurations go in airootfs/ overlay
- File permissions are explicitly defined in profiledef.sh
- Forked from AcreetionOS desktop — see parent project for desktop variant
- All repos are mirrored across GitHub, GitLab, and Codeberg
