# Eventlog fetch:

## List Application events
```powershell

Get-WinEvent -FilterHashtable @{
    LogName = 'Application'
    Id      = 1000
} -MaxEvents 10 |
Select-Object `
    TimeCreated,
    @{Name='Application';Expression={
        if ($_.Message -match 'Faulting application name:\s*([^,]+)') {$matches[1]}
    }},
    @{Name='FaultModule';Expression={
        if ($_.Message -match 'Faulting module name:\s*([^,]+)') {$matches[1]}
    }}
```

## List Application events in detail

```powershell
  Get-WinEvent -FilterHashtable @{
    LogName = 'Application'
    Id = 1000
} -MaxEvents 20 |
Select-Object TimeCreated, ProviderName, Id, Message |
Format-List
```

## List Application events for specific ids

```powershell
Get-WinEvent -LogName Application -MaxEvents 200 |
Where-Object {$_.Id -in 1000,1001,1026} |
Select-Object TimeCreated,Id,ProviderName,Message |
Format-List
```
