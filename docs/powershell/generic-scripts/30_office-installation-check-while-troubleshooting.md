# Office installation check while troubleshooting:
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" ,
"HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" |
Where-Object {
    $_.DisplayName -match "Microsoft 365|Office"
} |
Select-Object DisplayName, DisplayVersion, Publisher
```

Check office architecture
```powershell
#check office architecture
reg query "HKLM\SOFTWARE\Microsoft\Office\ClickToRun\Configuration" /v Platform
```
Check entire configuration
```powershell
#check entire configuration
reg query "HKLM\SOFTWARE\Microsoft\Office\ClickToRun\Configuration"
```
Check if ClickToRunSvc is running
```powershell
#check if ClickToRunSvc is running
Get-Service ClickToRunSvc -ErrorAction SilentlyContinue
```
Check Office Applications Exist
```powershell
$OfficePath = "${env:ProgramFiles}\Microsoft Office\root\Office16"

Get-ChildItem $OfficePath -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -match "WINWORD|EXCEL|POWERPNT|OUTLOOK"
} |
Select Name

```
Output would be like:
WINWORD.EX
EXCEL.EXE
POWERPNT.EXE
OUTLOOK.EXE
