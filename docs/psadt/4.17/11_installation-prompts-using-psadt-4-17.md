# Installation prompts using PSADT 4.17:

```powershell
#Installation prompts using PSADT 4.17:
<#checking running process Prompt the user to close Word, MSAccess and Excel. By using the PersistPrompt switch, the dialog will return to the center of the screen every couple of seconds, specified in the config.psd1, so the user cannot ignore it by dragging it aside.#>
Show-ADTInstallationWelcome -CloseProcesses @{ Name = 'winword' }, @{ Name = 'msaccess' }, @{ Name = 'excel' } -PersistPrompt
```
#alternative script

# Alternative script with deferred date and number of times to popup

```powershell
Show-ADTInstallationWelcome -CloseProcesses @{ Name = 'winword' }, @{ Name = 'excel' } -BlockExecution -AllowDefer -DeferTimes 10 -DeferDeadline '25/08/2013' -CloseProcessesCountdown 600

<#Allow the user to defer the deployment a maximum of 10 times or until the deadline is reached, whichever happens first.
When deferral expires, prompt the user to close the applications and automatically close them after 10 minutes. #>
```

#reference link: https://psappdeploytoolkit.com/docs/4.1.x/reference/functions/Show-ADTInstallationWelcome

