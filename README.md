# wazuh-threat-hunting-lab12-file-creation-detection
## Overview

This lab demonstrates how to detect Windows file creation activity using Sysmon Event ID 11 and investigate the generated telemetry in Wazuh SIEM.

File creation monitoring is valuable because attackers frequently create files while:

- Dropping malware
- Saving payloads
- Creating scripts
- Writing ransomware files
- Establishing persistence

The objective is to generate file creation events, verify Sysmon logging, and analyze the event inside Wazuh Threat Hunting.

---

## Lab Objectives

- Verify Sysmon and Wazuh services
- Generate Windows File Create events
- Capture Sysmon Event ID 11
- Investigate alerts in Wazuh
- Analyze event fields
- Map findings to MITRE ATT&CK

---

## Lab Environment

Host Machine
- Windows 11

Monitoring
- Sysmon
- Wazuh Agent
- Wazuh Manager

PowerShell
- PowerShell 7

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Defense Evasion | Masquerading | T1036 |
| Command and Scripting Interpreter | PowerShell | T1059.001 |
| Command and Scripting Interpreter | Windows Command Shell | T1059.003 |

---

## Lab Steps

### 1. Verify Services

```powershell
Get-Service Sysmon64
Get-Service WazuhSvc
```

---

### 2. Create Test Directory

```powershell
New-Item -ItemType Directory C:\SOCLab -Force
```

---

### 3. Create Test File

```powershell
"Hello from Sysmon Lab" | Out-File C:\SOCLab\TestFile.txt
```

---

### 4. Modify File

```powershell
Add-Content C:\SOCLab\TestFile.txt "Second line"
```

---

### 5. Verify Sysmon Event ID 11

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=11)]]" `
-MaxEvents 20
```

---

### 6. Investigate in Wazuh

Search:

```
data.win.system.eventID:11
```

Open the event and review:

- TargetFilename
- Image
- Process ID
- User
- CreationUtcTime
- Rule Description

---

### 7. Cleanup

```powershell
Remove-Item C:\SOCLab -Recurse -Force
```

---

## Detection Logic

Sysmon generates Event ID 11 whenever a file is created.

Wazuh collects the Sysmon event and stores it for threat hunting and investigation.

---

## Indicators Observed

- File creation
- Parent process
- Image path
- Target filename
- User account
- Process ID
- Creation timestamp

---

## MITRE ATT&CK Summary

Technique:
- T1059.001 – PowerShell
- T1059.003 – Windows Command Shell
- T1036 – Masquerading

---

## Learning Outcomes

After completing this lab you can:

- Detect file creation activity
- Investigate Sysmon Event ID 11
- Hunt Windows telemetry in Wazuh
- Correlate endpoint events
- Analyze process and file metadata

---

