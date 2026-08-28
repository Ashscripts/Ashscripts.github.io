# Firewall Add Rule
```powershell
#adding rule
Start-Process "cmd.exe" -ArgumentList "/C netsh advfirewall firewall add rule name=`"SpaixAwv4`" dir=in program=`"$envProgramFilesx86\SULZER\ABSEL V2 Pump Selection Program\SpaixAw.exe`" remoteip=localsubnet action=allow profile=domain protocol=any" -Wait -PassThru -ErrorAction SilentlyContinue
```
# Firewall Delete Rule
```powershell
#deleting rule
Start-Process "cmd.exe" -ArgumentList "/C netsh advfirewall firewall delete rule name=`"UgrafNX2512`"" -Wait -PassThru -ErrorAction SilentlyContinue
```