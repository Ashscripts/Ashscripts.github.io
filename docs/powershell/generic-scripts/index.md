# PowerShell Generic Scripts

A collection of reusable PowerShell scripts for Windows administration, software inventory, troubleshooting, automation, registry management, SCCM/MECM administration, Intune/Company Portal troubleshooting, file operations, process management, and application packaging.

## Scripts

### Registry and System Administration

* [Check if Registry Key Exists](01_check-if-registry-key-exists.md)
* [Allow Remote Desktop Connection (RDP) via Registry](02_allow-remote-desktop-connection-rdp-via-registry.md)
* [Import Registry for All Existing User Profiles Without Logoff](04_import-registry-for-all-existing-users-profiles-without-logoff.md)
* [Check for Pending Reboot](05_check-for-pending-reboot.md)
* [Suppress Pending Soft Reboot](06_suppress-pending-soft-reboot-by-deleting-the-pendingfilerenameoperations-reg-data.md)
* [Delete Registry Key Forcefully with Subkeys](07_delete-registry-key-forcefully-with-subkeys.md)
* [Delete Registry Value](08_delete-registry-value.md)
* [Batch Script Reference for Deleting Value](09_batch-script-reference-for-deleting-value.md)
* [Delete Registry Keys Using Matching String](10_delete-registry-keys-using-matching-string.md)
* [Check Registry Data Value](11_check-registry-data-value.md)
* [Export Bulk Registry Keys into a Single REG File](12_export-bulk-registry-keys-into-a-single-reg-file.md)
* [Creating a Registry Key](13_creating-a-registry-key.md)
* [Importing the Registry Key](14_importing-the-registry-key.md)
* [Check if Registry Key Exists](15_check-if-registry-key-exist.md)

### File and Folder Operations

* [Create Folder if Not Exists](16_create-folder-if-not-exists.md)
* [Copying Folder Using XCOPY](17_copying-folder-using-xcopy.md)
* [Copy File Using XCOPY](18_copy-file-using-xcopy.md)
* [Replacing a String in a File](19_replacing-a-string-in-a-file.md)
* [Replacing a String in an XML File](20_replacing-a-string-in-an-xml-file.md)
* [Check Folder Size](21_check-folder-size.md)
* [Read a Text File Until String Within It Is Found](23_read-a-text-file-until-string-within-it-is-found.md)
* [If Folder Empty Then Delete](24_if-folder-empty-then-delete.md)
* [Delete Folder from All Existing User Profiles](25_delete-folder-from-all-existing-users-profile.md)
* [Delete Registry Keys from All Existing User Profiles](26_delete-registry-keys-from-all-existing-users-profle.md)
* [Copy File to All Existing User Profiles](51_copy-file-to-all-existing-user-profiles.md)
* [Zip and Unzip Files](52_zip-and-unzip-files.md)
* [Firewall Operations](Firewall_operations.md)


### Software Installation and Application Management

* [Winget Install, Uninstall and Detection Method](03_winget-install-uninstall-and-detection-method.md)
* [Check for a File Version](22_check-for-a-file-version.md)
* [Start or Execute a Process Using Start-Process](27_start-or-execute-a-process-using-start-process.md)
* [Fetch Add/Remove Programs (ARP) Entries](28_fetch-add-and-remove-programs-arp-entries-with-powershell.md)
* [Stop and Delete a Service](29_stop-and-delete-a-service.md)
* [Office Installation Check](30_office-installation-check-while-troubleshooting.md)
* [DPInst Operations](39_dpinst-operations.md)
* [DLL Register or De-Register](40_dll-register-or-de-register.md)
* [PowerShell Paths](41_powershell-paths.md)
* [Unblock File - Windows Defender SmartScreen](43_unblock-file-windows-defender-smart-screen-prevented-an-unrecognized-app-fixed.md)
* [WMIC Command](44_wmic-command.md)
* [Check if .NET Framework Is Enabled and Uninstallation](45_check-if-net-framework-is-enabled-and-uninstallation.md)
* [Deleting Registries and Uninstalling App](53_deleting-registries-and-uninstalling-app.md)
* [ACT Compatibility Fix / Shims](54_act-compatibility-fix-shims.md)
* [Process Using More Than 100 MB RAM](56_process-using-more-than-100-mb-ram-usage.md)
* [Monitor Background Running Process](57_monitor-background-running-process-that-s-difficult-to-catch-background-running-setup-moni.md)

### SCCM / MECM

* [SCCM Policies Rerun](32_sccm-policies-rerun.md)
* [SCCM Client Removal Script](34_sccm-client-removal-script.md)
* [SCCM Client Repair](35_sccm-client-repair.md)
* [SCCM Application in SCCM Console Properties Locked - Fix](37_sccm-application-in-sccm-console-properties-locked-fix.md)
* [Update Distribution Points](48_update-distribution-points.md)
* [SCCM Download Stuck at 0%](50_sccm-download-stuck-at-0.md)
* [Get the SCCM Application Deployment ID](58_get-the-sccm-application-deployment-id.md)
* [Adding Devices to SCCM Collection](59_adding-devices-to-sccm-collection.md)
* [Check Application Is Deployed to What Collections](60_check-application-is-deployed-to-what-collections.md)
* [Check Dependent Applications in SCCM](61_check-dependent-applications-in-sccm.md)
* [SCCM Distribution Point Removal](62_sccm-distribution-point-removal.md)
* [Add Users to User Collection in SCCM](63_add-users-to-user-collection-in-sccm.md)
* [Add Users to User Collection Using TXT File](64_add-users-to-user-collection-using-txt-file.md)
* [Fetch Specific Application in Software Center](68_fetch-specific-application-in-software-center.md)
* [SCCM Query Collection for Installed Application](69_sccm-query-collection-to-fetch-the-devices-that-has-specific-application-installed-fetch-u.md)
* [CCMCache Cleanup Script](70_ccmcache-cleanup-script.md)

### Intune / Company Portal

* [Company Portal Repair and Fix](38_company-portal-repair-and-fix.md)
* [Company Portal Enrollment Issue Fix](49_company-portal-enrollment-issue-fix.md)

### User Interface and Troubleshooting

* [Unpin Shortcut from Taskbar](33_unpin-shortcut-from-taskbar.md)
* [HTA Window Reference](42_hta-window-reference.md)
* [Eventlog Fetch](47_eventlog-fetch.md)
* [Permission to Folder](36_permission-to-folder.md)
* [Fetch Running Tasks in Task Manager with Command Line](55_fetch-running-tasks-in-taskmanager-with-command-line.md)

### Application Packaging

* [Application Details and Software Center Description](65_script-to-get-displayname-version-manufacturer-owner-details-and-software-center-descripti.md)
* [Package Visual Studio Professional](66_how-to-package-visual-studio-professional-application.md)
* [Package SQL Server](67_how-to-package-sql-server-application.md)

## About This Section

This section contains standalone PowerShell scripts that can be used independently of a specific application deployment framework.

Each script page provides:

* An overview of the script.
* Requirements and usage information.
* The complete PowerShell source code.
* A Copy button for the code.
* An explanation of how the script works.
* Examples and notes where applicable.
