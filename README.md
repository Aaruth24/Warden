# 🛡️ Warden: Host-Based Forensic Triage Utility

**Warden v1.2 | Built by [Aaruth](https://github.com/Aaruth24)**  
*(Enhanced version inspired by Warden v1.1 built by [Dharan](https://github.com/Dharan10))*

---

## 🚀 Project Overview

**Warden** is a high-performance, single-binary **Command Line Interface (CLI)** tool built in **Go (Golang)** for **Digital Forensics and Incident Response (DFIR)**. It helps reduce **Mean Time To Respond (MTTR)** during incidents by rapidly collecting and analyzing volatile process data that attackers often exploit or erase.

Warden v1.2 is a Linux-first enhancement that improves safety, forensic clarity, and usability while preserving the original DFIR concept introduced in Warden v1.1.

By abstracting complex OS APIs (`/proc` on Linux), Warden delivers **immediate, actionable forensic intelligence** in a clean, scriptable format.

---

## 🛠️ Tech Stack

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.22+-blue?logo=go" alt="Golang" />
  <img src="https://img.shields.io/badge/gopsutil-library-green?logo=go" alt="gopsutil" />
  <img src="https://img.shields.io/badge/SHA256-Crypto-orange?logo=security" alt="SHA256" />
  <img src="https://img.shields.io/badge/Linux-/proc-red?logo=linux" alt="Linux procfs" />
</p>

---

## ⚙️ Installation & Build
```bash

git clone https://github.com/Aaruth24/Warden.git
cd Warden
go mod tidy
go build -o warden

```

this generates a single binary named warden

----------------------------------

## 💡 Core Modes & Usage

Warden operates in **five modes**, each targeting a specific phase of incident investigation:

| Command Structure | Core Functionality | Security Triage Value |
|------------------|------------------|----------------------|
| `./warden --list` | **Triage Overview** | Lists all running PIDs, Parent PIDs (PPID), user context, and command line for fast situational awareness. |
| `./warden <PID>` | **Standard Inspect** | Performs deep inspection of a process, including lineage, timestamps, hashing, networking, and file usage. |
| `./warden --ioc <PID>` | **Binary IOC Hunting** | Extracts suspicious ASCII strings such as URLs, domains, credentials, tokens, and API keywords from the executable. |
| `./warden --clean <PID>` | **Clean-Up Audit** | Detects anti-forensic behavior such as deleted executables and suspicious parent commands. |
| `./warden --kill --dry <PID>` | **Active Containment** | Safely previews recursive termination of a malicious process and all child processes. |

---

## 🔎 Features Behind Each Mode

### 🧾 `--list` (Triage Overview)

- Rapid enumeration of all active processes  
- Identifies suspicious parent-child relationships  
- Highlights abnormal user contexts (root, service accounts)  
- Useful during the **first 60 seconds of incident response**

---

### 🔍 Inspect Mode (`./warden <PID>`)

Extracts critical forensic metadata:

- Full process lineage back to PID 1  
- Executable path and command-line arguments  
- Process start time (useful for timeline reconstruction)  
- SHA256 hash of on-disk binary  
- Open file descriptor count (detects scanning or scraping behavior)  
- Active network connections  

Used for **live malware triage and behavior analysis**.

---

### 🧪 IOC Hunting (`--ioc`)

Scans executable binaries for:

- URLs and IP references  
- API tokens and secrets  
- Password-related keywords  

Helps identify:

- Command-and-Control (C2) endpoints  
- Hardcoded credentials  
- Embedded malicious indicators  

Designed for **quick IOC extraction without full reverse engineering**.

---

### 🧹 Clean-Up Audit (`--clean`)

Detects common attacker anti-forensic techniques:

- Executable deleted while process is still running (fileless malware)  
- Parent processes invoking destructive commands such as:
  - `rm -rf`
  - `shred`
  - `vssadmin delete shadows`

Useful for identifying **post-exploitation clean-up attempts**.

---

### ☠️ Active Containment (`--kill`)

- Recursively terminates a process and its children  
- Includes a mandatory-safe `--dry` preview  
- Prevents accidental system destabilization  
- Designed for **last-resort containment**

⚠️ **Highly dangerous if misused**

---

## 🆘 Help Menu

```bash
./warden
