# sccm download stuck at 0%:

```powershell
Net stop ccmexec
DEL C:\Windows\CCM\CcmStore.sdf
Net start ccmexec
```
