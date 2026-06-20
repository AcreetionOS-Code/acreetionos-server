# 🖥️ AcreetionOS Server

**Headless server variant of AcreetionOS** — minimal, hardened, deployment-ready.

Built from the same archiso foundation as the desktop variant, then stripped of all GUI
components and packed with server-oriented tooling.

## 🎯 Purpose

AcreetionOS Server exists because I trust our OS enough to deploy it on our own
infrastructure. No bloat, no X11/Wayland, no display manager — just a clean Arch base
with the tools you actually need to run servers.

## 📦 What's Included

| Category | Packages |
|----------|----------|
| **Web Serving** | nginx-mainline, apache, certbot |
| **Databases** | mariadb, postgresql, redis, sqlite |
| **Containers** | docker, docker-compose, containerd, buildkit |
| **Monitoring** | prometheus, grafana, node_exporter, netdata |
| **Networking** | bind, dnsmasq, wireguard-tools, nftables, frr, bird |
| **Cloud Init** | cloud-init, cloud-guest-utils |
| **Remote Access** | openssh, mosh, tigervnc |
| **Backup** | restic, borg |
| **Virtualization** | qemu-desktop, libvirt, edk2-ovmf |

## 🚀 Build

```bash
./build.sh
```

Output ISO lands in `../ISO/`.

## 🔄 Mirrors

| Platform | URL |
|----------|-----|
| GitHub (AcreetionOS-Code) | https://github.com/AcreetionOS-Code/acreetionos-server |
| GitHub (spivanatalie64) | https://github.com/spivanatalie64/acreetionos-server |
| GitLab.com | https://gitlab.com/sprungles/acreetionos-server |
| GitLab (self-hosted) | https://gitlab.acreetionos.org/natalie/acreetionos-server |
| Codeberg | https://codeberg.org/sprunglesontheberg/acreetionos-server |

## 📄 License

GPL-3.0 — see [LICENSE](LICENSE).
