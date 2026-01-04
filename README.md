# SOC and Active Protection – Open Source

[![GitHub stars](https://img.shields.io/github/stars/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/network)

---

## 📋 1. About the Project

This project presents the **architecture and implementation** of a complete **Security Operations Center (SOC)** built exclusively with **open-source tools**. The goal is to demonstrate how to create a robust cybersecurity solution with integrated capabilities for **detection, monitoring, analysis, and incident response**.

> **⚠️ Important**: This repository focuses on **presenting the implemented solution and its architecture**. It does not include specific configurations, installation scripts, or step-by-step tutorials. For implementation, refer to the official documentation of each tool.

### 1.1 🎯 Implemented Capabilities

| Capability                    | Description                                                 |
| ----------------------------- | ----------------------------------------------------------- |
| **Proactive Detection**       | Real-time identification of threats and suspicious behavior |
| **Centralized Visualization** | Unified dashboard for security events                       |
| **Structured Response**       | Organized incident response workflow                        |
| **Forensic Analysis**         | In-depth investigation of security cases                    |
| **Threat Intelligence**       | Sharing and correlation of threat indicators                |
| **Simple Architecture**       | Low-cost, easy-to-maintain solution                         |
| **Full Integration**          | Automatic communication between all tools                   |

---

## 🏗️ 2. Solution Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Workstations  │    │    Firewall      │    │   SOC Servers   │
│                 │    │   (OPNsense)     │    │                 │
│ • Wazuh Agent   │──▶│ • CrowdSec        │───▶│ • Wazuh SIEM   │
│ • Sysmon        │    │ • Suricata       │    │ • DFIR-IRIS     │
│                 │    │ • ClamAV         │    │ • MISP          │
│                 │    │ • Wazuh Agent    │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

### 2.1 📊 Component Overview

| Layer         | Component   | Main Function                        |
| ------------- | ----------- | ------------------------------------ |
| **Endpoints** | Wazuh Agent | Log collection and local monitoring  |
| **Endpoints** | Sysmon      | Advanced Windows activity logging    |
| **Perimeter** | OPNsense    | Firewall and network routing         |
| **Perimeter** | CrowdSec    | Behavioral detection and IP blocking |
| **Perimeter** | Suricata    | IDS/IPS with deep packet inspection  |
| **Perimeter** | ClamAV      | Malware and virus detection          |
| **SOC Core**  | Wazuh SIEM  | Event correlation and alerting       |
| **SOC Core**  | DFIR-IRIS   | Forensic investigation management    |
| **SOC Core**  | MISP        | Threat intelligence platform         |

---

## 🛡️ 3. Detailed Components

### 3.1 🔥 Perimeter Layer: Firewall / Router

#### OPNsense

**Open-source firewall and router based on FreeBSD**

| Tool            | Type                                 | Key Features                                                                                                                            |
| --------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| **CrowdSec**    | IDR (Intrusion Detection & Response) | • Real-time log analysis<br>• Brute-force and port-scan detection<br>• Threat intelligence sharing<br>• Automatic malicious IP blocking |
| **Suricata**    | IDS/IPS + DPI                        | • Signature-based detection<br>• Custom rule support<br>• Real-time protocol inspection<br>• Detailed event logging                     |
| **ClamAV**      | Antivirus                            | • Malware, trojan, and virus detection<br>• Real-time file scanning<br>• Automatic signature updates                                    |
| **Wazuh Agent** | Monitor                              | • File integrity monitoring<br>• System event collection<br>• Local threat detection                                                    |

### 3.2 💻 Endpoint Layer: Workstations

| Agent           | Platform                | Capabilities                                                                                                                                                              |
| --------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Wazuh Agent** | Windows / Linux / macOS | • OS log collection<br>• File integrity monitoring<br>• Rootkit and malware detection<br>• Registry monitoring (Windows)<br>• Compliance checks (PCI DSS, GDPR, HIPAA)    |
| **Sysmon**      | Windows                 | • Detailed system activity logging<br>• Process and network connection tracking<br>• File change monitoring<br>• Process creation events<br>• Evasion technique detection |

### 3.3 🖥️ Core Layer: SOC Servers

| Platform           | Category               | Features                                                                                                                                                                                                                                   |
| ------------------ | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Wazuh SIEM/XDR** | Analysis & Correlation | • Log collection and correlation from all sources<br>• Behavioral analysis and anomaly detection<br>• Real-time alerts with severity levels<br>• Interactive dashboards<br>• Automated compliance reporting<br>• REST API for integrations |
| **DFIR-IRIS**      | Incident Response      | • Incident case management<br>• Structured investigation workflows<br>• Analyst collaboration<br>• Detailed event timelines<br>• Evidence documentation<br>• Automated incident reports<br>• Integration with Wazuh via webhook            |
| **MISP**           | Threat Intelligence    | • Centralized threat intelligence<br>• IOC sharing<br>• Threat correlation across cases<br>• External intelligence feeds<br>• Automation API<br>• Organizational taxonomies and tags                                                       |

---

## 📦 4. Infrastructure Requirements

### 4.1 💾 Minimum Recommended Hardware

#### Firewall / Router

| Component   | Minimum Specification | Notes                          |
| ----------- | --------------------- | ------------------------------ |
| **CPU**     | 2 cores, 2.0 GHz      | Rule processing and inspection |
| **RAM**     | 4 GB                  | Stable operation               |
| **Storage** | 32 GB SSD             | Local log storage              |
| **Network** | 2x Gigabit NICs       | WAN and LAN                    |

#### SOC Server

| Component   | Minimum          | Recommended      | Notes                       |
| ----------- | ---------------- | ---------------- | --------------------------- |
| **CPU**     | 4 cores, 2.5 GHz | 8 cores, 3.0 GHz | Event correlation           |
| **RAM**     | 16 GB            | 32 GB            | Elasticsearch is RAM-hungry |
| **Storage** | 500 GB SSD       | 1 TB SSD         | Log retention               |
| **Network** | 1x Gigabit NIC   | 1x Gigabit NIC   | Endpoint communication      |

### 4.2 💿 Base Software

| Category             | Requirement             | Supported Versions                   |
| -------------------- | ----------------------- | ------------------------------------ |
| **Operating System** | Linux                   | Ubuntu 20.04+, CentOS 8+, Debian 11+ |
| **Operating System** | FreeBSD                 | For OPNsense                         |
| **Containerization** | Docker & Docker Compose | Latest stable                        |
| **Language**         | Python                  | 3.8+                                 |
| **Version Control**  | Git                     | Latest stable                        |

---

## 🔗 5. Official Tools and Resources

### 5.1 📚 Tool Links

| Category        | Tool      | Official Website                                             | Description                       |
| --------------- | --------- | ------------------------------------------------------------ | --------------------------------- |
| **Firewall**    | OPNsense  | [https://opnsense.org](https://opnsense.org)                 | FreeBSD-based firewall/router     |
| **Detection**   | CrowdSec  | [https://www.crowdsec.net](https://www.crowdsec.net)         | Collaborative detection system    |
| **IDS/IPS**     | Suricata  | [https://suricata.io](https://suricata.io)                   | Deep packet inspection engine     |
| **Antivirus**   | ClamAV    | [https://www.clamav.net](https://www.clamav.net)             | Open-source antivirus             |
| **SIEM/XDR**    | Wazuh     | [https://wazuh.com](https://wazuh.com)                       | SIEM and XDR platform             |
| **Logging**     | Sysmon    | Microsoft Docs                                               | Advanced Windows logging          |
| **DFIR**        | DFIR-IRIS | [https://dfir-iris.org](https://dfir-iris.org)               | Forensic investigation management |
| **TI Platform** | MISP      | [https://www.misp-project.org](https://www.misp-project.org) | Threat intelligence sharing       |

---

## 📊 6. Operational Flows

### 6.1 🔄 Detection and Response Flow

```
① Suspicious Event
    ↓
② Detection (Suricata / CrowdSec)
    ↓
③ Alert (Wazuh)
    ↓
④ Case Creation (DFIR-IRIS)
    ↓
⑤ IOC Export (MISP)
```

### 6.2 🔗 Integration Architecture

```
Wazuh SIEM ──→ DFIR-IRIS (Automatic case creation)
     ↓
DFIR-IRIS ──→ MISP (IOC export after investigation)
     ↓
MISP ──→ Threat Intelligence (Indicator sharing)
```

### 6.3 🛠️ Communication Methods

| Method         | Usage             | Purpose               |
| -------------- | ----------------- | --------------------- |
| **Webhooks**   | Wazuh → DFIR-IRIS | Workflow automation   |
| **REST APIs**  | All tools         | Data integration      |
| **TI Feeds**   | MISP → Wazuh      | Alert enrichment      |
| **Dashboards** | Wazuh             | Unified visualization |

---

## 🎯 7. Demonstrated Capabilities

### 7.1 📈 Core Features

| Feature                       | Description                            |
| ----------------------------- | -------------------------------------- |
| **Security Events Overview**  | Centralized, holistic dashboard        |
| **Compliance Monitoring**     | Regulatory compliance tracking         |
| **Threat Hunting**            | Proactive investigative threat hunting |
| **Vulnerability Assessment**  | Continuous vulnerability evaluation    |
| **File Integrity Monitoring** | Monitoring of critical files           |

### 7.2 🔍 Implemented Response Workflows

| #     | Workflow                     | Scenario                              | Tools                             |
| ----- | ---------------------------- | ------------------------------------- | --------------------------------- |
| **①** | Malware Investigation        | Malicious file detection and analysis | Wazuh + ClamAV + DFIR-IRIS + MISP |
| **②** | Phishing Response            | Phishing campaign investigation       | Wazuh + DFIR-IRIS + MISP          |
| **③** | Data Leak Response           | Data exfiltration investigation       | Wazuh + Suricata + DFIR-IRIS      |
| **④** | Insider Threat Investigation | Suspicious user behavior analysis     | Wazuh + Sysmon + DFIR-IRIS        |

---

## 📈 8. Results and Metrics

### 8.1 🎯 Efficiency Metrics

| Metric                     | Acronym | Description                             |
| -------------------------- | ------- | --------------------------------------- |
| **Mean Time to Detection** | MTTD    | Time between attack start and detection |
| **Mean Time to Response**  | MTTR    | Time between detection and response     |
| **False Positive Rate**    | FPR     | Alert accuracy                          |
| **Endpoint Coverage**      | –       | Percentage of monitored devices         |
| **TI Feed Status**         | –       | Quality and freshness of intelligence   |

### 8.2 🎭 Detected Threat Types

| #     | Category            | Examples                     |
| ----- | ------------------- | ---------------------------- |
| **①** | Malware             | Trojans, ransomware, spyware |
| **②** | Brute Force Attacks | Unauthorized login attempts  |
| **③** | Data Exfiltration   | Suspicious file transfers    |
| **④** | File Integrity      | Unauthorized file changes    |
| **⑤** | Command & Control   | Known C2 communications      |
| **⑥** | Insider Threats     | Suspicious internal activity |

### 8.3 ✅ Implementation Benefits

| #     | Benefit                  | Impact                                   |
| ----- | ------------------------ | ---------------------------------------- |
| **①** | Full Visibility          | 100% monitoring of network and endpoints |
| **②** | Coordinated Response     | Structured incident workflows            |
| **③** | Structured Documentation | Detailed case records                    |
| **④** | Intelligence Sharing     | Global community collaboration           |
| **⑤** | Zero Licensing Cost      | Fully open-source solution               |

---

## 🖼️ 9. Implemented Topology

<h3>Post-Implementation Topology Example</h3>

<img src="assets/TOPOLOGIA DEPOIS.png" alt="SOC Topology" width="1000"/>

---

## 🔗 10. Useful Links

| Title                                   | Link     | Description                         |
| --------------------------------------- | -------- | ----------------------------------- |
| **Wazuh – Cybersecurity**               | Playlist | Complete Wazuh system configuration |
| **MISP Installation and Configuration** | Video    | Basic MISP configuration            |

---

## 📝 11. Final Notes

**If you use this project in academic work or enterprise implementations, please keep proper credits.**

**Developed with ❤️ for the cybersecurity community**
