# Investigation Notes

## Alert Summary

Detection Type:
File Creation

Detection Source:
Sysmon Event ID 11

Platform:
Windows

Collected By:
Wazuh Agent

---

## Timeline

08:12

Created directory

C:\SOCLab

↓

08:13

Created

TestFile.txt

↓

08:13

Modified

TestFile.txt

↓

08:15

Sysmon generated

Event ID 11

↓

08:15

Wazuh indexed the event

↓

SOC investigation performed

---

## Key Findings

Directory created:

C:\SOCLab

File created:

C:\SOCLab\TestFile.txt

Event Logged:

Sysmon Event ID 11

Collected by:

Wazuh Agent

Indexed into:

Wazuh Threat Hunting

---

## Important Fields

CreationUtcTime

TargetFilename

Image

ProcessId

ProcessGuid

User

Rule Description

---

## Security Analysis

Legitimate administrative activity generated the file.

In real-world environments, Event ID 11 may indicate:

- Malware dropper activity
- Payload staging
- Script creation
- Ransomware file generation
- Living-off-the-land techniques

Context is required before determining malicious intent.

---

## MITRE ATT&CK

T1059.001

PowerShell

T1059.003

Windows Command Shell

T1036

Masquerading

---

## Conclusion

Sysmon successfully generated Event ID 11.

Wazuh ingested the telemetry.

The event was available for investigation and threat hunting.

The lab successfully demonstrated endpoint file creation monitoring.
