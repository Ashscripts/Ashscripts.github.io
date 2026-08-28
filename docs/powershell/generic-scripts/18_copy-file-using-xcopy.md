# Copy file using xcopy:

```powershell
Start-Process "xcopy.exe" -ArgumentList "`"$($adtSession.DirFiles)\client_specific.properties`" `"$envprogramfiles\Siemens\tc24\portal\plugins\configuration\`" /Y" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
