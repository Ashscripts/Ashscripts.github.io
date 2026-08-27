# Add Users to an SCCM User Collection

Bulk-add users to a Microsoft Configuration Manager (SCCM/MECM) user collection using PowerShell.

This approach allows administrators to import user accounts from a text file, validate whether the users already belong to the collection, and add only new direct membership rules.

Two methods are covered:

* Bulk import users from a text file.
* Add a small number of users directly with a PowerShell pipeline.

## Prerequisites

* Microsoft Configuration Manager console installed on the administration workstation.
* Access to the Configuration Manager PowerShell module.
* Permissions to modify SCCM user collections.
* A text file containing the user accounts to import.
* The users must be available in Configuration Manager through user discovery.

## Step 1: Create a User List File

Create a text file named `USR.TXT` with one user account per line.

```text
COMPANY\johnsmith
COMPANY\janedoe
COMPANY\michaeljones
COMPANY\alexbrown
COMPANY\davidwilson
```

Example file location:

```text
C:\Users\usr\Desktop\USR.TXT
```

Use domain-qualified usernames when possible.

## Step 2: Import the Configuration Manager Module

Before using the Configuration Manager cmdlets, import the Configuration Manager PowerShell module.

```powershell
Import-Module "E:\Program files\Microsoft Configuration Manager\AdminConsole\bin\ConfigurationManager\ConfigurationManager.psd1"
```

The path above is an example and may differ depending on where the Configuration Manager console is installed.

## Step 3: Connect to the SCCM Site

Specify the Configuration Manager site code and switch to the corresponding PowerShell site drive.

```powershell
$SiteCode = "S01"
Set-Location ($SiteCode + ":")
```

Replace `S01` with your actual site code.

## Step 4: Specify the User Collection

Define the target user collection.

```powershell
$CollectionName = "ALL_CLN_ACCOUNT"
```

Replace `ALL_CLN_ACCOUNT` with the name of the collection you want to modify.

## PowerShell Script

The following script reads users from the text file, checks whether each user exists in Configuration Manager, checks whether the user is already a collection member, and adds a direct membership rule when required.

