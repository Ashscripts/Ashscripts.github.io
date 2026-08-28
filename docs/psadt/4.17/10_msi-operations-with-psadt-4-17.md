# MSI operations with PSADT 4.17:

```powershell
#install with transforms
Start-ADTMsiProcess -Action 'Install' -FilePath 'OnsightConnectEnterprise.msi' -Transforms 'LibrestreamOnsighConnect_11.4.18.55050_x86_ENG_001.mst'
#MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S
```
```powershell
#install only msi
Start-ADTMsiProcess -Action 'Install' -FilePath 'Adobe_FlashPlayer_11.2.202.233_x64_EN.msi'
```
```powershell
#install only patch
Start-ADTMsiProcess -Action 'Patch' -FilePath 'Adobe_Reader_11.0.3_EN.msp'
```
```powershell
#uninstall msi
Start-ADTMsiProcess -Action 'Uninstall' -ProductCode '{8AF832CF-9D85-4B2E-AA2B-5667F9974909}' -ArgumentList 'MSIRESTARTMANAGERCONTROL=Disable MSIRMSHUTDOWN=2 REBOOT=ReallySuppress MSIDISABLERMRESTART=1 REBOOTPROMPT=S /QN'
```
