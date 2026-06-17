# 🛡️ Enterprise Active Directory Attack Detection Lab

![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Blue%20Team-blue)
![Splunk](https://img.shields.io/badge/SIEM-Splunk-orange)
![Windows](https://img.shields.io/badge/Active%20Directory-Windows%20Server-blue)
![Defender](https://img.shields.io/badge/EDR-Microsoft%20Defender-success)
![MITRE](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

---

# 📌 Project Overview

This project simulates a real-world enterprise environment where a Security Operations Center (SOC) monitors, detects, investigates, and responds to attacks targeting a Microsoft Active Directory infrastructure.

The lab was designed to replicate common attack techniques used by adversaries and demonstrate how security teams leverage SIEM, EDR, endpoint telemetry, and threat hunting methodologies to identify malicious activities.

---

# 🎯 Objectives

* Build a complete enterprise Active Directory environment
* Simulate real-world cyber attacks
* Collect telemetry using Sysmon and Windows Event Logs
* Monitor events using Splunk Enterprise
* Generate detections using Sigma Rules
* Investigate incidents through Microsoft Defender for Endpoint
* Map attack techniques to the MITRE ATT&CK Framework
* Create incident response workflows

---

# 🏗️ Lab Architecture

```text
                    ┌───────────────────────┐
                    │     Kali Linux        │
                    │     Attacker VM       │
                    └──────────┬────────────┘
                               │
                               ▼

┌──────────────────────────────────────────────────────────┐
│              Active Directory Network                    │
│                                                          │
│  Domain Controller      Windows Client 1               │
│      DC01                  WIN10-01                    │
│   192.168.10.10          192.168.10.30                 │
│                                                          │
│                     Windows Client 2                    │
│                        WIN10-02                         │
│                      192.168.10.31                      │
└──────────────────────────────────────────────────────────┘
                               │
                               ▼
                    ┌───────────────────────┐
                    │   Splunk Enterprise   │
                    │      SIEM Server      │
                    │    192.168.10.20      │
                    └───────────────────────┘
```

---

# 💻 Technologies Used

## Operating Systems

* Windows Server 2022
* Windows 10 Enterprise
* Kali Linux
* Ubuntu Server

## Security Tools

* Splunk Enterprise
* Splunk Universal Forwarder
* Sysmon
* Microsoft Defender for Endpoint
* Sigma Rules

## Offensive Tools

* Mimikatz
* BloodHound
* CrackMapExec
* Impacket
* PsExec
* Responder
* Rubeus

---

# 🔍 Attack Simulations

## 1. Password Spraying

### Objective

Identify weak password practices within the domain.

### Detection

* Event ID 4625
* Multiple failed login attempts
* Source IP correlation

### MITRE ATT&CK

T1110.003 – Password Spraying

---

## 2. Kerberoasting

### Objective

Extract Service Account Ticket Hashes for offline cracking.

### Detection

* Event ID 4769
* Excessive Kerberos Ticket Requests
* Service Account Enumeration

### MITRE ATT&CK

T1558.003 – Kerberoasting

---

## 3. Credential Dumping

### Objective

Dump credentials from LSASS memory.

### Detection

* Sysmon Event ID 10
* LSASS Access Attempts
* Defender EDR Alerts

### MITRE ATT&CK

T1003 – OS Credential Dumping

---

## 4. Pass-the-Hash

### Objective

Authenticate using NTLM hashes without plaintext credentials.

### Detection

* Logon Type 3
* NTLM Authentication Events
* Lateral Authentication Activity

### MITRE ATT&CK

T1550.002 – Pass the Hash

---

## 5. Lateral Movement

### Objective

Move between systems inside the domain.

### Detection

* Event ID 7045
* Remote Service Creation
* Administrative Share Access

### MITRE ATT&CK

T1021 – Remote Services

---

# 📊 Security Monitoring

## Sysmon Telemetry

Collected events include:

* Process Creation
* Process Injection
* Network Connections
* Registry Modifications
* File Creations
* Driver Loading
* Named Pipe Activity

---

## Splunk Dashboards

Developed dashboards for:

* Failed Login Attempts
* Privileged Account Activity
* PowerShell Monitoring
* Kerberoasting Detection
* Endpoint Security Alerts
* Threat Hunting Investigations

---

# 🚨 Detection Engineering

## Sigma Rules

Custom Sigma detections were developed for:

* Credential Dumping
* PowerShell Abuse
* Suspicious Services
* Persistence Techniques
* Lateral Movement
* Privilege Escalation

---

## Example Detection Logic

```yaml
title: Credential Dumping Detection

logsource:
  category: process_creation

detection:
  selection:
    Image|contains:
      - mimikatz
      - sekurlsa

condition: selection
```

---

# 🔎 Threat Hunting Activities

Conducted threat hunting using:

* Windows Event Logs
* Sysmon Logs
* Splunk Searches
* Defender Device Timeline

Investigations focused on:

* Credential Theft
* PowerShell Abuse
* Kerberoasting
* Lateral Movement
* Persistence Mechanisms

---

# 🎯 MITRE ATT&CK Mapping

| Technique            | ATT&CK ID |
| -------------------- | --------- |
| Password Spraying    | T1110.003 |
| Kerberoasting        | T1558.003 |
| Credential Dumping   | T1003     |
| Pass-the-Hash        | T1550.002 |
| PowerShell           | T1059.001 |
| PsExec               | T1021.002 |
| Privilege Escalation | T1068     |

---

# 🔄 Incident Response Workflow

```text
Alert Generated
       │
       ▼
Alert Validation
       │
       ▼
Threat Investigation
       │
       ▼
Containment
       │
       ▼
Eradication
       │
       ▼
Recovery
       │
       ▼
Lessons Learned
```

---

# 📈 Skills Demonstrated

### Blue Team Operations

* Security Monitoring
* Threat Hunting
* Detection Engineering
* Log Analysis
* Incident Response

### Windows Security

* Active Directory Administration
* Windows Event Logging
* Sysmon Configuration
* Group Policy Management

### SIEM Engineering

* Splunk Administration
* Dashboard Development
* Alert Engineering
* Correlation Searches

### Threat Detection

* Sigma Rules
* MITRE ATT&CK Mapping
* IOC Analysis
* Behavioral Analytics

---

# 📷 Screenshots

Add screenshots of:

* Splunk Dashboards
* Sysmon Events
* Defender Alerts
* BloodHound Graphs
* Attack Timeline
* Incident Investigations

---

# 🚀 Future Enhancements

* Wazuh Integration
* Security Onion Deployment
* Velociraptor DFIR Integration
* TheHive Case Management
* SOAR Automation
* Threat Intelligence Feeds
* YARA Rule Development

---

# 🏆 Key Outcomes

✔ Built a realistic enterprise Active Directory environment

✔ Simulated advanced attack techniques

✔ Implemented endpoint visibility using Sysmon

✔ Monitored events through Splunk SIEM

✔ Developed custom detections and Sigma rules

✔ Performed threat hunting investigations

✔ Mapped adversary behavior to MITRE ATT&CK

✔ Executed complete incident response workflows

---

# 👨‍💻 Author

**Aditya Gundure**

Cyber Security | SOC Operations | Threat Hunting | SIEM Engineering | Blue Team Operations

---

⭐ If you found this project useful, consider giving it a star and connecting with me on LinkedIn.
