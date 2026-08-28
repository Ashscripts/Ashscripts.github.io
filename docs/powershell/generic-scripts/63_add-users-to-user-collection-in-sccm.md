# add users to user collection in sccm:

```powershell
"COMPANY\eitzjen","Test\silvjes","COMPANY\espioli","COMPANY\xuyan1","COMPANY\WONGHIM" | % { Add-CMUserCollectionDirectMembershipRule -CollectionName "OEMX_AlreadyApprovedUsers" -ResourceID (Get-CMUser -Name $_).ResourceID }
```
