# Creating a registry key:

```powershell
$regpath32 = "HKLM:\Software\Wow6432Node\Packages"
new-item -path $regpath32 -force -ErrorAction SilentlyContinue
Set-ItemProperty -force -path $regpath32 -name "Vendor" -value "test" -ErrorAction SilentlyContinue
```
