# Winget install, uninstall and detection method

Install winget app:
```powershell
Winget Install GitHub.Copilot --silent --accept-source-agreements --accept-package-agreements
```

Uninstall winget app:
```powershell
winget uninstall --id Microsoft.VCLibs.Desktop.14 -e --silent --all-versions
exit 0
```
Winget Detection Script:
```powershell
$InstallCheck = winget list --id GitHub.Copilot --exact --accept-source-agreements
    if ($InstallCheck -like "*GitHub.Copilot*") {
Write-output "Installed"  
          Exit 0
    }
Clear-Host  
  Exit 0
```
