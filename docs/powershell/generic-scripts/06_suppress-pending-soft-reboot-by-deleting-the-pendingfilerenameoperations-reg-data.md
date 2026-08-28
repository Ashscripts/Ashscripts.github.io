# Suppress pending soft reboot by deleting the  PendingFileRenameOperations reg data:

```bat
Remove-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager' -Name 'PendingFileRenameOperations' -Force -ErrorAction SilentlyContinue
```
