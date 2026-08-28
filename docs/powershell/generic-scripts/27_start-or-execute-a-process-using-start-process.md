# Start or execute a process using Start-Process:

```powershell
Start-Process "msiexec.exe" -ArgumentList "/i `"$PSScriptRoot\Files\test.msi`" MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /qn" -Wait -PassThru -WindowStyle hidden -ErrorAction SilentlyContinue
```
