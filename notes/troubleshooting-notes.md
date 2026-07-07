# Troubleshooting Notes

## Issue 1

No Event ID 11 displayed.

Cause

File creation had not yet occurred or the Sysmon configuration had not produced recent File Create events.

Resolution

Created a new folder and file, then verified Event ID 11 again.

---

## Issue 2

Search returned no results.

Cause

The filter searched for "SOCLab" but the latest Event ID 11 records did not contain that string.

Resolution

Used a broader query:

data.win.system.eventID:11

Then inspected the latest events.

---

## Issue 3

Wazuh event not immediately visible.

Cause

Agent ingestion delay.

Resolution

Waited a few seconds and refreshed Threat Hunting.

---

## Issue 4

Unexpected file creation events appeared.

Cause

Windows and applications constantly create temporary files.

Resolution

Focused investigation on the manually created:

C:\SOCLab\TestFile.txt

---

## Useful Commands

Verify Services

```powershell
Get-Service Sysmon64
Get-Service WazuhSvc
```

View Event ID 11

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=11)]]"
```

Create Folder

```powershell
New-Item -ItemType Directory C:\SOCLab -Force
```

Create File

```powershell
"Hello from Sysmon Lab" | Out-File C:\SOCLab\TestFile.txt
```

Modify File

```powershell
Add-Content C:\SOCLab\TestFile.txt "Second line"
```

Cleanup

```powershell
Remove-Item C:\SOCLab -Recurse -Force
```

---

## Lessons Learned

- Sysmon Event ID 11 records file creation activity.
- Wazuh successfully ingests and indexes these events.
- Threat hunting becomes more effective when correlating file activity with process details and user context.
- File creation telemetry is useful for detecting malware deployment, payload staging, and other post-exploitation behaviors.
