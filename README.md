# PAQWRT - Professional Paqet Manager for OpenWrt

<div align="center">

![OpenWrt](https://img.shields.io/badge/Platform-OpenWrt-blue?logo=openwrt)
![Bash](https://img.shields.io/badge/Language-Bash-green?logo=gnu-bash)
![License](https://img.shields.io/badge/License-MIT-orange)
![Version](https://img.shields.io/badge/Version-2.1.0-red)

**The ultimate all-in-one management tool for deploying [Paqet](https://github.com/hanselime/paqet) tunnels on OpenWrt routers.**
<br>
Features **Smart Network Discovery**, **NFTables Bypass**, and **Zero-Lag Optimization**.

</div>

---

## 📖 Introduction

**PAQWRT** is a robust shell script wrapper designed to automate the deployment, configuration, and management of the `paqet` raw-socket tunnel on OpenWrt devices. It is engineered specifically for the constraints of embedded routers, handling complex tasks like firewall injection and service management automatically.

### ✨ Key Features

* **🚀 One-Command Deployment:** Installs dependencies (`libpcap`, `kmod-nft-bridge`, etc.) and detects CPU architecture (`x86_64`, `aarch64`, `armv7`) automatically.
* **🔄 Auto-Update:** Fetches the latest core binary release directly from the upstream repository.
* **🛡️ Router-Level Bypass:** Uses `nftables` with negative priority hooks (`-300`) to bypass connection tracking (`NOTRACK`), ensuring raw packet integrity.
* **⚡ Zero-Lag Optimization:** Automatically applies **MSS Clamping** rules to fix loading issues with heavy websites (e.g., Swagger UIs, Dashboards).
* **🧠 Smart Configuration Modes:**
    * **Default Mode:** Applies stability-focused settings (`conn: 1`, `interval: 10`) ideal for most users.
    * **Manual Mode:** Allows granular control over `Connections`, `MTU`, `SndWnd`, `RcvWnd`, and buffers. Supports "Enter for Default" workflow.
* **📡 Dynamic Network Discovery:** Automatically detects WAN IP and Gateway MAC address on every boot to handle dynamic IPs.

---

## 📦 Installation

SSH into your OpenWrt router and run this single command:

```bash
sh -c "$(curl -sL https://raw.githubusercontent.com/bolandi-org/paqwrt/main/paqwrt)" -- install
```

> **Note:** This command installs the script to `/usr/bin/paqwrt` and makes it executable.

## 🚀 Usage Guide

To open the interactive management menu, simply type:

```bash
paqwrt
```

### Configuration Steps

1.  **Install Core:** Select Option 1 to download the latest binary.
2.  **Configure:** Select Option 2.
    *   Enter your **Server IP:Port** and **Encryption Key**.
    *   **Choose Mode:**
        *   **Default:** Sets Connections: 1, MTU: 1350, Interval: 10.
        *   **Manual:** Prompts for each value. Press Enter to use the recommended default for any setting.
3.  **Start:** Select Option 3 to apply firewall rules and start the tunnel.

---

## ⚙️ Advanced Configuration

| Parameter | Default | Description |
| :--- | :--- | :--- |
| **Connections** | 1 | Number of simultaneous TCP connections. Increase to 4-6 for high-speed fiber. |
| **Interval** | 10 | Internal update interval (ms). Lower is more responsive but uses more CPU. |
| **MTU** | 1350 | Maximum Transmission Unit. Lower if you experience packet loss. |
| **SndWnd** | 350 | Upload window size. Controls upload throughput. |
| **RcvWnd** | 450 | Download window size. Controls download throughput. |

---

## 🔧 How It Works

### Firewall Injection (NFTables)
PAQWRT injects rules into the `inet` table with high priority to ensure packets are processed before the main firewall (fw4).

```nftables
# MSS Clamping to fix fragmentation
rule forward tcp flags syn tcp option maxseg size set 1300

# NOTRACK to bypass conntrack for raw sockets
rule prerouting tcp sport 1443 tcp dport 55555 notrack
```

### Service Management (Procd)
The service file `/etc/init.d/paqet` handles the lifecycle. It includes a **Watchdog** logic that waits for the WAN interface to receive an IP address before starting the tunnel, preventing boot-loops.

---

## 📝 License

This project is licensed under the MIT License.
Based on the core logic of [paqet](https://github.com/hanselime/paqet).
