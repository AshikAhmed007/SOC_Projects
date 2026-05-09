# 🛡️ Detecting Malicious Files Using VirusTotal Integration with Wazuh

---

## 📌 Overview

This document demonstrates how to integrate :contentReference[oaicite:0]{index=0} with :contentReference[oaicite:1]{index=1} to detect malicious files using hash-based reputation checking.

The system uses File Integrity Monitoring (FIM) to detect file changes and automatically submits file hashes to VirusTotal for threat intelligence analysis. If a file is identified as malicious, Wazuh generates real-time security alerts.

---

## 🎯 Objectives

- Integrate Wazuh with VirusTotal
- Enable File Integrity Monitoring (FIM)
- Detect file creation and modification in real time
- Perform hash-based malware reputation analysis
- Generate automated alerts for malicious files
- Validate detection using the EICAR test file

---

## 🧪 Lab Environment

| Component | Description |
|----------|-------------|
| SIEM Platform | :contentReference[oaicite:2]{index=2} |
| Endpoint OS | :contentReference[oaicite:3]{index=3} |
| Threat Intelligence | :contentReference[oaicite:4]{index=4} |
| Monitoring Method | File Integrity Monitoring (FIM) |
| Test File | :contentReference[oaicite:5]{index=5} |

---

## ⚙️ Prerequisites

Ensure the following are completed before starting:

- Wazuh Manager installed and running
- Wazuh Agent installed and connected
- VirusTotal API key generated
- File Integrity Monitoring enabled
- Internet access available on endpoint

---

## 🧩 Step 1 — Enable File Integrity Monitoring (FIM)

### 🔗 Connect to Ubuntu Agent

```bash
ssh wazuh-user@192.168.209
