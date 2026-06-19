# Active Directory Deployment Lab

## Author

**Aditya Gundure**

---

# Project Overview

This project demonstrates the deployment and configuration of a Microsoft Active Directory environment within a virtualized enterprise network. The lab is designed to simulate a real-world corporate infrastructure for cybersecurity, SOC operations, threat hunting, detection engineering, and Active Directory attack simulations.

---

<img width="1110" height="841" alt="Screenshot 2026-06-19 075830" src="https://github.com/user-attachments/assets/24ab6099-04a2-49cd-88d4-d9732e685f0e" />


# Objectives

* Deploy Active Directory Domain Services (AD DS)
* Configure DNS Services
* Create Enterprise Organizational Units (OUs)
* Create Domain Users and Groups
* Configure Shared Resources
* Apply Group Policies
* Prepare Infrastructure for Splunk Monitoring
* Simulate Enterprise Authentication Environment

---

# Lab Architecture

| System        | Role               | IP Address    |
| ------------- | ------------------ | ------------- |
| DC01          | Domain Controller  | 192.168.10.10 |
| SPLUNK01      | Splunk Enterprise  | 192.168.10.20 |
| WIN10-CLIENT1 | Domain Workstation | 192.168.10.30 |
| WIN10-CLIENT2 | Domain Workstation | 192.168.10.31 |
| KALI-LINUX    | Attacker Machine   | 192.168.10.50 |

Network:

```text
192.168.10.0/24
```

Domain:

```text
corp.local
```

---

# Environment Details

## Domain Controller

Hostname:

```text
DC01
```

Operating System:

```text
Windows Server 2022 Evaluation
```

Role:

```text
Active Directory Domain Services
DNS Server
```

---

# Active Directory Installation

## Configure Static IP

```text
IP Address : 192.168.10.10
Subnet Mask: 255.255.255.0
DNS Server : 127.0.0.1
```

Verify:

```powershell
ipconfig /all
```

---

## Rename Server

```powershell
Rename-Computer -NewName "DC01" -Restart
```

---

## Install AD DS Role

Open:

```text
Server Manager
→ Add Roles and Features
```

Install:

```text
Active Directory Domain Services
DNS Server
```

---

## Promote Server

Select:

```text
Promote this server to a domain controller
```

Choose:

```text
Add a new forest
```

Domain Name:

```text
corp.local
```

NetBIOS Name:

```text
CORP
```

Configure DSRM Password and complete installation.

Server automatically reboots.

---

# Verify Domain

Login:

```text
CORP\Administrator
```

Verify:

```powershell
Get-ADDomain
```

Expected Output:

```text
DNSRoot      : corp.local
NetBIOSName  : CORP
```

---

# Organizational Unit Structure

```text
corp.local
│
├── HR
├── Finance
├── SOC
├── IT
├── Servers
├── Workstations
└── Service Accounts
```

---

# Enterprise Users

| Username     | Department |
| ------------ | ---------- |
| jsmith       | HR         |
| admin.soc    | SOC        |
| finance.user | Finance    |
| helpdesk1    | IT         |

---

# Security Groups

| Group Name    | Purpose             |
| ------------- | ------------------- |
| HR_Users      | HR Department       |
| Finance_Users | Finance Department  |
| SOC_Analysts  | Security Operations |
| IT_Helpdesk   | IT Administration   |

Group Scope:

```text
Global
```

Group Type:

```text
Security
```

---

# Shared Folders

Directory:

```text
D:\Shares
```

Structure:

```text
D:\Shares\HR
D:\Shares\Finance
D:\Shares\SOC
D:\Shares\IT
```

Permissions:

| Folder  | Group         |
| ------- | ------------- |
| HR      | HR_Users      |
| Finance | Finance_Users |
| SOC     | SOC_Analysts  |
| IT      | IT_Helpdesk   |

---

# Group Policy Configuration

## Password Policy

Minimum Password Length:

```text
12 Characters
```

Password Complexity:

```text
Enabled
```

Maximum Password Age:

```text
90 Days
```

---

## Account Lockout Policy

Lockout Threshold:

```text
5 Failed Attempts
```

Lockout Duration:

```text
30 Minutes
```

---

## PowerShell Logging

Enable:

```text
Turn on Module Logging
Turn on Script Block Logging
```

---

## Audit Policy

Enable:

```text
Account Logon
Logon Events
Directory Service Access
Object Access
Privilege Use
Process Creation
```

---

# Security Monitoring Events

Important Windows Event IDs

| Event ID | Description                     |
| -------- | ------------------------------- |
| 4624     | Successful Logon                |
| 4625     | Failed Logon                    |
| 4672     | Special Privileges Assigned     |
| 4720     | User Account Created            |
| 4728     | User Added to Security Group    |
| 4732     | User Added to Local Group       |
| 4740     | Account Locked Out              |
| 4768     | Kerberos TGT Request            |
| 4769     | Kerberos Service Ticket Request |

---

# Validation Commands

Verify Domain:

```powershell
Get-ADDomain
```

Verify Domain Controller:

```powershell
Get-ADDomainController
```

Verify DNS:

```powershell
nslookup corp.local
```

Verify Replication:

```powershell
dcdiag
```

---

# Cyber Security Use Cases

This Active Directory environment supports:

* Active Directory Administration
* User Management
* Group Policy Management
* Windows Security Monitoring
* Kerberoasting Detection
* Pass-the-Hash Detection
* Lateral Movement Analysis
* Credential Dumping Detection
* Privilege Escalation Detection
* SIEM Log Collection
* SOC Investigation Workflows

---

# Skills Demonstrated

* Active Directory Administration
* Windows Server 2022
* DNS Administration
* Group Policy Management
* User & Group Management
* Security Hardening
* Enterprise Network Administration
* Identity and Access Management (IAM)
* SOC Operations
* Cyber Security Monitoring

---

# Future Enhancements

* Splunk Enterprise Integration
* Splunk Universal Forwarders
* Sysmon Deployment
* Windows Event Forwarding
* Sigma Rules
* MITRE ATT&CK Mapping
* Detection Engineering
* Threat Hunting Dashboards

---

# Author

**Aditya Gundure**

Cyber Security | Active Directory Security | SOC Operations | Splunk | Detection Engineering | Blue Team
