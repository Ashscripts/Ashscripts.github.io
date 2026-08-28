# Delete Registry key with subkeys PSADT 4.17+:

```powershell
Remove-ADTRegistryKey -Key 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run' -Recurse -ErrorAction SilentlyContinue
```