```powershell
# Import SCCM module
Import-Module "E:\Program files\Microsoft Configuration Manager\AdminConsole\bin\ConfigurationManager\ConfigurationManager.psd1"

# Set Site Code
$SiteCode = "S01"
Set-Location ($SiteCode + ":")

# Collection Name
$CollectionName = "ALL_CLN_ACCOUNT"

# TXT file path
$TxtFilePath = "C:\Users\usr\Desktop\USR.TXT"

# Read users from TXT file
$UserList = Get-Content $TxtFilePath

# Get existing members
$ExistingMembers = Get-CMUserCollectionMember -CollectionName $CollectionName

foreach ($UserName in $UserList) {

    try {

        # Get SCCM User
        $CMUser = Get-CMUser -Name $UserName

        if ($CMUser) {

            if ($ExistingMembers.ResourceId -contains $CMUser.ResourceId) {

                Write-Host "$UserName already exists in collection" `
                    -ForegroundColor Yellow
            }
            else {

                Add-CMUserCollectionDirectMembershipRule `
                    -CollectionName $CollectionName `
                    -ResourceId $CMUser.ResourceId

                Write-Host "Added: $UserName" `
                    -ForegroundColor Green
            }
        }
        else {

            Write-Host "User not found in SCCM: $UserName" `
                -ForegroundColor Magenta
        }
    }
    catch {

        Write-Host "Error processing $UserName : $_" `
            -ForegroundColor Red
    }
}

Write-Host "User import completed." -ForegroundColor Cyan
```

The Material theme provides a **Copy** button for this code block.

## How the Script Works

### 1. Load the Configuration Manager Module

The script imports the Configuration Manager PowerShell module so that SCCM-specific cmdlets such as `Get-CMUser` and `Add-CMUserCollectionDirectMembershipRule` are available.

### 2. Connect to the Configuration Manager Site

The site code is used to create the Configuration Manager PowerShell drive.

```powershell
$SiteCode = "S01"
Set-Location ($SiteCode + ":")
```

Without switching to the Configuration Manager site drive, Configuration Manager cmdlets that rely on the site provider may not behave as expected.

### 3. Read the User List

The script reads the contents of the text file:

```powershell
$UserList = Get-Content $TxtFilePath
```

Each line becomes a username that is processed individually.

### 4. Retrieve Existing Collection Members

The script retrieves the collection's existing members:

```powershell
$ExistingMembers = Get-CMUserCollectionMember -CollectionName $CollectionName
```

The returned resource IDs are later used to prevent duplicate membership rules.

### 5. Find the User in Configuration Manager

For each username, the script attempts to retrieve the corresponding Configuration Manager user:

```powershell
$CMUser = Get-CMUser -Name $UserName
```

If the user cannot be found, the script reports:

```text
User not found in SCCM: <username>
```

### 6. Check for Existing Membership

Before adding the user, the script checks whether the user's `ResourceId` already exists in the collection:

```powershell
if ($ExistingMembers.ResourceId -contains $CMUser.ResourceId)
```

This prevents the script from attempting to create a duplicate direct membership rule.

### 7. Add the User

When the user exists in Configuration Manager and is not already a collection member, the script adds a direct membership rule:

```powershell
Add-CMUserCollectionDirectMembershipRule `
    -CollectionName $CollectionName `
    -ResourceId $CMUser.ResourceId
```

### 8. Display the Result

The script displays status messages using different console colors to make successful additions, existing memberships, missing users, and errors easier to distinguish.

## Duplicate Membership Protection

A key feature of this script is duplicate validation.

```powershell
if ($ExistingMembers.ResourceId -contains $CMUser.ResourceId)
```

This checks the user's Configuration Manager resource ID against the existing members of the target collection.

As a result, users who are already members are skipped rather than having another direct membership rule attempted.

## Status Messages

| Message                                 | Meaning                                               |
| --------------------------------------- | ----------------------------------------------------- |
| `Added: Username`                       | The user was successfully added to the collection.    |
| `Username already exists in collection` | The user is already a member of the collection.       |
| `User not found in SCCM: Username`      | The user could not be found in Configuration Manager. |
| `Error processing Username`             | An exception occurred while processing the user.      |
| `User import completed.`                | Processing of the supplied user list has finished.    |

## Alternative One-Line Method

For a small number of users, a text file may not be necessary.

The following pipeline adds several users directly to a collection:

```powershell
"COMPANY\eitzjen",
"COMPANY\silvjes",
"COMPANY\espioli",
"COMPANY\xuyan1",
"COMPANY\WONGHIM" |
ForEach-Object {
    Add-CMUserCollectionDirectMembershipRule `
        -CollectionName "OEMX_AlreadyApprovedUsers" `
        -ResourceID (Get-CMUser -Name $_).ResourceID
}
```

This method is useful for quick administrative tasks where only a small number of users need to be added.

Unlike the bulk-import script, this example does not explicitly perform the same validation and error handling.

## When to Use Each Method

| Method                      | Recommended scenario                                                       |
| --------------------------- | -------------------------------------------------------------------------- |
| Text File Import            | Large user lists and recurring tasks.                                      |
| One-Line Script             | Small number of users.                                                     |
| Bulk Import with Validation | Production environments where validation and error handling are important. |

## Common Use Cases

This approach can be useful for:

* Software deployment targeting.
* Pilot user collections.
* Application approval workflows.
* User migration projects.
* Testing and UAT deployments.
* Privilege management collections.
* Compliance and remediation deployments.

## Troubleshooting

| Issue                | Resolution                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| User not found       | Verify that Active Directory User Discovery is working and that the account has been discovered by Configuration Manager. |
| Access denied        | Confirm that the account has sufficient Configuration Manager permissions.                                                |
| Collection not found | Verify the collection name and confirm that you are connected to the correct site.                                        |
| Module import error  | Verify the Configuration Manager console installation path and the location of `ConfigurationManager.psd1`.               |
| No users added       | Check the username format in the source file and verify that the users exist in Configuration Manager.                    |

## Best Practices

* Use domain-qualified usernames.
* Validate that user accounts are available in Configuration Manager before importing them.
* Maintain the source user list for audit and repeatability.
* Test the script in a lab environment before using it against production collections.
* Add logging when processing large user lists.
* Keep duplicate membership validation enabled for production use.
* Review collection membership after the import completes.
* Confirm that the correct Configuration Manager site and collection are targeted before execution.

!!! tip
For recurring application deployments, maintain a controlled master user list and automate collection population with PowerShell to reduce manual administration.

## Notes

This script creates **direct membership rules** in the specified Configuration Manager user collection.

The source user accounts must already be available to Configuration Manager. If `Get-CMUser` cannot find an account, the script will not add it to the collection.

The script also uses the existing collection membership information captured before processing begins. For normal imports this is sufficient to prevent duplicate additions.

## Conclusion

PowerShell provides a scalable way to add users to Configuration Manager collections.

The text-file method is appropriate for larger user lists because it provides per-user processing, duplicate validation, user discovery validation, status messages, and exception handling.

For small administrative tasks, the direct pipeline method can be more convenient.

Before using either method in production, verify the Configuration Manager site, collection name, source user accounts, and administrative permissions.
