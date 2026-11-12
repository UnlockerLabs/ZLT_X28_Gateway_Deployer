# ZLT X28 5G Gateway Deployer — Unified Master Suite (v3.0)

An all-in-one Windows desktop deployment tool (`ZLT_X28_Gateway_Deployer.exe`) for **ZLT X28 / X28 Pro 5G CPE** routers (MediaTek T750 / ARM64 `aarch64`), combining all 14 operational modules into a **single unified deployment** with persistent storage across firmware upgrades and reboots.

> [!IMPORTANT]
> **Strict Compatibility & Prerequisites**
> - **Supported Software Version:** `1.5.13` **ONLY**
> - **Supported Hardware Version:** `TZ7.823.432A` **ONLY**
> - **Required Router Access:** **SSH or Telnet must be enabled (`ON`)** before running deployment (supports SSH on ports `22`, `2222`, `10210` with legacy `ssh-rsa` SHA-1, automatic Telnet port `23` SSH recovery, and HTTP/HTTPS `cmd 409` SSH enablement).
> - Do **not** flash or deploy this build onto other hardware revisions or firmware versions.

---

## Key Features (All 14 Suites Combined)

### 1. AetherOS 3.0 Studio WebUI + 212 Hidden Hub
- **Obsidian Dark** & **Daylight Pro** dual-theme interface served on the main router gateway (`http://192.168.1.1/`).
- Integrated **212 Hidden Hub** (`router_hidden_hub.js`) exposing hidden router engineering menus and diagnostics directly inside the WebUI.
- **Optional Carrier Theme:** Leave unchecked (`keep`) by default, or optionally apply a custom carrier branding theme during deployment.

### 2. Full OpenWrt 19.07.10 LuCI + Web Terminal
- Full OpenWrt **LuCI Web Interface** running independently alongside the stock gateway stack:
  - HTTP: `http://192.168.1.1:4153/cgi-bin/luci/`
  - HTTPS: `https://192.168.1.1:4154/cgi-bin/luci/`
- Built-in **OpenWrt Web Terminal (`ttyd`)** on `http://192.168.1.1:7681/`.
- Includes `OPENWRT_WAN_GUARD` WAN/LAN firewall protection so router LAN access and login credentials remain intact.

### 3. Asad V2Ray / Xray Control + DataMon Pro
- Dedicated **Xray Control Dashboard** (`http://192.168.1.1/cgi-bin/xray.cgi`) with strict manual **Start / Stop** control by default (plus optional autostart toggle).
- **DataMon Pro Suite** (`http://192.168.1.1/cgi-bin/datamon.cgi`) for real-time bandwidth monitoring, per-client traffic tracking, and usage diagnostics.

### 4. Wi-Fi Repeater / WISP & Multi-WAN Load Balancer
- **Wi-Fi Repeater / WISP (`wifi_repeater.html`)** with dual-band `apcli0` / `apclii0` uplink support.
- **Multi-WAN Balancer (`mwan_balancer.html` / `/cgi-bin/mwan.cgi`)** for cellular + Wi-Fi/Ethernet failover and load balancing.

### 5. Cellular Intelligence, Carrier Juggler & RF Unlock
- **Cellular Intelligence & GPS Station** (`cellular_daemon.lua`, `cellular_station.html`) for live 4G/5G cell tower metrics, band locking, and signal telemetry.
- **Smart Carrier Auto-Juggler** (`carrier_juggler.sh`, `carrier_juggler.html`) for automated carrier/APN profile switching.
- **4G+ / 5G 4CC Carrier Aggregation + External SMA Antenna** enablement.
- **SIM / PLMN & X28-PRO ESMLCK Unlock** + **TIHSG ZLT Universal Unlocker**.

### 6. Security, Superadmin & Telemetry Hardening
- **Level-1 Superadmin Unlock** (`Superadmin` / `admin` WebUI access).
- **TR-069 / ACS / SNMP Blocker** to stop remote ISP provisioning, forced updates, and telemetry callbacks.
- **Safe Flash & Persistent Storage Architecture:** Stores `/data/sysupgrade_master.tgz`, `/data/boot_restore.sh`, and `/data/openwrt_luci.tar.gz` in `/data` while keeping the 10 MB `/mnt/data` `user_config` partition protected from overflow and preserving `/mnt/data/etc/tzcfg/main_config` and `/mnt/data/etc/tzcfg/apply_user_arp_binding_config`.

---

## Requirements Before Running

| Requirement | Value / Status |
| :--- | :--- |
| **Device** | ZLT X28 / X28 Pro 5G CPE |
| **Software Version** | `1.5.13` (**Only**) |
| **Hardware Version** | `TZ7.823.432A` (**Only**) |
| **SSH / Telnet** | **Must be ON** (SSH or Telnet reachable from PC) |
| **Host OS** | Windows 10 / 11 (Ethernet or Wi-Fi connected to `192.168.1.1`) |
| **Deployer Passcode** | `889911` |

---

## Default Endpoints & Access

| Service | URL | Credentials |
| :--- | :--- | :--- |
| **Main WebUI (AetherOS 3.0)** | `http://192.168.1.1/` | `Superadmin` / `admin` (or `admin` / `admin`) |
| **OpenWrt 19.07.10 LuCI (HTTP)** | `http://192.168.1.1:4153/cgi-bin/luci/` | Router root / admin credentials |
| **OpenWrt 19.07.10 LuCI (HTTPS)** | `https://192.168.1.1:4154/cgi-bin/luci/` | Router root / admin credentials |
| **OpenWrt Web Terminal (`ttyd`)** | `http://192.168.1.1:7681/` | Root shell |
| **Xray Control Dashboard** | `http://192.168.1.1/cgi-bin/xray.cgi` | Local WebUI |
| **DataMon Pro** | `http://192.168.1.1/cgi-bin/datamon.cgi` | Local WebUI |

---

## Quick Usage

1. Verify your router matches **Software Version `1.5.13`** and **Hardware Version `TZ7.823.432A`**, and confirm **SSH or Telnet is ON**.
2. Connect your Windows PC directly to the ZLT X28 LAN/Wi-Fi (`192.168.1.1`).
3. Launch `ZLT_X28_Gateway_Deployer.exe` and enter operator passcode `889911`.
4. Select **Unified ZLT X28 5G Master Project (All-in-One Single Deployment)**.
5. *(Optional)* Check **Apply Carrier Theme** if you want a carrier-branded skin; leave unchecked to keep the default AetherOS 3.0 Studio appearance.
6. Start deployment and wait for completion + reboot restoration.

---

## Building from Source (Windows)

Requires **Python 3.10+**:

```bat
pip install paramiko pillow pyinstaller cryptography
pyinstaller --noconfirm --clean --onefile --windowed --name "ZLT_X28_Gateway_Deployer" --icon "payload\app_icon.ico" --add-data "payload;payload" --hidden-import tihsg_extension --hidden-import PIL --hidden-import PIL.ImageTk app.py
```

The standalone executable will be generated at `dist\ZLT_X28_Gateway_Deployer.exe`.

---

## Disclaimer

This tool modifies router system configuration, WebUI assets, and background services. **Only use on ZLT X28 devices running Software Version `1.5.13` and Hardware Version `TZ7.823.432A` with SSH or Telnet enabled.** Use at your own risk.
