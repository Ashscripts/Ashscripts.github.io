# Check the disk space:

```powershell
if ((Get-WMIObject Win32_Logicaldisk -filter "deviceid='C:'").FreeSpace -lt 25GB)	
{	
Show-ADTInstallationPrompt -Message 'Low Disk Space! 65GB of free disk space is required to proceed with the installation.' -ButtonLeftText 'OK' -Icon Error -NoWait
Close-ADTSession -ExitCode 1602	
}	
```
