
## 2. Utilisation de PowerShell pour Active Directory

```powershell
Import-Module ActiveDirectory
Get-Command -Module ActiveDirectory
```

## 3. Installation et configuration d'AD

```powershell
Install-WindowsFeature AD-Domain-Services
Install-ADDSForest
Install-ADDSDomain  
Add-ADDSDomainController
Get-ADForest
Get-ADDomain
Move-ADDirectoryServerOperationMasterRole
```

## 4. Gestion des utilisateurs

```powershell
New-ADUser
Get-ADUser
Set-ADUser  
Remove-ADUser
```

## 5. Gestion des groupes

```powershell
New-ADGroup
Get-ADGroup
Get-ADGroupMember
Add-ADGroupMember
Get-ADPrincipalGroupMembership
Remove-ADGroupMember
Set-ADGroup
Remove-ADGroup
```

## 6. Gestion des ordinateurs  

```powershell
Get-ADComputer
New-ADComputer
Set-ADComputer
Remove-ADComputer
```

## 7. Gestion des OU

```powershell
Get-ADOrganizationalUnit
New-ADOrganizationalUnit
Set-ADOrganizationalUnit  
Remove-ADOrganizationalUnit
```

## 8. Gestion des objets AD

```powershell
Get-ADObject
Move-ADObject
New-ADObject
Rename-ADObject
Set-ADObject
Remove-ADObject
```

## 9. Comptes de service

```powershell
New-ADServiceAccount
Set-ADServiceAccount
Install-ADServiceAccount
New-ScheduledTaskPrincipal
Uninstall-ADServiceAccount
Remove-ADServiceAccount
```

## 10. Stratégies de mot de passe

```powershell
Get-ADDefaultDomainPasswordPolicy
Set-ADDefaultDomainPasswordPolicy
New-ADFineGrainedPasswordPolicy
```

## 11. Gestion des GPO

```powershell
Get-GPO
New-GPO
New-GPLink
Backup-GPO
Restore-GPO
Remove-GPLink
Set-GPPermission
Copy-GPO
Rename-GPO
Remove-GPO
```

## 12. Gestion des sites AD

```powershell
Get-ADReplicationSite
New-ADReplicationSite
Move-ADDirectoryServer
Get-ADReplicationSubnet
New-ADReplicationSubnet
Set-ADReplicationSite
Remove-ADReplicationSite
Set-ADReplicationSubnet
Remove-ADReplicationSubnet
```

## 13. Cmdlets de gestion AD

```powershell
Enable-ADAccount
Disable-ADAccount
Unlock-ADAccount
Search-ADAccount -AccountInactive
Set-ADAccountPassword
Set-ADUser -PasswordNeverExpires $true
```

# Citations:

[1] https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adserviceaccount?view=windowsserver2022-ps

[2] https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adgroup?view=windowsserver2019-ps

[3] https://www.lepide.com/how-to/add-remove-ad-groups-and-objects-in-groups-with-powershell.html

[4] https://specopssoft.com/blog/managing-active-directory-password-policy-with-powershell/

[5] https://rdr-it.io/en/active-directory-create-an-organizational-unit-ou-in-powershell/

[6] https://www.readandexecute.com/how-to/server-2016/active-directory/installing-active-directory-with-powershell-windows-server-2016/
[7] https://activedirectorypro.com/find-service-accounts-in-active-directory/
[8] https://www.cbtnuggets.com/blog/certifications/microsoft/6-common-active-directory-powershell-commands
[9] https://documentation.sailpoint.com/connectors/active_directory/help/integrating_active_directory/powershell.html
[10] https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps
