# Fetch Installed Applications from Add or Remove Programs

Retrieve installed application information from the Windows registry locations used by **Add or Remove Programs**.

This script is useful for Windows administrators, application packagers, and support teams who need to inventory installed software without opening Control Panel.

## Overview

Windows stores installed application information in several registry locations. This script queries the common locations for machine-wide and per-user applications, filters the results, and returns useful application details.

The default example filters applications whose name contains **Siemens**.

## Requirements

* Windows PowerShell
* Access to the registry locations being queried
* No additional PowerShell modules are required

## Script

```powershell
$apps = Get-ItemProperty `
    HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*,
    HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*,
    HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* `
    -ErrorAction SilentlyContinue

$apps |
    Where-Object { $_.DisplayName -like "*Siemens*" } |
    Select-Object DisplayName, DisplayVersion, Publisher, InstallDate |
    Sort-Object DisplayName
```

## How It Works

### 1. Read Installed Application Information

`Get-ItemProperty` reads application information from multiple registry locations commonly used by Windows for installed software.

| Registry path                                                             | Purpose                                         |
| ------------------------------------------------------------------------- | ----------------------------------------------- |
| `HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*`             | 64-bit applications installed for all users     |
| `HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*` | 32-bit applications installed on 64-bit Windows |
| `HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*`             | Applications installed for the current user     |

The `-ErrorAction SilentlyContinue` parameter prevents non-critical registry errors from interrupting the query.

### 2. Filter Applications

The following statement filters the registry results so that only applications containing `Siemens` in their display name are returned:

```powershell
Where-Object { $_.DisplayName -like "*Siemens*" }
```

The wildcard characters allow matches where `Siemens` appears anywhere in the application name.

For example:

```text
Siemens NX
Siemens Teamcenter Integration
Siemens Automation License Manager
```

### 3. Select Relevant Properties

The script returns these properties:

| Property         | Description                       |
| ---------------- | --------------------------------- |
| `DisplayName`    | Installed application name        |
| `DisplayVersion` | Installed application version     |
| `Publisher`      | Software publisher                |
| `InstallDate`    | Installation date, when available |

This keeps the output focused on information that is normally useful for software inventory and troubleshooting.

### 4. Sort the Results

The final pipeline sorts the output alphabetically by application name:

```powershell
Sort-Object DisplayName
```

## Sample Output

```text
DisplayName                    DisplayVersion   Publisher        InstallDate
-----------                    --------------   ---------        -----------
Siemens NX                     2306             Siemens          20250115
Siemens Teamcenter Integration 14.3             Siemens Digital  20250115
```

The exact values depend on the applications installed on the computer.

## List All Installed Applications

To retrieve all detected installed applications instead of filtering for Siemens products, remove the `Where-Object` statement:

```powershell
$apps = Get-ItemProperty `
    HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*,
    HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*,
    HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* `
    -ErrorAction SilentlyContinue

$apps |
    Select-Object DisplayName, DisplayVersion, Publisher, InstallDate |
    Sort-Object DisplayName
```

## Filtering for Another Vendor or Product

The filter can easily be changed to another vendor or product name.

For example, to search for applications containing `Microsoft`:

```powershell
Where-Object { $_.DisplayName -like "*Microsoft*" }
```

You can also change the wildcard pattern to target a more specific application name.

## Benefits

This method can be used to:

* Inventory installed applications.
* Identify installed software versions.
* Troubleshoot application installations.
* Support application packaging and migration activities.
* Generate software inventory information.
* Find vendor-specific applications.

## Notes

This approach queries registry data directly rather than using the `Win32_Product` WMI class.

That distinction is important because `Win32_Product` is generally not recommended for routine software inventory on Windows systems due to its behavior and performance characteristics.

The registry-based approach also provides access to both machine-wide and current-user uninstall entries.

## Conclusion

This script provides a straightforward way to retrieve installed application information from the Windows registry locations associated with **Add or Remove Programs**.

By changing the `Where-Object` filter, the same script can be used to search for a specific vendor, product, or application pattern.
