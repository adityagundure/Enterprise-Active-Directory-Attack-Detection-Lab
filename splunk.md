# Splunk Enterprise Deployment & Windows Log Forwarding 

## Author

**Aditya Gundure**

<img width="1918" height="1020" alt="Screenshot 2026-06-19 081033" src="https://github.com/user-attachments/assets/015411b0-d69f-49b4-8f78-cd68829139a8" />
<img width="1918" height="1022" alt="Screenshot 2026-06-19 080113" src="https://github.com/user-attachments/assets/f7a91f25-54f8-4c9b-ae59-96a9e72844f7" />


---

# Project Overview

This project demonstrates the deployment and configuration of Splunk Enterprise in a Windows Active Directory lab environment. The objective is to centralize Windows security logs, monitor user activities, detect malicious behavior, and build a Security Operations Center (SOC) monitoring environment.

The lab simulates a real enterprise network consisting of:

* Active Directory Domain Controller
* Windows 10 Endpoints
* Splunk Enterprise SIEM
* Splunk Universal Forwarders
* Sysmon Telemetry
* Attack Simulation Environment

---

# Lab Architecture

| Hostname      | Role                     | IP Address    |
| ------------- | ------------------------ | ------------- |
| DC01          | Domain Controller + DNS  | 192.168.10.10 |
| SPLUNK01      | Splunk Enterprise Server | 192.168.10.20 |
| WIN10-CLIENT1 | Domain User Workstation  | 192.168.10.30 |
| WIN10-CLIENT2 | Domain User Workstation  | 192.168.10.31 |
| KALI-LINUX    | Attacker Machine         | 192.168.10.50 |

Domain Name:

corp.local

---

# Objectives

* Deploy Splunk Enterprise
* Configure Splunk Receiving Port
* Install Universal Forwarders
* Collect Windows Event Logs
* Ingest Security Logs into Splunk
* Monitor Authentication Events
* Detect Active Directory Attacks
* Create SOC Dashboards
* Build Detection Rules

---

# Splunk Server Installation

## Environment

Operating System:

Ubuntu Server 22.04

Hostname:

SPLUNK01

IP Address:

192.168.10.20

---

## Install Splunk Enterprise

Download Splunk Enterprise package:

splunk-<version>-linux-amd64.deb

Install package:

```bash
sudo dpkg -i splunk-<version>-linux-amd64.deb
```

Start Splunk:

```bash
sudo /opt/splunk/bin/splunk start --accept-license
```

Enable Startup:

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

Check Status:

```bash
sudo /opt/splunk/bin/splunk status
```

---

# Access Splunk Web

Open Browser:

http://192.168.10.20:8000

Login using:

Username: admin

Password: <Configured During Installation>

---

# Configure Receiving Port

Login to Splunk.

Navigate:

Settings → Forwarding and Receiving

Select:

Configure Receiving

Add New Port:

9997

Save Configuration.

Verify:

```bash
sudo /opt/splunk/bin/splunk display listen
```

Expected:

```text
Splunk TCP listening on port 9997
```

---

# Windows Universal Forwarder Installation

## Target Systems

* DC01
* WIN10-CLIENT1
* WIN10-CLIENT2

Download Universal Forwarder MSI.

Run Installer as Administrator.

Choose:

On-Premises Splunk Enterprise

Receiving Indexer:

192.168.10.20:9997

Complete Installation.

---

# Verify Forwarder Service

Open Services:

services.msc

Verify:

SplunkForwarder

Status:

Running

Startup Type:

Automatic

---

# Configure Windows Event Log Collection

Navigate:

C:\Program Files\SplunkUniversalForwarder\etc\system\local

Create:

inputs.conf

Contents:

```ini
[WinEventLog://Security]
disabled = 0
index = wineventlog

[WinEventLog://System]
disabled = 0
index = wineventlog

[WinEventLog://Application]
disabled = 0
index = wineventlog

[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = 0
index = powershell
```

Restart Service:

```powershell
Restart-Service SplunkForwarder
```

---

# Windows Firewall Configuration

Allow outbound traffic to Splunk:

