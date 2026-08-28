# copying multiple file to all users profile using psadt 4.17:

```powershell
Copy-ADTFileToUserProfiles -Path "$($adtSession.DirSupportFiles)\config.txt","$($adtSession.DirSupportFiles)\config2.txt" -Destination "AppData\Roaming\MyApp" -ErrorAction SilentlyContinue
```
