# Replacing a string in a file:

```powershell
$XmlPath = "C:\Users\<username>\AppData\Roaming\PhraseExpress\config.xml"
$Placeholder = "USERNAME_C"
$CurrentUser = $env:USERNAME
$content = Get-Content -Path $XmlPath -Raw -Encoding UTF8 -ErrorAction SilentlyContinue
$content = $content -replace $Placeholder, $CurrentUser
Set-Content -Path $XmlPath -Value $content -Encoding UTF8 -ErrorAction SilentlyContinue
```
#alternative script
```powershell
$settingsFile = "$env:AppData\Docker\settings.json"
$settingsFile2 = "$env:AppData\Docker\settings-store.json"
if (Test-Path $settingsFile)
{
    $content = Get-Content $settingsFile -Raw -ErrorAction SilentlyContinue
    $updatedContent = $content -replace '"WslUpdateRequired"\s*:\s*true', '"WslUpdateRequired": false'
    Set-Content $settingsFile -Value $updatedContent -Encoding utf8NoBOM -Force -ErrorAction SilentlyContinue
}
if (Test-Path $settingsFile2)
{
    $content2 = Get-Content $settingsFile2 -Raw -ErrorAction SilentlyContinue
    $updatedContent2 = $content2 -replace '"WslUpdateRequired"\s*:\s*true', '"WslUpdateRequired": false'
    Set-Content $settingsFile2 -Value $updatedContent2 -Encoding utf8NoBOM -Force -ErrorAction SilentlyContinue
}
```
