# Adding devices to sccm collection:


```powershell
$SiteCode        = "S01"  # 🔧 CHANGE to your site code
$ProviderMachine = "SEPCSCM008.COMPANY.com" 
# ===== VARIABLES =====
#$SiteCode = "ABC"                     # Your SCCM Site Code
$CollectionName = "Drawio_PreviousInstalled"
$DeviceListPath = "C:\Users\shaiadm\Desktop\dev.txt"
# ===== LOAD SCCM MODULE =====
Import-Module "E:\Program files\Microsoft Configuration Manager\AdminConsole\bin\ConfigurationManager\ConfigurationManager.psd1"
Set-Location "$SiteCode`:"
# ===== GET COLLECTION =====
$Collection = Get-CMDeviceCollection -Name $CollectionName
if (!$Collection) {
    Write-Error "Collection not found: $CollectionName"
    return
}
# ===== READ DEVICES =====
$Devices = Get-Content $DeviceListPath
foreach ($DeviceName in $Devices) {
    $Device = Get-CMDevice -Name $DeviceName
    if ($Device) {
        # Check if rule already exists
        $ExistingRule = Get-CMDeviceCollectionDirectMembershipRule `
            -CollectionId $Collection.CollectionID `
            | Where-Object { $_.ResourceID -eq $Device.ResourceID }
        if (!$ExistingRule) {
            Add-CMDeviceCollectionDirectMembershipRule `
                -CollectionId $Collection.CollectionID `
                -ResourceId $Device.ResourceID
            Write-Host "Added $DeviceName" -ForegroundColor Green
        }
        else {
            Write-Host "$DeviceName already exists in collection" -ForegroundColor Yellow
        }
    }
    else {
        Write-Host "Device not found in SCCM: $DeviceName" -ForegroundColor Red
    }
}
# ===== UPDATE COLLECTION =====
Invoke-CMDeviceCollectionUpdate -Id $Collection.CollectionID
```
