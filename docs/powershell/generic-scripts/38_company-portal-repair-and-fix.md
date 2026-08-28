
# Company Portal Repair and Fix

This guide provides troubleshooting steps for situations where **Microsoft Company Portal** or **Intune Win32 application deployments** become stuck and new applications stop processing.

A common symptom is the following message in the Intune Management Extension (IME) logs:

```text
Win32 application workload thread is already in progress
````

When this condition occurs, Win32 application processing may become blocked.

!!! warning
The troubleshooting steps below range from a simple service restart to more invasive remediation. Start with the least disruptive option and proceed only when necessary.

## Symptoms

Typical symptoms include:

* New Win32 applications do not begin processing.
* Company Portal shows an application as **Installing** for an extended period.
* Applications remain queued.
* Required application deployments stop progressing.
* IME logs repeatedly report that a Win32 application workload thread is already in progress.

## How the Win32 Application Workload Normally Works

Under normal conditions, the Intune Management Extension (IME) processes Win32 application workloads in a sequence similar to:

```text
IME Service
    ↓
Starts Win32 workload thread
    ↓
Downloads application
    ↓
Installs application
    ↓
Runs detection
    ↓
Updates status to Intune
    ↓
Thread exits
```

## What Happens When the Workload Becomes Stuck

In the affected state, the workload thread may fail to complete:

```text
IME starts workload
    ↓
Thread hangs, crashes, or becomes stuck
    ↓
Workload remains marked as in progress
    ↓
Next workload starts
    ↓
"Win32 application workload thread is already in progress"
```

As a result:

* New Win32 applications may not process.
* Company Portal may continue displaying **Installing**.
* Applications may remain queued.
* Required application deployments may stop progressing.

## IME Log Location

The primary log used for troubleshooting is:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log
```

You can open the log with:

```powershell
notepad "C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log"
```

Useful strings to search for include:

```text
Processing app
Installing app
Waiting for install
Detection failed
Win32 application workload thread is already in progress
```

---

# Fix 1: Restart the Intune Management Extension Service

The first troubleshooting step should be a simple IME service restart.

Open **PowerShell as Administrator** and run:

```powershell
Restart-Service IntuneManagementExtension -Force
```

Alternatively:

```powershell
net stop IntuneManagementExtension
net start IntuneManagementExtension
```

After restarting the service, wait approximately **2–5 minutes** and monitor the IME log:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log
```

Check whether application processing resumes.

---

# Fix 2: Check for Stuck Installer Processes

A Win32 application workload may remain blocked if the installer process launched by the workload is still running.

Check for common installer processes:

```powershell
Get-Process setup -ErrorAction SilentlyContinue
Get-Process msiexec -ErrorAction SilentlyContinue
Get-Process office -ErrorAction SilentlyContinue
```

You can also check the processes most commonly associated with Office and other application installers:

```powershell
Get-Process msiexec,setup,OfficeClickToRun -ErrorAction SilentlyContinue
```

Common processes to investigate include:

| Process                | Typical purpose                          |
| ---------------------- | ---------------------------------------- |
| `msiexec.exe`          | Windows Installer                        |
| `setup.exe`            | Application installer/bootstrapper       |
| `OfficeClickToRun.exe` | Microsoft 365 Apps / Office Click-to-Run |
| Visio installer        | Visio installation or update process     |

!!! warning
Do not terminate an installer simply because it is running. First determine whether it is legitimately installing or updating software. Terminating a valid installation can leave the application in a partially installed state.

If a process has been running for an unusually long time and is confirmed to be stuck, it may be preventing the IME workload from completing.

---

# Fix 3: Completely Restart the IME Service and Process

If restarting the service does not resolve the problem, completely stop the IME service and terminate the IME process.

First stop the service:

```powershell
Stop-Service IntuneManagementExtension -Force
```

Then terminate the IME process if it is still running:

```powershell
Get-Process Microsoft.Management.Services.IntuneWindowsAgent -ErrorAction SilentlyContinue |
    Stop-Process -Force
```

Start the service again:

```powershell
Start-Service IntuneManagementExtension
```

Then allow several minutes for the IME to initialize and begin processing workloads again.

---

# Fix 4: Clear IME Agent State

If the workload continues to remain stuck, clearing the local IME policy and content state can force the agent to rebuild its local state.

!!! warning
This is more invasive than restarting the service. Ensure that the device is properly enrolled in Intune and that you understand the impact before performing this step.

## Stop the IME Service

```powershell
Stop-Service IntuneManagementExtension -Force
```

## Rename the Policies Directory

Rename:

```text
C:\Program Files (x86)\Microsoft Intune Management Extension\Policies
```

to:

```text
Policies.old
```

PowerShell example:

```powershell
Rename-Item `
    "C:\Program Files (x86)\Microsoft Intune Management Extension\Policies" `
    "C:\Program Files (x86)\Microsoft Intune Management Extension\Policies.old"
```

## Rename the Content Directory

Rename:

```text
C:\Program Files (x86)\Microsoft Intune Management Extension\Content
```

to:

```text
Content.old
```

PowerShell example:

```powershell
Rename-Item `
    "C:\Program Files (x86)\Microsoft Intune Management Extension\Content" `
    "C:\Program Files (x86)\Microsoft Intune Management Extension\Content.old"
