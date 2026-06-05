<div align="center">
```
███████╗███████╗ ██████╗    ██╗      █████╗ ██████╗
██╔════╝██╔════╝██╔════╝    ██║     ██╔══██╗██╔══██╗
███████╗█████╗  ██║         ██║     ███████║██████╔╝
╚════██║██╔══╝  ██║         ██║     ██╔══██║██╔══██╗
███████║███████╗╚██████╗    ███████╗██║  ██║██████╔╝
╚══════╝╚══════╝ ╚═════╝    ╚══════╝╚═╝  ╚═╝╚═════╝
```

# 🛡️ Linux Security Homelab

> *A hands-on cybersecurity and system administration project — built from scratch on a real machine.*

<br/>

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

**Linux Security Homelab** is a personal cybersecurity and system administration project built by repurposing an old Linux desktop into a fully self-managed, security-focused server environment.

The machine originally ran **Athena OS** (an Arch-based cybersecurity-oriented distribution) with a full **Hyprland** desktop environment. It was later transitioned into a headless server — with services configured, users managed, firewall rules written, and logs actively monitored.

> Unlike just "learning Kali Linux tools," this project involved actually **owning and securing a real system** — configuring it from the ground up, making deliberate security decisions, and understanding why each setting matters.

This project reflects practical skills directly relevant to **Log Analysis**, **Linux System Administrator**, and **Blue Team** roles.

---

## 🛠️ Technologies Used

<div align="center">

| Category | Technology | Purpose |
|----------|-----------|---------|
| 🐧 **Operating System** | Athena OS (Arch-based Linux) | Security-focused distro, rolling release |
| 🌐 **Server Admin UI** | Cockpit | Browser-based server management & log viewing |
| 🔥 **Firewall** | Firewalld | Dynamic firewall daemon, zone-based rules |
| 🐳 **Containers** | Docker | Isolated service deployment |
| ⚙️ **Init System** | Systemd | Service lifecycle and boot management |
| 🔑 **Remote Access** | SSH | Encrypted terminal access |
| 📋 **Log Tools** | Journalctl / /var/log | System and authentication log analysis |

</div>

---

## 🗺️ Project Architecture

```
                        ┌──────────────────────────────────┐
                        │        🖥️  Athena OS Server        │
                        │                                  │
          Browser ────► │  🌐 Cockpit Web UI  (Port 9090)  │
                        │                                  │
          SSH ────────► │  🔑 SSH Daemon      (Port 22)    │
                        │                                  │
                        │  🐳 Docker Services              │
                        │     └─ Containerized apps        │
                        │                                  │
                        │  ⚙️  Systemd                      │
                        │     └─ Service management        │
                        │                                  │
                        │  📋 Log Monitoring               │
                        │     ├─ journalctl                │
                        │     └─ /var/log/                 │
                        │                                  │
                        │  ════════════════════════════    │
                        │  🔥 FIREWALLD  (Active Gate)     │
                        │     ✅ SSH allowed               │
                        │     ✅ DHCPv6 allowed            │
                        │     ❌ Everything else blocked   │
                        └──────────────────────────────────┘
```

---

## 🎯 Key Areas of Work

### 🔥 1. Firewall Management

Configured **Firewalld** — a dynamic, zone-based firewall daemon — to control all inbound and outbound network traffic on the server.

- Applied **default-deny** policy: all ports are blocked unless explicitly allowed
- Managed **zones** to define trust levels for different network interfaces
- Allowed only the minimum necessary services: `SSH` and `DHCPv6`
- Understood how Firewalld interacts with Docker networking and iptables chains underneath

> 🔍 *This reflects how firewalls are managed in real Linux server environments — not just toggling UFW on/off, but understanding zone-based policy.*

---

### 👥 2. User & Permission Management

Set up and managed **users, groups, and file permissions** following the principle of least privilege.

- Created system users with restricted shell access and limited sudo rights
- Configured **file and directory permissions** (`chmod`, `chown`) to prevent unauthorized access
- Understood the difference between regular users, service accounts, and root
- Managed which users can perform administrative actions and under what conditions

