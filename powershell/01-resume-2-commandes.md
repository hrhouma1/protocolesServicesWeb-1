
## 1. **Introduction à la formation PowerShell pour Active Directory**
   - Pas de commandes spécifiques, il s'agit d'une présentation.

## 2. **Utilisation de PowerShell pour Active Directory**
   - **Pourquoi utiliser PowerShell** : Introduction, pas de commandes spécifiques.
   - **Découvrir les modules PowerShell pour Active Directory** :
     ```powershell
     Get-Module -ListAvailable
     Import-Module ActiveDirectory
     ```

## 3. **Installation et configuration d'Active Directory avec PowerShell**
   - **Installer Active Directory** :
     ```powershell
     Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
     ```
   - **Ajouter une nouvelle forêt** :
     ```powershell
     Install-ADDSForest -DomainName "example.com"
     ```
   - **Ajouter un nouveau domaine dans une forêt existante** :
     ```powershell
     Install-ADDSDomain -NewDomainName "sub.example.com" -ParentDomainName "example.com"
     ```
   - **Ajouter un contrôleur de domaine à un domaine existant** :
     ```powershell
     Install-ADDSDomainController -DomainName "example.com"
     ```
   - **Gérer les domaines et les forêts** :
     ```powershell
     Get-ADForest
     Get-ADDomain
     ```
   - **Déplacer un rôle FSMO d'un DC vers un autre** :
     ```powershell
     Move-ADDirectoryServerOperationMasterRole -Identity "TargetDC" -OperationMasterRole PDCEmulator
     ```

## 4. **Gestion des utilisateurs dans Active Directory avec PowerShell**
   - **Créer un nouveau compte utilisateur** :
     ```powershell
     New-ADUser -Name "John Doe" -GivenName "John" -Surname "Doe" -SamAccountName "jdoe" -UserPrincipalName "jdoe@example.com" -Path "OU=Users,DC=example,DC=com"
     ```
   - **Lister/Récupérer des informations sur un compte utilisateur** :
     ```powershell
     Get-ADUser -Identity "jdoe"
     ```
   - **Modifier un compte utilisateur** :
     ```powershell
     Set-ADUser -Identity "jdoe" -Title "IT Specialist"
     ```
   - **Supprimer un compte utilisateur** :
     ```powershell
     Remove-ADUser -Identity "jdoe"
     ```

## 5. **Gestion des groupes Active Directory avec PowerShell**
   - **Créer un nouveau groupe** :
     ```powershell
     New-ADGroup -Name "IT Group" -GroupCategory Security -GroupScope Global -Path "OU=Groups,DC=example,DC=com"
     ```
   - **Lister/Récupérer des informations sur un groupe** :
     ```powershell
     Get-ADGroup -Identity "IT Group"
     ```
   - **Lister les membres d'un groupe** :
     ```powershell
     Get-ADGroupMember -Identity "IT Group"
     ```
   - **Ajouter des membres à un groupe** :
     ```powershell
     Add-ADGroupMember -Identity "IT Group" -Members "jdoe"
     ```
   - **Lister les groupes auxquels appartient un membre** :
     ```powershell
     Get-ADUser -Identity "jdoe" -Properties MemberOf | Select-Object -ExpandProperty MemberOf
     ```
   - **Supprimer un membre d'un groupe** :
     ```powershell
     Remove-ADGroupMember -Identity "IT Group" -Members "jdoe"
     ```
   - **Modifier les propriétés d'un groupe** :
     ```powershell
     Set-ADGroup -Identity "IT Group" -Description "IT department group"
     ```
   - **Supprimer un groupe** :
     ```powershell
     Remove-ADGroup -Identity "IT Group"
     ```

## 6. **Gestion des ordinateurs dans Active Directory avec PowerShell**
   - **Lister/Récupérer des informations sur les ordinateurs** :
     ```powershell
     Get-ADComputer -Filter *
     ```
   - **Ajouter un ordinateur** :
     ```powershell
     New-ADComputer -Name "PC001" -Path "OU=Computers,DC=example,DC=com"
     ```
   - **Modifier les propriétés d'un ordinateur** :
     ```powershell
     Set-ADComputer -Identity "PC001" -Description "Workstation for IT"
     ```
   - **Supprimer un ordinateur** :
     ```powershell
     Remove-ADComputer -Identity "PC001"
     ```

