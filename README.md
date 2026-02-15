PAQWRT - Professional Paqet Manager for OpenWrt

<div align="center">

The ultimate all-in-one management tool for running Paqet on OpenWrt routers.





Features Smart Network Discovery, NFTables Bypass, and Zero-Lag Optimization.

</div>

📖 Introduction

PAQWRT is a robust shell script wrapper designed to deploy, configure, and manage the paqet raw-socket tunnel on OpenWrt devices. Unlike generic Linux scripts, this tool is engineered specifically for the constraints and architecture of embedded routers.

It handles complex tasks like NFTables firewall injection, WAN IP binding, MAC address discovery, and Service management (Procd) automatically, ensuring a plug-and-play experience.

✨ Key Features

🚀 Automated Deployment: Installs dependencies (libpcap, kmod-nft-bridge, etc.) and detects CPU architecture (x86_64, aarch64, armv7).

🛡️ Router-Level Bypass: Uses nftables with negative priority hooks (-300) to bypass connection tracking (NOTRACK), preventing the router from interfering with raw packets.

⚡ Zero-Lag Optimization: Includes MSS Clamping rules to fix loading issues with heavy websites (e.g., Swagger UIs, Dashboards) caused by MTU fragmentation.

🧠 Smart Configuration:

Default Mode: Applies stability-focused settings (conn: 1, interval: 10) ideal for most users.

Manual Mode: Allows granular control over Connections, MTU, SndWnd, RcvWnd, and buffers for power users.

🔄 Auto-Healing Service: Uses OpenWrt's native procd system to automatically restart the service if it crashes or if the router reboots.

📡 Dynamic Network Discovery: Automatically detects the WAN Interface, Gateway IP, and Gateway MAC address on every service start.

📦 Installation

You can install PAQWRT with a single command. SSH into your OpenWrt router and run:

sh -c "$(curl -sL [https://raw.githubusercontent.com/bolandi-org/paqwrt/main/paqwrt](https://raw.githubusercontent.com/bolandi-org/paqwrt/main/paqwrt))" -- install


Note: This command installs the script to /usr/bin/paqwrt and makes it executable.

🚀 Usage Guide

To open the interactive management menu, simply type:

paqwrt


Step-by-Step Setup

Install Core:

Select Option 1 (Install / Update Binary).

The script will download the latest compatible paqet binary from GitHub and fix library symlinks (musl vs glibc).

Configuration:

Select Option 2 (Configure / Modify Settings).

Enter your Server IP:Port and Encryption Key.

Choose your mode:

1) Default: Best for stability. Sets Connections: 1, MTU: 1350, Interval: 10.

2) Manual: Allows you to define custom values. Press Enter to accept optimized defaults or type your own values (e.g., increase connections to 6 for high-speed fiber).

Start Service:

Select Option 3 (Start Service).

The tool will apply Firewall rules and start the background daemon.

⚙️ Configuration Modes

🟢 Default Mode (Recommended)

Optimized for stability on routers with limited RAM (<512MB).

Connections: 1

MTU: 1350

Interval: 10ms (High Responsiveness)

Buffers: Standard

🟡 Manual Mode (Advanced)

For users with powerful routers (e.g., NanoPi, Raspberry Pi, High-end Xiaomi) or Fiber connections.

Connections: Adjustable (Recommended: 4-8 for high speed).

Buffers: Adjustable sndwnd/rcvwnd (Increase for higher throughput).

Interval: Adjustable (Lower = Lower Latency, Higher = Lower CPU usage).

🔧 Technical Details (How it works)

Firewall Injection (NFTables)

PAQWRT injects rules into the inet table with high priority to ensure packets are processed before the main firewall (fw4).

# Example of injected rules
chain prerouting { type filter hook prerouting priority -300; }
chain output     { type filter hook output priority -300; }
chain forward    { type filter hook forward priority -150; }

# MSS Clamping to fix fragmentation issues
rule forward tcp flags syn tcp option maxseg size set 1300

# NOTRACK to bypass conntrack for raw sockets
rule prerouting tcp sport 1443 tcp dport 55555 notrack


Service Management (Procd)

The service file /etc/init.d/paqet handles the lifecycle. It includes a Watchdog logic that waits for the WAN interface to receive an IP address before starting the tunnel, preventing boot-loops.

🛠 Troubleshooting

1. Service status shows "not running":
Check the logs using Option 6 in the menu.
Common cause: Missing libpcap or library mismatches. Run Option 1 again to fix symlinks.

2. Speed is slow:

Try Manual Mode and increase Connections to 4 or 6.

Ensure your ISP supports the configured MTU (Default 1350 is safe for most).

3. Specific websites (like Swagger) don't load:
This is an MTU/MSS issue. PAQWRT automatically applies MSS Clamping (1300). If issues persist, try lowering the MTU in Manual Mode.

📝 License

This project is licensed under the MIT License.
Based on the core logic of paqet and paqctl.