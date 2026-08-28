# importing the registry key:

```powershell
Start-Process "reg.exe" -ArgumentList "import `"C:\temp\uporthide.reg`"" -Wait -PassThru -ErrorAction SilentlyContinue
```