```powershell
New-NetFirewallRule `
-DisplayName "Splunk UF Outbound" `
-Direction Outbound `
-Protocol TCP `
-RemotePort 9997 `
-Action Allow
```

---

# Verify Connectivity

Run:

```powershell
Test-NetConnection 192.168.10.20 -Port 9997
```

Expected:

```text
TcpTestSucceeded : True
```

---

# Verify Data Ingestion

Open Splunk Search & Reporting.

Run:

```spl
index=*
```

Or:

```spl
index=wineventlog
```

Expected Data Sources:

* Security Logs
* System Logs
* Application Logs
* PowerShell Logs
* Authentication Events

---

# Useful Splunk Searches

## Successful Logins

```spl
index=wineventlog EventCode=4624
```

## Failed Logins

```spl
index=wineventlog EventCode=4625
```

## New User Creation

```spl
index=wineventlog EventCode=4720
```

## Group Membership Changes

```spl
index=wineventlog EventCode=4728
```

## PowerShell Activity

```spl
index=powershell
```

---

# Security Monitoring Use Cases

### Brute Force Detection

```spl
index=wineventlog EventCode=4625
| stats count by Account_Name
```

### Privileged Logins

```spl
index=wineventlog EventCode=4672
```

### Account Creation Monitoring

```spl
index=wineventlog EventCode=4720
```

### Password Change Monitoring

```spl
index=wineventlog EventCode=4723
```

---

# Sysmon Integration

Recommended for advanced visibility.

Benefits:

* Process Creation Monitoring
* Network Connections
* Registry Changes
* DLL Loads
* PowerShell Visibility

Key Event IDs:

| Event ID | Description           |
| -------- | --------------------- |
| 1        | Process Creation      |
| 3        | Network Connection    |
| 7        | Image Loaded          |
| 11       | File Creation         |
| 13       | Registry Modification |

---

# Splunk Universal Forwarder Rules

# Author: Aditya Gundure

## inputs.conf

Location:

C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf

```ini
# Windows Security Events
[WinEventLog://Security]
disabled = 0
start_from = oldest
index = wineventlog
current_only = 0

# Windows System Events
[WinEventLog://System]
disabled = 0
start_from = oldest
index = wineventlog

# Windows Application Events
[WinEventLog://Application]
disabled = 0
start_from = oldest
index = wineventlog

# PowerShell Operational Logs
[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = 0
index = powershell

# Sysmon Logs
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = sysmon

# DNS Server Logs (Domain Controller)
[WinEventLog://DNS Server]
disabled = 0
index = dns

# Directory Service Logs
[WinEventLog://Directory Service]
disabled = 0
index = active_directory

# File Share Auditing
[WinEventLog://Microsoft-Windows-SMBServer/Audit]
disabled = 0
index = smb

# Windows Defender Logs
[WinEventLog://Microsoft-Windows-Windows Defender/Operational]
disabled = 0
index = defender
```

## outputs.conf

Location:

C:\Program Files\SplunkUniversalForwarder\etc\system\local\outputs.conf

```ini
[tcpout]
defaultGroup = splunk_indexers

[tcpout:splunk_indexers]
server = 192.168.10.20:9997

[tcpout-server://192.168.10.20:9997]
```

## deploymentclient.conf

```ini
[deployment-client]

[target-broker:deploymentServer]
targetUri = 192.168.10.20:8089
```

## Restart Universal Forwarder

```powershell
Restart-Service SplunkForwarder
```

## Verify Forwarder

```powershell
"C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server
```

Expected:

Active forwards:
192.168.10.20:9997

```
```


# Skills Demonstrated

* Active Directory Administration
* Windows Security Monitoring
* Splunk Enterprise Deployment
* Log Management
* SIEM Engineering
* Security Event Analysis
* Threat Detection
* SOC Operations
* Detection Engineering

---

# Future Enhancements

* Sysmon Deployment
* Splunk Dashboards
* Sigma Rule Integration
* MITRE ATT&CK Mapping
* Detection Engineering
* Kerberoasting Detection
* Pass-the-Hash Detection
* Privilege Escalation Detection

---

# Author

Aditya Gundure

Cyber Security | SOC Analyst | SIEM Engineering | Active Directory Security | Splunk | Threat Detection
