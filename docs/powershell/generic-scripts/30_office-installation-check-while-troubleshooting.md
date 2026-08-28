# Office installation check while troubleshooting:

```powershell
#check office architecture
reg query "HKLM\SOFTWARE\Microsoft\Office\ClickToRun\Configuration" /v Platform
```

```powershell
#check entire configuration
reg query "HKLM\SOFTWARE\Microsoft\Office\ClickToRun\Configuration"
```

```powershell
#check if ClickToRunSvc is running
Get-Service ClickToRunSvc -ErrorAction SilentlyContinue
```
