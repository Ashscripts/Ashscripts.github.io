# Check if registry key exists

```powershell
If (!(Test-Path "HKLM:\SOFTWARE\WOW6432Node\Company\Packages\HSTToolsBundle_PK1_2.0_Platform_ENG_002") -and (!(HKLM:\SOFTWARE\Company\Packages\HSTToolsBundle_PK1_2.0_Platform_ENG_002")))
{
	# <cmd-lets>
}
```
