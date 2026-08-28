# sccm distribution point removal:

```powershell
Set-Location "S01:"
$ApplicationName = "VLCMediaPlayer_3.0.21_x64_ENG_001-copy"
$DistributionPoint = "se0210piscm003.company.com"
$distgrp = "03 UAT Distribution points"
$distgrp1 = "00 All DPs"
$distgrp2 = "Cloud DP"
# Remove the content from the specified distribution point
#Remove-CMContentDistribution -ApplicationName $ApplicationName -DistributionPointName $DistributionPoint -DisableContentDependencyDetection
Remove-CMContentDistribution -ApplicationName $ApplicationName -DistributionPointGroupName $distgrp -Force
Remove-CMContentDistribution -ApplicationName $ApplicationName -DistributionPointGroupName $distgrp1 -Force
Remove-CMContentDistribution -ApplicationName $ApplicationName -DistributionPointGroupName $distgrp2 -Force
```
