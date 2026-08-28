# Check for pending reboot:

```powershell
$pend = $false
 if (Get-ChildItem "HKLM:\Software\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending" -EA SilentlyContinue) { $pend = $true }
     if (Get-Item "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired" -EA SilentlyContinue) { $pend = $true }
If ($pend -eq $true)
{
# if reboot is pending; perform action
}
```
