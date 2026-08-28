# Executing a process with psadt 4.17:

```powershell
Start-ADTProcess -FilePath 'setup.exe' -ArgumentList '/S'
#-IgnoreExitCodes 1,2
#-WindowStyle 'Hidden'
#-ErrorAction SilentlyContinue, Stop
```
#reference link: https://psappdeploytoolkit.com/docs/4.1.x/reference/functions/Start-ADTProcess