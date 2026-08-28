# ACT (Compatibility fix) SHIMS

```powershell
#Installation:
Start-Process "sdbinst.exe" -ArgumentList "-q `"$PSScriptRoot\Files\Anysys_ACT.sdb`"" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```

```powershell
#Uninstallation:
Start-Process "sdbinst.exe" -ArgumentList "-u `"$PSScriptRoot\Files\Anysys_ACT.sdb`"" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
