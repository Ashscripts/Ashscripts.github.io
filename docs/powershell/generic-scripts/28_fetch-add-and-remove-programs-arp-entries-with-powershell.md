# Fetch add and remove programs (ARP entries) with powershell:

> Source lines: 466–514

```powershell
$uninstallPaths = @(
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*",
    "HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*"
)
foreach ($path in $uninstallPaths) {
    Get-ItemProperty $path | Where-Object {
        $_.DisplayName -like "*Aspen*" -and $_.PSObject.Properties.Name -contains "UninstallString"
    } | ForEach-Object {
        $productName = $_.DisplayName
        $uninstallString = $_.UninstallString
        # Extract product code if uninstall string contains it
        if ($uninstallString -match "\{[0-9A-Fa-f\-]{36}\}") {
            $productCode = $Matches[0]
            Write-Output "msiexec /x $productCode #$productName"
        }
    }
}
# alternative script for fetching ARP entries:

$paths = @(
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*",
    "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*"
)
foreach ($path in $paths) {
    Get-ItemProperty $path -ErrorAction SilentlyContinue | Where-Object {
        #$_.DisplayName -like "*Aspen*" -and $_.UninstallString -match "msiexec"
        $_.Publisher -like "*Aspen*" -and $_.UninstallString -match "msiexec"
    } | ForEach-Object {
        $name = $_.DisplayName
        $code = $_.PSChildName
        if ($code -match "^\{.*\}$") {
            $shortName = ($name -replace '\s', '').Substring(0, [Math]::Min(16, ($name -replace '\s', '').Length))
            #Write-Output "Start-ADTMsiProcess -Action 'Uninstall' -ProductCode '$code' -ArgumentList 'MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /qn' #$shortName"
            #Write-Output "msiexec /x $code MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /qn #$shortName"
            #Write-Output "msiexec /x $code MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /qn"
        }
    }
}
```
