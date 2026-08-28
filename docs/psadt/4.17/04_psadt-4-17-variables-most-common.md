# PSADT 4.17 Variables most common:

```powershell
#Current folder variable
$PSScriptRoot = Split-Path -Parent -Path $MyInvocation.MyCommand.Definition
```
```powershell
#psadt fetching UAN for PSADT 4.17+
$adtSession.UAN
```
```powershell
#psadt fetching Files\ folder path for PSADT 4.17+
$($adtSession.DirFiles)
```
```powershell
#psadt fetching SupportFiles\ folder path for PSADT 4.17+
$($adtSession.DirSupportFiles)
```
