# PaqWrt - Professional Paqet Manager for OpenWrt

[![OpenWrt](https://img.shields.io/badge/Platform-OpenWrt-blue?logo=openwrt)](https://openwrt.org/)
[![Bash](https://img.shields.io/badge/Language-Bash-green?logo=gnu-bash)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/badge/License-MIT-orange)](https://opensource.org/licenses/MIT)
![Version](https://img.shields.io/badge/Version-2.2.1-red)

**The ultimate all-in-one management tool for deploying [Paqet](https://github.com/hanselime/paqet) tunnels on OpenWrt routers.**

---

## � Installation

### 1. Prerequisites (Mandatory)
Before running the installation command, ensure that `curl` is installed on your router. If it's not installed, run:

```bash
opkg update && opkg install curl
```

### 2. Quick Install
Run the following command to download the manager and start it:

```bash
curl -L "https://raw.githubusercontent.com/bolandi-org/paqwrt/main/paqwrt" -o /usr/bin/paqwrt && chmod +x /usr/bin/paqwrt && paqwrt install
```

---

## 🛠 Usage & Features

After installation, simply type `paqwrt` in your terminal to open the interactive menu.

1.  **Install / Update Core:** Downloads the latest Paqet binary matching your CPU architecture (Auto-detected).
2.  **Configure:** Setup your server IP:Port, encryption key, and performance settings.
3.  **Manage Service:** Start, Stop, or Restart the tunnel.
4.  **Logging:** Real-time system logs for troubleshooting.

### Integration with other Apps (PassWall / v2rayA)
Paqwrt starts a SOCKS5 proxy on port **1080**. You can use it as an upstream proxy in other OpenWrt apps:

*   **PassWall:** Go to `Node Settings` -> `Add` -> Select Type `SOCKS5` -> IP `127.0.0.1` -> Port `1080`.
*   **v2rayA/OpenClash:** Use `127.0.0.1:1080` as your proxy source to route all router traffic through Paqet.

---

## 💎 Design Philosophy
*   **Architecture Detection:** Smartly detects MIPS, ARM, x86, and their Endianness.
*   **Self-Healing Config:** Automatically repairs configuration blocks if corrupted.
*   **KCP Optimization:** Pre-configured with "Optimized/Stable" and "Manual" modes.
*   **Firewall Bypass:** Uses `NFTables` to bypass `conntrack`, ensuring high speed and low CPU usage.

---

# 🇮🇷 راهنمای فارسی (Persian Documentation)

**پک‌وِرت (PaqWrt) - ابزار حرفه‌ای مدیریت تونل Paqet روی روترهای OpenWrt**

---

## 🚀 نصب و راه‌اندازی

### ۱. پیش‌نیازها (اجباری)
قبل از اجرای کد نصب، مطمئن شوید ابزار `curl` روی روتر شما نصب است. اگر نصب نیست، دستور زیر را اجرا کنید:

```bash
opkg update && opkg install curl
```

### ۲. نصب سریع
دستور زیر را کپی کرده و در ترمینال روتر خود اجرا کنید:

```bash
curl -L "https://raw.githubusercontent.com/bolandi-org/paqwrt/main/paqwrt" -o /usr/bin/paqwrt && chmod +x /usr/bin/paqwrt && paqwrt install
```

---

## � راهنمای استفاده و قابلیت‌ها

پس از نصب، کافیست کلمه `paqwrt` را در ترمینال بنویسید تا منوی مدیریت باز شود.

1.  **بروزرسانی هسته:** شناسایی خودکار نوع پردازنده روتر و دانلود آخرین نسخه Paqet.
2.  **تنظیمات:** وارد کردن آی‌پی سرور، کلید رمزنگاری و تنظیمات سرعت.
3.  **مدیریت سرویس:** استارت، استاپ و ریستارت برنامه.
4.  **بررسی لاگ:** مشاهده وضعیت لحظه‌ای و عیب‌یابی.

### اتصال به PassWall و v2rayA
برنامه PaqWrt یک پروکسی **SOCKS5** روی پورت **1080** اجرا می‌کند. شما می‌توانید از این پورت در برنامه‌های دیگر استفاده کنید:

*   **PassWall:** یک نود جدید از نوع `SOCKS5` بسازید، آی‌پی را `127.0.0.1` و پورت را `1080` بگذارید.
*   **v2rayA:** از آدرس `127.0.0.1:1080` به عنوان منبع پروکسی استفاده کنید تا تمام اینترنت روتر از تونل عبور کند.

---

## � قابلیت‌های فنی
*   **تشخیص هوشمند معماری:** پشتیبانی از تمام متغیرهای MIPS، ARM و x86.
*   **رفع خطای خودکار:** اصلاح برچسب‌های کانفیگ در صورت خرابی فایل.
*   **بهینه‌سازی KCP:** دارای مدهای "پیش‌فرض" و "دستی" برای حداکثر سرعت.
*   **فایروال پیشرفته:** استفاده از `NFTables` برای کاهش فشار به CPU و افزایش پایداری اتصال.

---

## � License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
