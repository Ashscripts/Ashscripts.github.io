# Wait until the folder is found or created at runtime VBScript:

```vbscript
ON ERROR RESUME NEXT		
		
Set fso = CreateObject("Scripting.FileSystemObject")		
		
' Specify the folder to monitor and the filename to search for		
folder = "C:\temp\New folder" ' Replace with the actual folder path		
searchFile = "file.txt" ' Replace with the actual filename		
		
Do While True		
    ' Check if the file exists		
    If fso.FileExists(folder & "\" & searchFile) Then		
        ' File found! Perform actions here		
        MsgBox "File found: " & folder & "\" & searchFile		
        Exit Do ' Exit the loop		
    End If		
		
    ' File not found, wait for a specified interval		
    WScript.Sleep 5000 ' Wait for 5 seconds (adjust as needed)		
Loop		
```

# PowerShell script alternative
```powershell
wait until folder is found or created at runtime:
$folderPath = "C:\temp\New folder\a"  # Replace with the actual folder path
while (!(Test-Path $folderPath)) {
    Start-Sleep -Seconds 5  # Check for the folder every 5 seconds
}
#operation
```