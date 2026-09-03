# Copy file using xcopy:

```powershell
Start-Process "xcopy.exe" -ArgumentList "`"C:\Temp\client_specific.properties`" `"$env:programfiles\Siemens\tc24\portal\plugins\configuration\`" /Y" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
