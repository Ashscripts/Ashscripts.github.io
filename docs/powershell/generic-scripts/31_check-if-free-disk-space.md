# Check if free disk space:

```powershell
Add-Type -AssemblyName System.Windows.Forms	
if ((Get-WMIObject Win32_Logicaldisk -filter "deviceid='C:'").FreeSpace -lt 25GB)	
{	
$message = "Hello, this is a message box!"	
[System.Windows.Forms.MessageBox]::Show($message, "Message Box", [System.Windows.Forms.MessageBoxButtons]::OK, [System.Windows.Forms.MessageBoxIcon]::Information)	
Exit-Script -ExitCode '9999'	
}	
```

oneliner to check free disk space:
```powershell
Get-PSDrive C | Select-Object Name, @{Name="FreeGB";Expression={[math]::Round($_.Free/1GB,2)}}, @{Name="TotalGB";Expression={[math]::Round($_.Used/1GB + $_.Free/1GB,2)}}
```