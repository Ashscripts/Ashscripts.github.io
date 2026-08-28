# copy file to all users profile using psadt 4.17:

```powershell
Copy-ADTFileToUserProfiles -Path "$($adtSession.DirSupportFiles)\config.txt" -Destination "AppData\Roaming\MyApp" -ErrorAction SilentlyContinue
```
