# Delete Registry value:

```Powershell
Remove-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name 'Simatic_OAM' -Force -ErrorAction SilentlyContinue
```
