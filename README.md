# Zenith Local - Offline Windows Security Dashboard 
<img width="400" height="400" alt="svgviewer-png-output (2)" src="https://github.com/user-attachments/assets/4060418a-c037-4e84-bfcc-d6cb3b220bcd" />

> A self-hosted, offline Windows security dashboard — no cloud, no telemetry, everything runs on your machine.



![Platform](https://img.shields.io/badge/platform-Windows-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)

---

<img width="1424" height="809" alt="image" src="https://github.com/user-attachments/assets/671d4543-fc64-43c7-a4a0-6d9057969818" />


## What it does

Zenith Local is a local web dashboard (runs on `localhost:5000`) that gives you a full security and health audit of your Windows machine — in one place, without sending any data anywhere.

| Section | What you get |
|---|---|
| **Overview** | Prioritized findings (Critical / High / Medium / Low / Info) across all scans |
| **Network** | Open ports, active connections with hostname resolution, firewall rules, per-process I/O |
| **Malware / AV** | ClamAV scan profiles (Quick / Standard / Full / Custom path), Windows Defender status, process hash check via MalwareBazaar |
| **Registry** | Autorun entries (Run / RunOnce / Winlogon / AppInit_DLLs) — flags suspicious paths and living-off-the-land binaries |
| **File Scanner** | Scan any folder for double-extension files, scripts in high-risk locations, executables in Temp/Downloads |
| **Event Logs** | System / Application / Security logs with severity coloring, full detail panel per entry |
| **Monitoring** | Live CPU / RAM / Disk / Network charts, swap, interface status, process table |
| **Hardware** | Disk S.M.A.R.T. health, RAM modules, GPU info, disk stress test |
| **Drivers** | All installed drivers with date, age, Hardware ID (copy button), vendor download link |
| **System Info** | Full profile: CPU, GPU, RAM slots, motherboard, BIOS, OS, battery health % |
| **File Watch** | Real-time folder monitoring — alerts on new/modified/deleted files |
| **Reports** | Full scan report — print as PDF or download as `.txt` |

---

## Quick start

**Requirements:** Python 3.10+ from [python.org](https://www.python.org/downloads/) — check *"Add python.exe to PATH"* during install.

```
1. Download and unzip the release
2. Double-click  start.bat
3. First run: venv is created and dependencies installed automatically
4. Click Run — the dashboard opens at http://localhost:5000
```

> For complete results right-click `start.bat` → **Run as administrator**

---

## ClamAV (optional but recommended)

Zenith Local integrates with [ClamAV](https://www.clamav.net/download) as a second engine alongside Windows Defender.

1. Download the Windows `.msi` from [clamav.net/download](https://www.clamav.net/download)
2. Install to `C:\Program Files\ClamAV`
3. Dashboard → **Malware / AV** → **Update Database**
4. Choose a scan profile and start

---

## Build a standalone EXE

```bat
build_exe.bat
```

Bundles Python + all dependencies into `dist\ZenithLocal.exe` using PyInstaller.
Copy `templates\`, `static\`, and `scanner\` folders alongside the EXE.

---

## Project structure

```
zenith-local/
├── app.py                  # Flask server — all API routes
├── launcher.py             # tkinter GUI launcher
├── desktop.py              # PyWebView native window (optional)
├── start.bat               # One-click web launcher
├── start_desktop.bat       # One-click native window launcher
├── build_exe.bat           # PyInstaller EXE builder
├── requirements.txt
├── scanner/
│   ├── clamav_scan.py      # ClamAV engine integration
│   ├── clamav_profiles.py  # Quick / Standard / Full profiles
│   ├── defender_scan.py    # Windows Defender
│   ├── driver_scan.py      # Driver list + Hardware IDs
│   ├── event_logs.py       # Windows Event Log
│   ├── file_scanner.py     # Suspicious file detection
│   ├── file_watch.py       # Real-time folder watch (watchdog)
│   ├── hardware_scan.py    # S.M.A.R.T., RAM, GPU, stress test
│   ├── hash_scan.py        # Process hashes + MalwareBazaar
│   ├── monitor.py          # Live system metrics
│   ├── monitor_alerts.py   # Threshold alerts
│   ├── network_scan.py     # Ports + firewall
│   ├── network_advanced.py # Connections + per-process I/O
│   ├── registry_scan.py    # Autorun scanner
│   ├── risk_engine.py      # Finding schema + aggregation
│   └── system_scan.py      # UAC, RDP, SMBv1, updates
├── templates/
│   └── index.html
└── static/
    ├── style.css
    ├── script.js
    └── logo.svg
```

---

## Tech stack

| Layer | Technology |
|---|---|
| Backend | Python 3.10+, Flask |
| System access | psutil, PowerShell / WMI |
| File watch | watchdog |
| Malware scanning | ClamAV, Windows Defender |
| Frontend | Vanilla JS, Canvas API, IBM Plex fonts |
| Native window | PyWebView / Edge WebView2 (optional) |

---

## Security & privacy

- **100% local.** Nothing is sent anywhere.
- Binds to `127.0.0.1` only — not reachable from the network.
- MalwareBazaar lookup is opt-in — requires your own API key.

---

## License

MIT — free to use, modify, and distribute.

---

## Contributing

Pull requests and issues are welcome.
