# SCCM Application in SCCM Console properties locked - fix

```powershell
Unlock-CMObject -InputObject $(Get-CMApplication -Name 'ApplicationName') -Force
```
