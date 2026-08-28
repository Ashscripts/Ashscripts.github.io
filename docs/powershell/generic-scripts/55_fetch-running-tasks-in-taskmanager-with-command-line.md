# Fetch running tasks in taskmanager with command-line:

```powershell
Get-CimInstance Win32_Process |
Where-Object { $_.CommandLine } |
Sort-Object Name |
Format-Table Name, CommandLine -AutoSize -Wrap
```

# Fetch running tasks in taskmanager with command-line and exporting to csv
```powershell
#exporting to csv
Get-CimInstance Win32_Process |
Where-Object { $_.CommandLine } |
Sort-Object Name |
Select-Object Name, CommandLine |
Export-Csv "C:\Temp\RunningProcesses.csv" -NoTypeInformation
```