> 🔍 *Improper user permissions are one of the most common misconfigurations found during security audits. Doing this manually on a real system builds real intuition.*

---

### 📊 3. Log Monitoring & Analysis

Actively monitored the server's **system and authentication logs** to identify suspicious activity and understand what normal vs. abnormal behaviour looks like.

- Used `journalctl` to query systemd logs by service, time range, and priority
- Reviewed `/var/log/auth.log` and related files for failed SSH logins and privilege escalations
- Used **Cockpit's** built-in log viewer as a real-time browser-based monitoring dashboard
- Learned to distinguish noise (routine events) from signals (potential incidents)

> 🔍 *Log analysis is the core daily skill of a SOC analyst. Reading real logs from a real system — not a lab simulation — builds the pattern recognition that matters.*

---

### 🐳 4. Docker & Container Management

Deployed and managed services using **Docker** containers on the server.

- Pulled, configured, and ran containerized applications
- Managed container networking and understood how Docker interacts with the host firewall
- Used **Systemd** to manage Docker as a service and ensure it starts on boot
- Administered running containers through both CLI and Cockpit

> 🔍 *Containers are everywhere in modern infrastructure. Understanding how they run, how they expose ports, and how they interact with host-level security controls is essential.*

---

### 🌐 5. Server Administration via Cockpit

Used **Cockpit** — a web-based Linux server management tool — as the primary interface for day-to-day server management.

- Monitored CPU, memory, disk, and network usage in real time
- Managed services (start/stop/enable/disable) through the UI
- Reviewed system logs without needing to be at the terminal
- Managed storage and system configuration from a browser

> 🔍 *Cockpit is used in real enterprise Linux environments. Knowing how to navigate and administer a server through it is a practical, job-relevant skill.*

---

## 🔐 Security Principles Applied

```
┌─────────────────────────────────────────────────────────┐
│                  SECURITY PRINCIPLES                    │
├──────────────────────┬──────────────────────────────────┤
│  Least Privilege     │  Users and services get only     │
│                      │  the access they actually need   │
├──────────────────────┼──────────────────────────────────┤
│  Default Deny        │  Firewall blocks everything      │
│                      │  unless explicitly permitted     │
├──────────────────────┼──────────────────────────────────┤
│  Visibility          │  Logs monitored to detect and    │
│                      │  review suspicious activity      │
├──────────────────────┼──────────────────────────────────┤
│  Defense in Depth    │  Multiple layers: firewall +     │
│                      │  permissions + monitoring        │
└──────────────────────┴──────────────────────────────────┘
```

---

## 📂 Repository Structure

```
linux-security-homelab/
│
├── 📄 README.md                  ← Project overview (you are here)
│
├── 🔥 firewall/
│   └── README.md                 ← Firewall rules & configuration walkthrough
│
├── 👥 users/
│   └── README.md                 ← User management & permission setup
│
├── 📊 logs/
│   └── README.md                 ← Log monitoring process & findings
│
├── 🐳 docker/
│   └── README.md                 ← Docker setup & container management
│
└── 🌐 cockpit/
    └── README.md                 ← Cockpit setup & administration notes
```

---

## ✅ Skills Demonstrated

```
 🐧  Linux system administration on a real Arch-based machine
 🔥  Firewall configuration and zone-based policy management
 👥  User and group management with least-privilege principles
 📊  System and authentication log monitoring and analysis
 🐳  Docker container deployment and service management
 🔑  SSH administration and remote server access
 🌐  Web-based server administration via Cockpit
 🛡️  Security hardening and blue-team thinking
```

---

<div align="center">

---

*This project is part of an ongoing cybersecurity learning journey.*
*Each folder in this repository documents a specific area of work with configurations, commands used, and findings.*

[![Athena OS](https://img.shields.io/badge/Built%20on-Athena%20OS-1793d1?style=flat-square&logo=archlinux&logoColor=white)](https://athenaos.org)
[![Blue Team](https://img.shields.io/badge/Focus-Blue%20Team%20%7C%20Hardening-blue?style=flat-square)]()
[![Learning](https://img.shields.io/badge/Type-Homelab%20%7C%20Portfolio-orange?style=flat-square)]()

</div>
