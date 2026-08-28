# DPInst operations:

> Source lines: 1005–1015

#Driver Installation:
```powershell
Start-Process "$PSScriptRoot\Files\driversinternal\dpinst.exe" -ArgumentList "/A /Q /SA /SE" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```

#Driver Uninstallation:
```powershell
Start-Process "$PSScriptRoot\Files\driversinternal\dpinst.exe" -ArgumentList "/Q /D /U `"C:\Windows\System32\DriverStore\FileRepository\vuh.inf_amd64_eea746b107e00bff\vuh.inf`"" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
