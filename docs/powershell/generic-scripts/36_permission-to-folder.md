# Permission to folder:

```powershell
Start-Process "cmd.exe" -ArgumentList "/C cacls `"$envprogramfiles\KiCad\10.0`" /t /c /e /p Users:F" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
