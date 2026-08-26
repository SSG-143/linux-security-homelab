<div align="center">

<pre>
███████╗███████╗ ██████╗    ██╗      █████╗ ██████╗
 ██╔════╝██╔════╝██╔════╝    ██║     ██╔══██╗██╔══██╗
 ███████╗█████╗  ██║         ██║     ███████║██████╔╝
 ╚════██║██╔══╝  ██║         ██║     ██╔══██║██╔══██╗
 ███████║███████╗╚██████╗    ███████╗██║  ██║██████╔╝
 ╚══════╝╚══════╝ ╚═════╝    ╚══════╝╚═╝  ╚═╝╚═════╝
</pre>

# 🛡️ Linux Security Homelab

> *A self-hosted Linux server, configured and hardened from the ground up — not a tutorial clone, but a real box I run, break, monitor, and fix.*

<br>

[![OS](https://img.shields.io/badge/Athena%20OS-Arch%20Based-1793d1?style=for-the-badge&logo=archlinux&logoColor=white)](https://athenaos.org)
[![Firewall](https://img.shields.io/badge/Firewall-Firewalld-e94560?style=for-the-badge&logo=linux&logoColor=white)]()
[![Docker](https://img.shields.io/badge/Containers-Docker-2496ed?style=for-the-badge&logo=docker&logoColor=white)]()
[![Cockpit](https://img.shields.io/badge/Admin%20UI-Cockpit-0078d4?style=for-the-badge&logo=linux&logoColor=white)]()
[![SSH](https://img.shields.io/badge/Remote-SSH-4caf50?style=for-the-badge&logo=openssh&logoColor=white)]()
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()
[![Type](https://img.shields.io/badge/Type-Blue%20Team%20%2F%20Sysadmin-blueviolet?style=for-the-badge)]()

</div>

---

## 📖 Overview

<div align="center">

|  |  |
|---|---|
| **OS** | Athena OS (Arch-based), Linux 7.0.12 |
| **Hardware** | Acer Veriton M200-H110, Intel i3-6100T, 12 GB RAM |
| **Access** | SSH (key-based), Cockpit web console |
| **Core services** | Firewalld, Docker, Cockpit, SSH, centralized logging |
| **Hosted apps** | Pi-hole, Plex, custom project containers |

</div>

The box is administered two ways: **SSH** from the terminal for day-to-day work, and **Cockpit** (web console, port 9090) for monitoring, log review, and network configuration at a glance.

<div align="center">

![Cockpit system overview](screenshots/04-cockpit-overview.jpg)

</div>

---

## 🗺️ Project Architecture

```
                        ┌──────────────────────────────────┐
                        │        🖥️  Athena OS Server      │
                        │                                  │
          Browser ────► │  🌐 Cockpit Web UI  (Port 9090)  │
                        │                                  │
          SSH ────────► │  🔑 SSH Daemon      (key-only)   │
                        │                                  │
                        │  🐳 Docker (bridge: docker0)      │
                        │     ├─ Pi-hole                   │
                        │     ├─ Plex                      │
                        │     └─ project containers        │
                        │                                  │
                        │  📋 Centralized Logging          │
                        │     ├─ journalctl -f             │
                        │     └─ Cockpit log viewer        │
                        │                                  │
                        │  ════════════════════════════    │
                        │  🔥 FIREWALLD  (3 active zones)  │
                        │     └─ granular, per-interface   │
                        └──────────────────────────────────┘
```

---

## 🎯 Key Areas of Work

### 🔥 1. Firewall

Firewalld runs with **3 active zones**, giving granular control over what's reachable and from where — rather than one flat set of rules for every interface.

<div align="center">

![Networking and firewall status](screenshots/06-cockpit-networking.jpg)

</div>

---

### 🔐 2. Access Control

- **SSH access is key-based** — password auth is disabled for remote login
- **User permissions are scoped per-service** — containerized apps (Pi-hole, Plex) run under their own accounts, not root, limiting blast radius if any one service is compromised
- **Failed login attempts are visible directly on the Cockpit dashboard**, so unusual access attempts don't go unnoticed

---

### 📊 3. Logging & Monitoring

All system logs are centralized and filterable through Cockpit — by priority, service, or time range — instead of digging through raw log files over SSH for every check.

<div align="center">

![Centralized log monitoring](screenshots/05-cockpit-logs.jpg)

</div>

Live logs are also tailed directly via `journalctl -f` to catch misconfigurations early (e.g. duplicate D-Bus service names, deprecated policies) rather than letting warnings go unread.

<div align="center">

![System boot log](screenshots/03-boot-log.jpg)

</div>

---

### 🐳 4. Isolation

Docker handles containerized services on their own bridge network (`docker0`, `172.17.0.1/16`), separating application traffic from the host's LAN-facing interface.

---

### 🗂️ 5. Server Layout

Standard structure kept clean and predictable — home directory, a dedicated `Projects` folder for work in progress, and separate root-owned directories for long-running services (`pihole`, `plex`) to keep service data isolated from user data.

<div align="center">

![Server file structure](screenshots/02-file-structure.jpg)

</div>

Remote file access is available over FTP for quick transfers — though as Windows itself warns, FTP is unencrypted, so this is being migrated to **WebDAV/SFTP-only** access.

<div align="center">

![FTP access prompt](screenshots/01-ftp-login.jpg)

</div>

---

## 🔐 Security Principles Applied

```
┌─────────────────────────────────────────────────────────┐
│                  SECURITY PRINCIPLES                    │
├──────────────────────┬──────────────────────────────────┤
│  Least Privilege     │  Services run under their own    │
│                      │  accounts, never root             │
├──────────────────────┼──────────────────────────────────┤
│  Zone-Based Firewall │  3 firewalld zones instead of     │
│                      │  one flat rule set                │
├──────────────────────┼──────────────────────────────────┤
│  Visibility          │  Centralized logs + live tailing  │
│                      │  to catch issues early            │
├──────────────────────┼──────────────────────────────────┤
│  Network Isolation   │  Docker bridge network keeps      │
│                      │  app traffic off the LAN iface    │
├──────────────────────┼──────────────────────────────────┤
│  Encrypted Access    │  Key-based SSH; FTP being retired │
│                      │  in favor of WebDAV/SFTP          │
└──────────────────────┴──────────────────────────────────┘
```

---

## 📚 What This Project Is For

This lab exists to practice things that don't show up in tutorials:

- Actually hardening a box, not just installing software on top of defaults
- Reading logs and boot output critically instead of ignoring warnings
- Structuring firewall zones and container isolation with intent
- Documenting a real system the way it'd need to be documented on a real job

---

## ✅ Skills Demonstrated

```
 🐧  Linux system administration on a real Arch-based machine
 🔥  Zone-based firewall configuration with Firewalld
 🔐  Key-based SSH access control and per-service permissions
 📊  Centralized log monitoring and live journalctl tailing
 🐳  Docker container isolation via dedicated bridge network
 🌐  Web-based server administration via Cockpit
 🗂️  Clean, predictable server/file layout
 🛡️  Security hardening and blue-team thinking
```

---

<div align="center">

---

*Part of my broader cybersecurity portfolio — see [profile README](https://github.com/SSG-143) for other projects.*

[![Athena OS](https://img.shields.io/badge/Built%20on-Athena%20OS-1793d1?style=flat-square&logo=archlinux&logoColor=white)](https://athenaos.org)
[![Blue Team](https://img.shields.io/badge/Focus-Blue%20Team%20%7C%20Hardening-blue?style=flat-square)]()
[![Learning](https://img.shields.io/badge/Type-Homelab%20%7C%20Portfolio-orange?style=flat-square)]()

</div>