## 7. **Administration des unités d'organisation (OU) avec PowerShell**
   - **Lister les unités d'organisation** :
     ```powershell
     Get-ADOrganizationalUnit -Filter *
     ```
   - **Créer une nouvelle unité d'organisation** :
     ```powershell
     New-ADOrganizationalUnit -Name "IT Department" -Path "DC=example,DC=com"
     ```
   - **Modifier les propriétés d'une unité d'organisation** :
     ```powershell
     Set-ADOrganizationalUnit -Identity "OU=IT Department,DC=example,DC=com" -Description "Unit for IT Department"
     ```
   - **Supprimer une unité d'organisation** :
     ```powershell
     Remove-ADOrganizationalUnit -Identity "OU=IT Department,DC=example,DC=com"
     ```

## 8. **Gestion des objets Active Directory avec PowerShell**
   - **Lister les objets** :
     ```powershell
     Get-ADObject -Filter *
     ```
   - **Déplacer un objet** :
     ```powershell
     Move-ADObject -Identity "CN=John Doe,OU=Users,DC=example,DC=com" -TargetPath "OU=IT,DC=example,DC=com"
     ```
   - **Créer/Renommer un objet** :
     ```powershell
     Rename-ADObject -Identity "CN=John Doe,OU=Users,DC=example,DC=com" -NewName "Johnny Doe"
     ```
   - **Modifier les propriétés d'un objet** :
     ```powershell
     Set-ADObject -Identity "CN=Johnny Doe,OU=IT,DC=example,DC=com" -Description "Lead Developer"
     ```
   - **Supprimer un objet** :
     ```powershell
     Remove-ADObject -Identity "CN=Johnny Doe,OU=IT,DC=example,DC=com"
     ```

## 9. **Gestion des comptes de service Active Directory avec PowerShell**
   - **Créer un compte de service Active Directory** :
     ```powershell
     New-ADServiceAccount -Name "MyServiceAccount" -DNSHostName "server.example.com"
     ```
   - **Installer un compte de service sur un serveur** :
     ```powershell
     Install-ADServiceAccount -Identity "MyServiceAccount"
     ```

## 10. **Stratégies de mot de passe dans Active Directory avec PowerShell**
   - **Récupérer la stratégie des mots de passe** :
     ```powershell
     Get-ADDefaultDomainPasswordPolicy
     ```
   - **Modifier la stratégie des mots de passe** :
     ```powershell
     Set-ADDefaultDomainPasswordPolicy -MinPasswordLength 10 -MaxPasswordAge 30.00:00:00
     ```

## 11. **Gestion des GPO avec PowerShell**
   - **Récupérer des informations sur les GPO** :
     ```powershell
     Get-GPO -All
     ```
   - **Créer une GPO et la lier à une OU** :
     ```powershell
     New-GPO -Name "New GPO" | New-GPLink -Target "OU=IT,DC=example,DC=com"
     ```

## 12. **Gestion des sites Active Directory avec PowerShell**
   - **Lister les sites disponibles** :
     ```powershell
     Get-ADReplicationSite -Filter *
     ```

## 13. **Découverte des Cmdlets de gestion Active Directory**
   - **Activer/Désactiver un compte** :
     ```powershell
     Disable-ADAccount -Identity "jdoe"
     Enable-ADAccount -Identity "jdoe"
     ```

## 14. **Ateliers pratiques sur la gestion Active Directory avec PowerShell**
   - **Exporter des utilisateurs dans un fichier CSV** :
     ```powershell
     Get-ADUser -Filter * | Export-Csv -Path "C:\Users.csv"
     ```

## 15. **Conclusion de la formation**
   - Pas de commandes spécifiques.

Ces commandes couvrent les principales actions que vous pouvez effectuer avec PowerShell pour gérer Active Directory.
