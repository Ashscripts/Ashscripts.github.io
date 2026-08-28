# Process using more than 100 MB RAM usage

```powershell
#Processes using more than 100 MB RAM
Get-CimInstance Win32_Process |
Where-Object {
    $_.CommandLine -and
    $_.WorkingSetSize -gt 100MB
} |
Sort-Object Name |
Select-Object Name, CommandLine,
    @{Name='MemoryMB';Expression={[math]::Round($_.WorkingSetSize / 1MB, 2)}}
```

# Top 20 memory-consuming processes
```powershell
#Top 20 memory-consuming processes
Get-CimInstance Win32_Process |
Where-Object { $_.CommandLine } |
Sort-Object @{Expression={[int64]$_.WorkingSetSize}; Descending=$true} |
Select-Object -First 20 `
    Name,
    @{Name='MemoryMB';Expression={[math]::Round($_.WorkingSetSize / 1MB, 2)}},
    CommandLine
```