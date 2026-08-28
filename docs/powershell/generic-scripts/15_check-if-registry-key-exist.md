# Check if registry key exist:

```powershell
If ((Test-Path "HKLM:\SOFTWARE\WOW6432Node\Test\Packages\HSTToolsBundle_PK1_2.0_Platform_ENG_002") -OR (Test-Path "HKLM:\SOFTWARE\Test\Packages\HSTToolsBundle_PK1_2.0_Platform_ENG_002")) 
{
       # <cmd-lets>
}
```