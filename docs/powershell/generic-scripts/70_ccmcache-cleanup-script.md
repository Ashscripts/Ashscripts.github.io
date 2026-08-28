# ccmcache cleanup script:

```powershell
# CCM Cache Cleanup
$ParentFolder = "C:\Windows\ccmcache"
$DetectionFile = "C:\ProgramData\CCMCleanup.txt"

    # Determine the cache folder the script is running from
    $CurrentCacheFolder = (Get-Item -Path $PSScriptRoot -ErrorAction SilentlyContinue).FullName
    # Remove all cache folders except the current one
    Get-ChildItem -Path $ParentFolder -Directory -ErrorAction SilentlyContinue |
        Where-Object { $_.FullName -ne $CurrentCacheFolder } |
        ForEach-Object {
            Remove-Item -Path $_.FullName -Recurse -Force -ErrorAction SilentlyContinue            
        }
    # Create/update detection file
    Set-Content -Path $DetectionFile -Value "CCM Cache cleanup completed on $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')" ` -Force -ErrorAction SilentlyContinue
exit 0
```