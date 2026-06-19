# Client Systems Configuration
<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/4656756f-90e2-4719-a357-44cbfe1267aa" />
<img width="225" height="225" alt="image" src="https://github.com/user-attachments/assets/cb16453d-2569-43cd-a39e-cf2236c45fa5" />


## Author

Aditya Gundure

---

# Overview

This document describes the client systems used in the Enterprise Active Directory Attack Detection Lab.

The environment includes both Windows and Linux endpoints connected to the Active Directory infrastructure and monitored by Splunk Enterprise.

---

# Client Architecture

| Hostname      | Operating System      | Role                    | IP Address    |
| ------------- | --------------------- | ----------------------- | ------------- |
| WIN10-CLIENT1 | Windows 10 Enterprise | Domain User Workstation | 192.168.10.30 |
| WIN10-CLIENT2 | Windows 10 Enterprise | Domain User Workstation | 192.168.10.31 |
| LINUX-CLIENT1 | Ubuntu Desktop 22.04  | Linux Endpoint          | 192.168.10.40 |
| KALI-LINUX    | Kali Linux            | Red Team Attacker       | 192.168.10.50 |

Domain:

corp.local

Network:

192.168.10.0/24

---

# Windows Client Configuration

## WIN10-CLIENT1

Operating System:

Windows 10 Enterprise

IP Configuration:

IP Address: 192.168.10.30

Subnet Mask: 255.255.255.0

DNS Server: 192.168.10.10

---

## Join Domain

Open:

System Properties → Change Settings

Select:

Domain

Enter:

corp.local

Authenticate using:

CORP\Administrator

Restart System.

---

## Verify Domain Membership

```powershell
whoami
```

Expected:

```text
corp\adi
```

---

# WIN10-CLIENT2

Operating System:

Windows 10 Enterprise

IP Configuration:

IP Address: 192.168.10.31

Subnet Mask: 255.255.255.0

DNS Server: 192.168.10.10

Domain:

corp.local

---

# Windows Logging

Installed Components:

* Splunk Universal Forwarder
* Sysmon
* PowerShell Logging
* Windows Event Logging

Collected Logs:

* Security Logs
* System Logs
* Application Logs
* PowerShell Logs
* Sysmon Events

---

# Linux Client Configuration

## LINUX-CLIENT1

Operating System:

Ubuntu Desktop 22.04

Hostname:

LINUX-CLIENT1

IP Configuration:

IP Address: 192.168.10.40

Subnet Mask: 255.255.255.0

DNS Server: 192.168.10.10

---

# Configure Static IP

Netplan Configuration

```yaml
network:
  version: 2
  renderer: networkd

  ethernets:
    ens33:
      dhcp4: false
      addresses:
        - 192.168.10.40/24
      nameservers:
        addresses:
          - 192.168.10.10
```

Apply:

```bash
sudo netplan apply
```

---

# Install Splunk Universal Forwarder

Install Package:

```bash
sudo dpkg -i splunkforwarder*.deb
```

Start Service:

```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

Configure Forwarding:

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.10.20:9997
```

Enable Startup:

```bash
sudo /opt/splunkforwarder/bin/splunk enable boot-start
```

---

# Linux Log Monitoring

Monitored Files:

```text
/var/log/auth.log
/var/log/syslog
/var/log/kern.log
/var/log/apache2/access.log
/var/log/apache2/error.log
```

Example inputs.conf:

```ini
[monitor:///var/log/auth.log]
disabled = 0
index = linux

[monitor:///var/log/syslog]
disabled = 0
index = linux

[monitor:///var/log/kern.log]
disabled = 0
index = linux
```

---

# Kali Linux Attacker

Hostname:

KALI-LINUX

IP Address:

192.168.10.50

Purpose:

* Active Directory Enumeration
* Credential Attacks
* Kerberoasting
* Lateral Movement
* Privilege Escalation Testing

Tools:

* Nmap
* BloodHound
* CrackMapExec
* Impacket
* Evil-WinRM
* Kerbrute

---

# Endpoint Monitoring

## Windows Events

| Event ID | Description      |
| -------- | ---------------- |
| 4624     | Successful Login |
| 4625     | Failed Login     |
| 4672     | Admin Login      |
| 4720     | User Creation    |
| 4740     | Account Lockout  |

---

## Sysmon Events

| Event ID | Description           |
| -------- | --------------------- |
| 1        | Process Creation      |
| 3        | Network Connection    |
| 7        | Image Loaded          |
| 11       | File Creation         |
| 13       | Registry Modification |

---

## Linux Events

| Log File          | Purpose            |
| ----------------- | ------------------ |
| auth.log          | Authentication     |
| syslog            | System Activity    |
| kern.log          | Kernel Events      |
| apache access.log | Web Requests       |
| apache error.log  | Application Errors |

---

# Security Monitoring Objectives

* Detect Brute Force Attacks
* Monitor User Authentication
* Detect PowerShell Abuse
* Detect Credential Dumping
* Detect Kerberoasting Activity
* Monitor Linux Authentication Events
* Track Lateral Movement
* Investigate Suspicious Processes

---

# Skills Demonstrated

* Windows Administration
* Linux Administration
* Active Directory
* Endpoint Monitoring
* Splunk Universal Forwarder
* Sysmon Deployment
* Threat Detection
* Security Monitoring
* SOC Operations

---

# Author

Aditya Gundure

Cyber Security | Active Directory | Linux Administration | Windows Security | Splunk SIEM | SOC Analyst
