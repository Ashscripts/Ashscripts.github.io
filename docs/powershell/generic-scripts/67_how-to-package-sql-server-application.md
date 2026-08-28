# How to package sql server application

## SQL2025‑SSEI‑StdDev.exe

Download the offline content to:
SQL2025-SSEI-StdDev.exe /Action=Download /MediaPath="C:\SQL2025"


```powershell
$sqlarg = "/ConfigurationFile=`"$envwindir\Temp\SQL2025_SRC\ConfigurationFile.ini`" /IAcceptSQLServerLicenseTerms /ACTION=Install /INSTANCENAME=MSSQLSERVER /QUIET=True /SKIPRULES=FusionRebootCheck /ENU"
        Start-ADTProcess -FilePath "$envwindir\Temp\SQL2025_SRC\SETUP.EXE" -ArgumentList $sqlarg -WindowStyle 'Hidden' -ErrorAction Stop
```

## Configuration file: ConfigurationFile.ini

```powershell
;SQL Server 2025 Configuration File
[OPTIONS]
PRODUCTCOVEREDBYSA="False"
SUPPRESSPRIVACYSTATEMENTNOTICE="False"
UpdateEnabled="True"
USEMICROSOFTUPDATE="False"
SUPPRESSPAIDEDITIONNOTICE="False"
UpdateSource="MU"
FEATURES=SQLENGINE
HELP="False"
INSTANCENAME="MSSQLSERVER"
INSTALLSHAREDDIR="C:\Program Files\Microsoft SQL Server"
INSTALLSHAREDWOWDIR="C:\Program Files (x86)\Microsoft SQL Server"
SQLTELSVCSTARTUPTYPE="Automatic"
SQLTELSVCACCT="NT Service\SQLTELEMETRY"
INSTANCEDIR="C:\Program Files\Microsoft SQL Server"
AGTSVCACCOUNT="NT Service\SQLSERVERAGENT"
AGTSVCSTARTUPTYPE="Manual"
SQLSVCSTARTUPTYPE="Automatic"
FILESTREAMLEVEL="0"
SQLMAXDOP="1"
ENABLERANU="False"
SQLSVCACCOUNT="NT Service\MSSQLSERVER"
SQLSVCINSTANTFILEINIT="False"
SQLSYSADMINACCOUNTS="Administrators"
SQLTEMPDBFILECOUNT="1"
SQLTEMPDBFILESIZE="8"
SQLTEMPDBFILEGROWTH="64"
SQLTEMPDBLOGFILESIZE="8"
SQLTEMPDBLOGFILEGROWTH="64"
ADDCURRENTUSERASSQLADMIN="False"
TCPENABLED="1"
NPENABLED="0"
BROWSERSVCSTARTUPTYPE="Disabled"
SQLMAXMEMORY="2147483647"
SQLMINMEMORY="0"
```

## Configuration file: uninstallConfig.ini

```powershell
;SQL Server 2019 Configuration File
[OPTIONS]
; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter. 
ACTION="Uninstall"
; Specifies that SQL Server Setup should not display the privacy statement when ran from the command line. 
SUPPRESSPRIVACYSTATEMENTNOTICE="False"
; Use the /ENU parameter to install the English version of SQL Server on your localized Windows operating system. 
ENU="True"
; Setup will not display any user interface. 
QUIET="False"
; Setup will display progress only, without any user interaction. 
QUIETSIMPLE="False"
; Parameter that controls the user interface behavior. Valid values are Normal for the full UI,AutoAdvance for a simplied UI, and EnableUIOnServerCore for bypassing Server Core setup GUI block. 
;UIMODE="Normal"
; Specifies that SQL Server Setup should not display the paid edition notice when ran from the command line. 
SUPPRESSPAIDEDITIONNOTICE="False"
; Specifies features to install, uninstall, or upgrade. The list of top-level features include SQL, AS, IS, MDS, and Tools. The SQL feature will install the Database Engine, Replication, Full-Text, and Data Quality Services (DQS) server. The Tools feature will install shared components. 
FEATURES=SQLENGINE
; Displays the command line parameters usage 
HELP="False"
; Specifies that the detailed Setup log should be piped to the console. 
INDICATEPROGRESS="False"
; Specifies that Setup should install into WOW64. This command line argument is not supported on an IA64 or a 32-bit system. 
;X86="False"
; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the SQL Server Database Engine (SQL), or Analysis Services (AS). 
INSTANCENAME="MSSQLSERVER"
```