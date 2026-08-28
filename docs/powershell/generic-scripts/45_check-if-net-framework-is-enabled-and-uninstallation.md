# Check if .net framework is enabled and uninstallation:

#installation
```powershell
DISM /Online /Get-FeatureInfo /FeatureName:NetFx3
```

#uninstallation:
```powershell
dism.exe /online /disable-feature /featurename:NetFx3 /quiet /norestart
```
