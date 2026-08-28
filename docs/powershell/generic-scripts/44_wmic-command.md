# wmic command:

```powershell
Get-CimInstance -Classname WIn32_Product | Where-Object Name -Like '*kofax*' | Invoke-CimMethod -MethodName UnInstall -ErrorAction SilentlyContinue
```

#alternative script
```powershell
$productCode = (Get-WmiObject -Class Win32_Product | Where-Object { $_.Name -like "*GlobalProtect*" }).IdentifyingNumber
    if ($productCode) {
        $args = "/x $productCode MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /qn"
        Start-Process "msiexec.exe" -ArgumentList $args -Wait -NoNewWindow -ErrorAction SilentlyContinue
    }  
```


#reference link: https://support.microsoft.com/en-US/servicing/os/windows/docs/2025/09/windows-management-instrumentation-command-line-wmic-removal-from-windows