```

## Start the IME Service

```powershell
Start-Service IntuneManagementExtension
```

The IME can then rebuild its local state and synchronize policy and application content again.

---

# Fix 5: Check Enrollment Status Page (ESP)

Sometimes an application failure during device provisioning or enrollment can prevent expected application processing.

Review the following Windows event logs:

```text
Applications and Services Logs
    ↓
Microsoft
    ↓
Windows
    ↓
DeviceManagement-Enterprise-Diagnostics-Provider
```

Also review:

```text
Microsoft-Windows-ModernDeployment-Diagnostics-Provider
```

Look for events related to:

* Application installation failures.
* Detection failures.
* Timeout conditions.
* Enrollment or provisioning errors.
* Required applications that remain incomplete.

An application that repeatedly fails during ESP processing may require separate remediation.

---

# Fix 6: Reinstall the Intune Management Extension

If multiple remediation attempts fail, a full IME reinstallation can be considered.

!!! warning
Reinstalling the Intune Management Extension is more disruptive than restarting the service. Use it only after simpler remediation steps have been exhausted.

Stop the service:

```powershell
Stop-Service IntuneManagementExtension -Force
```

Uninstall **Microsoft Intune Management Extension** from:

```text
Control Panel
    ↓
Programs and Features
```

After removal, allow the device to synchronize with Intune.

You can also trigger a manual synchronization from:

```text
Settings
    ↓
Accounts
    ↓
Access work or school
    ↓
Select the connected work account
    ↓
Info
    ↓
Sync
```

When the device receives the appropriate policy, Intune can reinstall the IME as required.

---

# Find the Application Blocking the Workload

The most important troubleshooting step is identifying which application caused the workload to become stuck.

Open:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log
```

Search for:

```text
Processing app
```

Then investigate nearby entries such as:

```text
Installing app
Waiting for install
Detection failed
```

A common pattern is:

```text
Processing app: Microsoft Visio
```

followed by no successful completion or detection result.

This can indicate that the application is the workload currently preventing the IME thread from completing.

## Quick Process Check

Run:

```powershell
Get-Process msiexec,setup,OfficeClickToRun -ErrorAction SilentlyContinue
```

This helps identify active installer processes that may correspond to the application being processed.

## Review the Most Recent IME Log Entries

To retrieve the last 50 lines:

```powershell
Get-Content `
    "C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log" `
    -Tail 50
```

You can use this together with the workload error:

```text
Win32 application workload thread is already in progress
```

to identify where application processing stopped.

---

# Recommended Troubleshooting Order

Use the following order from least invasive to most invasive:

```text
1. Restart IME Service
        ↓
2. Check for stuck installer processes
        ↓
3. Restart IME service and process completely
        ↓
4. Clear IME Policies and Content state
        ↓
5. Check ESP and Windows event logs
        ↓
6. Reinstall the Intune Management Extension
```

Start with the simplest remediation and escalate only when required.

---

# Registry Remediation

If the device's enrollment state is suspected to be corrupted, administrators may encounter situations where enrollment-related registry entries require cleanup.

The commonly referenced registry location is:

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Enrollments
```

!!! danger
Do not delete enrollment registry keys as a first troubleshooting step. Removing enrollment-related registry information can affect device management and enrollment state. Back up the relevant registry information and confirm the remediation procedure for your organization's enrollment model before making changes.

If registry cleanup is required as part of a documented remediation procedure, back up the relevant registry data first and remove only the specific enrollment entries identified as problematic.

---

# Useful PowerShell Commands

## Restart IME

```powershell
Restart-Service IntuneManagementExtension -Force
```

## Check IME Service

```powershell
Get-Service IntuneManagementExtension
```

## Check Installer Processes

```powershell
Get-Process msiexec,setup,OfficeClickToRun -ErrorAction SilentlyContinue
```

## Check IME Process

```powershell
Get-Process Microsoft.Management.Services.IntuneWindowsAgent -ErrorAction SilentlyContinue
```

## Show Recent IME Log Entries

```powershell
Get-Content `
    "C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log" `
    -Tail 50
```

---

# Conclusion

When Intune Win32 application processing becomes stuck and the IME reports:

```text
Win32 application workload thread is already in progress
```

the first objective should be to identify the workload or installer that is preventing the IME thread from completing.

Start by restarting the IME service and checking for stuck installer processes. If the problem persists, progressively move to a complete IME restart, clearing local IME state, reviewing ESP and event logs, and finally reinstalling the Intune Management Extension.

The IME log is the primary source of information for identifying the application that is holding up the workload.

````

One change I made deliberately is the registry section. Your original opening line says to delete the subkeys under `HKLM\SOFTWARE\Microsoft\Enrollments`, but that is potentially a much more consequential remediation than the other steps. I would not present blanket deletion as the default repair procedure in your public documentation.

For your site, I would categorize this as:

```text
PowerShell
└── Generic Scripts
    └── Company Portal Repair and Fix
````

and it can use the same Copy buttons as your other pages.
