# copy folders and sub folders using psadt 4.17:

```powershell
Copy-ADTFile -Path "$($adtSession.DirFiles)\CefLibZip\*" -Destination "$envlocalappdata\Programs\Tagetik Excel .NET Client" -Recurse
```
