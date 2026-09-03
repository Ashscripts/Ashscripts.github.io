# copying folder using xcopy:

```powershell
Start-Process "xcopy.exe" -ArgumentList "`"C:\Temp\client_specific`" `"$env:programfiles\Siemens\tc24\portal\plugins\client_specific`" /E /H /Y /I" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
