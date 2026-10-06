# Zenith Local

> Offline Windows security dashboard — no cloud, no telemetry, everything runs on your machine.

![Platform](https://img.shields.io/badge/platform-Windows-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![Version](https://img.shields.io/badge/version-3.1-brightgreen)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Overview

Zenith Local is a local web dashboard (`localhost:5000`) that gives you a full security and health audit of your Windows machine in one place — without sending any data anywhere.

---

## Features

### 🔍 Overview
- Prioritized findings across all scans: **Critical / High / Medium / Low / Info**
- Click any severity counter to filter instantly
- Category filter (Network / System / Malware / Logs / Hardware / Drivers)
- Click any finding to open a full detail modal with CVE links

### 🌐 Network
- **Open Ports** — listening ports with risk tagging (RDP, SMB, Telnet, VNC…)
- **Active Connections** — outbound connections with hostname resolution, external IP detection, suspicious port alerts
- **Firewall Rules** — full inbound/outbound rule list with enable/disable per rule
- **Per-Process I/O** — which process is using the most network bandwidth

### 🛡️ Malware / AV
- **Scan Profiles** — Quick (2-5 min), Standard (10-20 min), Full (30-90 min), Custom path
- **Windows Defender** — status check + trigger Quick Scan (works without ClamAV)
- **ClamAV** *(optional)* — second engine, database age check, update button
- **Process Integrity** — SHA-256 hash of every running process, optional MalwareBazaar API check

> ClamAV is **optional**. Windows Defender integration and all other features work without it.

### 🔑 Registry
- Scans Run / RunOnce / Winlogon / AppInit_DLLs autorun keys
- Flags suspicious paths: Temp folder, encoded PowerShell, double-extension, LOLBIN
- Shows whether the referenced file actually exists on disk

### 📂 File Scanner
- Scan any folder for: double-extension files, scripts in high-risk locations, executables in Temp/Downloads, zero-byte executables
- SHA-256 hash per flagged file
- Optional ClamAV scan over the same folder

### 📋 Event Logs
- System / Application / Security logs with severity color coding
- Click **Detail** on any entry for full Event ID, source, keywords, and complete message
- Filter by level, log channel, and free-text search

### 📊 Monitoring
- Live **CPU / RAM** line charts (50-point rolling history)
- **Disk** usage bars with read/write MB/s per drive
- **Swap / Page file** usage
- **Network** cumulative sent/received + interface UP/DOWN status
- **Process table** — CPU%, RAM, exe path, user, status; sort by CPU or RAM; live filter; auto-refresh every 3 s
- **Threshold alerts** — banner appears when CPU > 90% or RAM > 95% sustained

### ⚙️ Services
- Full Windows service list with status and start type
- **Start / Stop / Restart** any service from the dashboard
- Set **Automatic / Manual / Disabled** startup type
- Risky services highlighted (RemoteRegistry, WinRM, Telnet, SNMP…)

### 🔥 Firewall Manager
- **Profile cards** — enable/disable Domain, Private, Public profiles
- **Rules table** — full inbound/outbound rule list, enable/disable per rule, delete
- Filter by direction, action (Allow/Block), and free-text
- **Add Rule** form — name, direction, action, protocol, port, remote address

### 💡 Security Recommendations
- Auto-generated after each scan based on findings
- Covers: firewall off, SMBv1, RDP exposed, UAC disabled, outdated AV signatures, Guest account, old drivers, suspicious autoruns, overdue Windows Update
- Each recommendation includes a ready-to-run fix command or step-by-step instructions

### 💾 Hardware
- **Disk** — S.M.A.R.T. health status, failure prediction, operational status
- **RAM** — per-slot capacity, speed, type, manufacturer
- **GPU** — VRAM, driver version, resolution, status
- **Stress Test** — sequential write + read throughput test (64 MB – 1 GB), result in MB/s

### 🔧 Drivers
- Full list of installed drivers with version, date, age in days
- **Hardware ID copy button** — copies `PCI\VEN_XXXX&DEV_XXXX…` for browser search
- Vendor **Download link** for NVIDIA, AMD, Intel, Realtek, Qualcomm and others
- Filter by device class, name, manufacturer; toggle **Outdated only**

### 🖥️ System Info
- Computer model, manufacturer, system type
- OS version, build, architecture, install date, last boot
- CPU name, cores, threads, cache sizes, socket
- GPU name, VRAM, driver, resolution
- RAM slots — capacity, speed, type per module
- Motherboard and BIOS details
- **Battery health %** — design vs full-charge capacity, chemistry, status, charge level

### 👁️ File Watch
- Real-time folder monitoring (watchdog)
- Events: created, modified, deleted, renamed
- Alert banner on ransomware-style burst (mass file changes)
- Add / remove watched paths from the UI
- Filter events by type and path

### 📄 Reports
- Generate a full scan report grouped by severity
- **Print / Save as PDF** via browser print dialog
- **Download as .txt**

### ⏱️ Auto-Scan
- Scheduled automatic full scan: **every 15 / 30 / 60 minutes**
- Select interval from the top bar — applies immediately
- Label shows next scheduled run time
- Set to **Off** to disable

---

## Quick Start

**Requirements:** Python 3.10+ from [python.org](https://www.python.org/downloads/)
During install check **"Add python.exe to PATH"**.

```
1. Unzip the release
2. Double-click  start.bat
3. First run: venv is created and dependencies installed automatically
4. Click Run — dashboard opens at http://localhost:5000
```

> For complete results (firewall, Security log, update history) right-click `start.bat` → **Run as administrator**

---

## ClamAV (Optional)

ClamAV adds a second malware engine alongside Windows Defender.

1. Download Windows `.msi` from [clamav.net/download](https://www.clamav.net/download)
2. Install to `C:\Program Files\ClamAV`
3. Dashboard → **Malware / AV** → **Update Database**
4. Choose a scan profile and start

Without ClamAV: Defender integration, File Scanner, Registry scanner, Process Integrity, and all other features continue to work normally.

---

## MalwareBazaar (Optional)

Process Integrity can check SHA-256 hashes against [MalwareBazaar](https://bazaar.abuse.ch/).

1. Get a free API key from [auth.abuse.ch](https://auth.abuse.ch/)
2. Dashboard → **Malware / AV** → **Process Integrity** → paste key → **Save**

---

## Build a Standalone EXE

```bat
build_exe.bat
```

Bundles Python + all dependencies into `dist\ZenithLocal.exe` using PyInstaller.
Copy `templates\`, `static\`, and `scanner\` alongside the EXE before distributing.

---

## Project Structure

```
zenith-local/
├── app.py                      # Flask server — all API routes
├── launcher.py                 # tkinter GUI launcher
├── desktop.py                  # PyWebView native window (optional)
├── start.bat                   # One-click launcher
├── build_exe.bat               # PyInstaller EXE builder
├── requirements.txt
├── scanner/
│   ├── auto_scanner.py         # Auto-scan scheduler (15/30/60 min)
│   ├── clamav_profiles.py      # Quick / Standard / Full scan profiles
│   ├── clamav_scan.py          # ClamAV engine integration
│   ├── config_store.py         # Persistent config (JSON)
│   ├── defender_scan.py        # Windows Defender status + trigger
│   ├── driver_scan.py          # Driver list + Hardware IDs
│   ├── event_logs.py           # Windows Event Log reader
│   ├── file_scanner.py         # Suspicious file detection
│   ├── file_watch.py           # Real-time folder watch (watchdog)
│   ├── firewall_manager.py     # Firewall rules CRUD + profile control
│   ├── hardware_scan.py        # S.M.A.R.T., RAM, GPU, stress test
│   ├── hash_scan.py            # Process hashes + MalwareBazaar
│   ├── monitor.py              # Live system metrics
│   ├── monitor_alerts.py       # Threshold-based alerts
│   ├── network_advanced.py     # Connections + per-process I/O
│   ├── network_scan.py         # Ports + firewall profiles
│   ├── ps.py                   # PowerShell helpers
│   ├── recommendations.py      # Security recommendations engine
│   ├── registry_scan.py        # Autorun scanner
│   ├── risk_engine.py          # Finding schema + aggregation
│   ├── services_manager.py     # Windows service control
│   └── system_scan.py          # UAC, RDP, SMBv1, updates, accounts
├── templates/
│   └── index.html              # Single-page dashboard
└── static/
    ├── style.css
    ├── script.js
    └── logo.svg
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.10+, Flask |
| System access | psutil, PowerShell / WMI |
| File watch | watchdog |
| Malware scanning | Windows Defender (built-in), ClamAV (optional) |
| Frontend | Vanilla JS, Canvas API, IBM Plex fonts |
| Native window | PyWebView / Edge WebView2 (optional) |

---

## Security & Privacy

- **100% local.** Nothing is sent anywhere.
- Binds to `127.0.0.1` only — not reachable from the network.
- MalwareBazaar lookup is opt-in and requires your own API key.
- ClamAV is optional — the app works fully without it.

---

## License

MIT — free to use, modify, and distribute.

---

## Contributing

Pull requests and issues are welcome on [GitHub](https://github.com/ezerux/Zenith).
