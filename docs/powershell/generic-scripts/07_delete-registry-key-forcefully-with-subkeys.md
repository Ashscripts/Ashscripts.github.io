# Delete Registry key forcefully with subkeys:

```bat
Remove-Item "HKLM:\Software\Test\HSTToolsBundle_PK1_2.0_Platform_ENG_001" -Force -Recurse -ErrorAction SilentlyContinue
```
