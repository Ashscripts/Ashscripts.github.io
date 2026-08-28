# Unblock File: Windows Defender Smart Screen prevented an unrecognized app - Fixed

```powershell
get-childitem "$envProgramFilesX86\Ahlborn\ALMEMO_Control-6-3" -ErrorAction SilentlyContinue | unblock-file -ErrorAction SilentlyContinue
```
