# Résumé sans explications :


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


# Explications: 

Je comprends que vous êtes débutant et que vous avez besoin d'explications plus détaillées. Je vais vous fournir des explications simples pour chaque commande, en indiquant les parties que vous devrez personnaliser. Commençons par les commandes de base :

## 2. Utilisation de PowerShell pour Active Directory

```powershell
Import-Module ActiveDirectory
```
Cette commande charge le module Active Directory dans PowerShell. Vous n'avez rien à modifier ici.

```powershell
Get-Command -Module ActiveDirectory
```
Cette commande affiche toutes les commandes disponibles pour Active Directory. Aucune modification nécessaire.

## 4. Gestion des utilisateurs

```powershell
New-ADUser -Name "John Doe" -SamAccountName "jdoe" -UserPrincipalName "jdoe@votredomaine.com" -Path "OU=Utilisateurs,DC=votredomaine,DC=com"
```
Cette commande crée un nouvel utilisateur. Modifiez :
- "John Doe" : Nom complet de l'utilisateur
- "jdoe" : Nom de connexion de l'utilisateur
- "jdoe@votredomaine.com" : Adresse email de l'utilisateur
- "OU=Utilisateurs,DC=votredomaine,DC=com" : Chemin où l'utilisateur sera créé dans AD

```powershell
Get-ADUser -Identity "jdoe"
```
Cette commande récupère les informations d'un utilisateur. Remplacez "jdoe" par le nom d'utilisateur que vous recherchez.

```powershell
Set-ADUser -Identity "jdoe" -Description "Nouvel employé"
```
Cette commande modifie un utilisateur. Remplacez "jdoe" par le nom d'utilisateur à modifier et "Nouvel employé" par la description souhaitée.

```powershell
Remove-ADUser -Identity "jdoe"
```
Cette commande supprime un utilisateur. Remplacez "jdoe" par le nom d'utilisateur à supprimer.

## 5. Gestion des groupes

```powershell
New-ADGroup -Name "Groupe Marketing" -GroupScope Global
```
Cette commande crée un nouveau groupe. Remplacez "Groupe Marketing" par le nom du groupe que vous souhaitez créer.

```powershell
Get-ADGroup -Identity "Groupe Marketing"
```
Cette commande récupère les informations d'un groupe. Remplacez "Groupe Marketing" par le nom du groupe que vous recherchez.

```powershell
Add-ADGroupMember -Identity "Groupe Marketing" -Members "jdoe"
```
Cette commande ajoute un membre à un groupe. Remplacez "Groupe Marketing" par le nom du groupe et "jdoe" par le nom d'utilisateur à ajouter.

```powershell
Remove-ADGroup -Identity "Groupe Marketing"
```
Cette commande supprime un groupe. Remplacez "Groupe Marketing" par le nom du groupe à supprimer.


--------------------------------


## 6. Gestion des ordinateurs

```powershell
Get-ADComputer -Filter * -Properties *
```
Cette commande liste tous les ordinateurs dans Active Directory. Vous n'avez rien à modifier ici.

```powershell
New-ADComputer -Name "PC001" -SamAccountName "PC001" -Path "OU=Ordinateurs,DC=votredomaine,DC=com"
```
Cette commande crée un nouvel ordinateur dans AD. Modifiez :
- "PC001" : Nom de l'ordinateur
- "OU=Ordinateurs,DC=votredomaine,DC=com" : Chemin où l'ordinateur sera créé

```powershell
Set-ADComputer -Identity "PC001" -Description "Ordinateur de la salle de conférence"
```
Cette commande modifie les propriétés d'un ordinateur. Remplacez "PC001" par le nom de l'ordinateur et modifiez la description selon vos besoins.

```powershell
Remove-ADComputer -Identity "PC001"
```
Cette commande supprime un ordinateur. Remplacez "PC001" par le nom de l'ordinateur à supprimer.

## 7. Gestion des OU (Unités d'Organisation)

```powershell
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName
```
Cette commande liste toutes les OU. Vous n'avez rien à modifier ici.

```powershell
New-ADOrganizationalUnit -Name "Marketing" -Path "DC=votredomaine,DC=com"
```
Cette commande crée une nouvelle OU. Remplacez "Marketing" par le nom de l'OU et modifiez le chemin selon votre structure AD.

```powershell
Set-ADOrganizationalUnit -Identity "OU=Marketing,DC=votredomaine,DC=com" -Description "Département Marketing"
```
Cette commande modifie une OU. Ajustez le chemin de l'OU et la description selon vos besoins.

```powershell
Remove-ADOrganizationalUnit -Identity "OU=Marketing,DC=votredomaine,DC=com" -Recursive
```
Cette commande supprime une OU. Modifiez le chemin de l'OU à supprimer. Le paramètre -Recursive supprime également tous les objets contenus dans l'OU.

## 8. Gestion des objets AD

```powershell
Get-ADObject -Filter * -Properties *
```
Cette commande liste tous les objets AD. Vous n'avez rien à modifier ici.

```powershell
Move-ADObject -Identity "CN=John Doe,OU=Ancien,DC=votredomaine,DC=com" -TargetPath "OU=Nouveau,DC=votredomaine,DC=com"
```
Cette commande déplace un objet AD. Modifiez les chemins source et cible selon votre structure AD.

```powershell
Rename-ADObject -Identity "CN=AncienNom,OU=Utilisateurs,DC=votredomaine,DC=com" -NewName "NouveauNom"
```
Cette commande renomme un objet AD. Modifiez le chemin de l'objet et le nouveau nom selon vos besoins.

Ces commandes vous permettent de gérer les éléments de base dans Active Directory. N'oubliez pas de toujours vérifier les chemins et les noms avant d'exécuter une commande, surtout pour les opérations de suppression ou de modification. 




----------------------------------------

Certainement, continuons avec les sections suivantes :

## 9. Gestion des comptes de service

```powershell
New-ADServiceAccount -Name "SvcAccount1" -DNSHostName "SvcAccount1.votredomaine.com"
```
Cette commande crée un nouveau compte de service. Remplacez :
- "SvcAccount1" par le nom souhaité pour le compte de service
- "votredomaine.com" par votre nom de domaine

```powershell
Set-ADServiceAccount -Identity "SvcAccount1" -Description "Compte pour le service XYZ"
```
Cette commande modifie un compte de service. Remplacez :
- "SvcAccount1" par le nom du compte de service à modifier
- "Compte pour le service XYZ" par la description souhaitée

```powershell
Install-ADServiceAccount -Identity "SvcAccount1"
```
Cette commande installe un compte de service sur un serveur. Remplacez "SvcAccount1" par le nom du compte de service à installer.

## 10. Stratégies de mot de passe

```powershell
Get-ADDefaultDomainPasswordPolicy
```
Cette commande affiche la stratégie de mot de passe par défaut du domaine. Aucune modification nécessaire.

```powershell
Set-ADDefaultDomainPasswordPolicy -MinPasswordLength 12 -MaxPasswordAge "60.00:00:00"
```
Cette commande modifie la stratégie de mot de passe par défaut. Ajustez :
- 12 : longueur minimale du mot de passe
- "60.00:00:00" : durée maximale avant expiration (ici 60 jours)

```powershell
New-ADFineGrainedPasswordPolicy -Name "PasswordPolicyAdmin" -Precedence 10 -MinPasswordLength 14
```
Cette commande crée une nouvelle stratégie de mot de passe fine. Modifiez :
- "PasswordPolicyAdmin" : nom de la nouvelle stratégie
- 10 : priorité de la stratégie (plus le chiffre est bas, plus la priorité est élevée)
- 14 : longueur minimale du mot de passe pour cette stratégie

## 11. Gestion des GPO (Group Policy Objects)

```powershell
Get-GPO -All
```
Cette commande liste toutes les GPO. Aucune modification nécessaire.

```powershell
New-GPO -Name "GPO-Securite" | New-GPLink -Target "OU=IT,DC=votredomaine,DC=com"
```
Cette commande crée une nouvelle GPO et la lie à une OU. Remplacez :
- "GPO-Securite" par le nom souhaité pour la GPO
- "OU=IT,DC=votredomaine,DC=com" par le chemin de l'OU cible

```powershell
Backup-GPO -Name "GPO-Securite" -Path "C:\GPOBackups"
```
Cette commande sauvegarde une GPO. Remplacez :
- "GPO-Securite" par le nom de la GPO à sauvegarder
- "C:\GPOBackups" par le chemin où vous voulez sauvegarder la GPO

```powershell
Remove-GPLink -Name "GPO-Securite" -Target "OU=IT,DC=votredomaine,DC=com"
```
Cette commande supprime le lien d'une GPO. Ajustez le nom de la GPO et le chemin de l'OU cible.

Ces commandes vous permettent de gérer les comptes de service, les stratégies de mot de passe et les GPO dans Active Directory. 
Comme toujours, soyez prudent lors de l'exécution de ces commandes, surtout celles qui modifient des paramètres importants comme les stratégies de mot de passe. 
Il est recommandé de tester ces commandes dans un environnement de test avant de les appliquer en production.



------------------------------------


## 12. Gestion des sites Active Directory

```powershell
Get-ADReplicationSite -Filter *
```
Cette commande liste tous les sites de réplication AD. Aucune modification nécessaire.

```powershell
New-ADReplicationSite -Name "SiteParis"
```
Cette commande crée un nouveau site AD. Remplacez "SiteParis" par le nom du site que vous souhaitez créer.

```powershell
Move-ADDirectoryServer -Identity "DC01" -Site "SiteParis"
```
Cette commande déplace un serveur vers un site. Remplacez :
- "DC01" par le nom du serveur à déplacer
- "SiteParis" par le nom du site de destination

```powershell
New-ADReplicationSubnet -Name "192.168.1.0/24" -Site "SiteParis"
```
Cette commande crée un nouveau sous-réseau et l'associe à un site. Remplacez :
- "192.168.1.0/24" par l'adresse de votre sous-réseau
- "SiteParis" par le nom du site associé

## 13. Cmdlets de gestion Active Directory

```powershell
Enable-ADAccount -Identity "jdoe"
```
Cette commande active un compte AD. Remplacez "jdoe" par le nom du compte à activer.

```powershell
Disable-ADAccount -Identity "jdoe"
```
Cette commande désactive un compte AD. Remplacez "jdoe" par le nom du compte à désactiver.

```powershell
Unlock-ADAccount -Identity "jdoe"
```
Cette commande déverrouille un compte AD. Remplacez "jdoe" par le nom du compte à déverrouiller.

```powershell
Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 | Select-Object Name, LastLogonDate
```
Cette commande recherche les comptes inactifs depuis 90 jours. Ajustez "90.00:00:00" pour modifier la période de recherche.

```powershell
Set-ADAccountPassword -Identity "jdoe" -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NouveauMotDePasse123!" -Force)
```
Cette commande réinitialise le mot de passe d'un compte. Remplacez :
- "jdoe" par le nom du compte
- "NouveauMotDePasse123!" par le nouveau mot de passe

```powershell
Set-ADUser -Identity "jdoe" -PasswordNeverExpires $true
```
Cette commande empêche l'expiration du mot de passe d'un compte. Remplacez "jdoe" par le nom du compte.

## 14. Ateliers pratiques

```powershell
Get-ADUser -Filter * -Properties * | Export-Csv -Path "C:\Users.csv" -NoTypeInformation
```
Cette commande exporte tous les utilisateurs dans un fichier CSV. Modifiez "C:\Users.csv" pour changer le chemin et le nom du fichier d'export.

```powershell
$userExists = Get-ADUser -Filter {SamAccountName -eq "jdoe"} -ErrorAction SilentlyContinue
if ($userExists) { Write-Host "L'utilisateur existe" } else { Write-Host "L'utilisateur n'existe pas" }
```
Ce script vérifie si un compte utilisateur existe. Remplacez "jdoe" par le nom d'utilisateur à vérifier.

```powershell
Import-Csv "C:\NouveauxUtilisateurs.csv" | ForEach-Object {
    New-ADUser -Name $_.Name -SamAccountName $_.SamAccountName -UserPrincipalName $_.UserPrincipalName -Path $_.Path
}
```
Ce script crée des utilisateurs en masse à partir d'un fichier CSV. Assurez-vous que votre fichier CSV contient les colonnes Name, SamAccountName, UserPrincipalName et Path. Modifiez "C:\NouveauxUtilisateurs.csv" pour pointer vers votre fichier CSV.

```powershell
Get-ADUser -Filter * -Properties PasswordLastSet | Select-Object Name, PasswordLastSet
```
Cette commande récupère la date de dernière modification du mot de passe pour tous les utilisateurs.

Ces commandes couvrent une large gamme d'opérations administratives dans Active Directory. N'oubliez pas de toujours vérifier et ajuster les paramètres selon votre environnement spécifique avant d'exécuter ces commandes, surtout dans un environnement de production.


-----------------------


## 15. Conclusion et commandes avancées

### Recherche avancée d'objets AD

```powershell
Get-ADObject -Filter 'ObjectClass -eq "user" -and Name -like "J*"' -SearchBase "OU=Utilisateurs,DC=votredomaine,DC=com"
```
Cette commande recherche tous les utilisateurs dont le nom commence par "J" dans une OU spécifique. Modifiez :
- 'J*' pour changer le critère de recherche
- "OU=Utilisateurs,DC=votredomaine,DC=com" pour cibler une autre OU

### Gestion des groupes imbriqués

```powershell
Get-ADGroup -Identity "Groupe1" | Get-ADGroupMember -Recursive | Where-Object {$_.objectClass -eq "user"} | Select-Object Name
```
Cette commande liste tous les utilisateurs d'un groupe, y compris ceux des sous-groupes. Remplacez "Groupe1" par le nom du groupe que vous voulez examiner.

### Modification en masse des attributs utilisateur

```powershell
Get-ADUser -Filter {Department -eq "IT"} | Set-ADUser -Company "MaSociété"
```
Cette commande définit la société pour tous les utilisateurs du département IT. Modifiez :
- {Department -eq "IT"} pour cibler un autre département
- -Company "MaSociété" pour définir une autre valeur de société

### Création d'un rapport sur les comptes expirés

```powershell
Search-ADAccount -AccountExpired | Select-Object Name, SamAccountName, AccountExpirationDate | Export-Csv -Path "C:\ComptesExpires.csv" -NoTypeInformation
```
Cette commande génère un rapport CSV des comptes expirés. Modifiez "C:\ComptesExpires.csv" pour changer l'emplacement du fichier de sortie.

### Nettoyage des objets ordinateurs inactifs

```powershell
$InactiveDate = (Get-Date).AddDays(-90)
Get-ADComputer -Filter {LastLogonDate -lt $InactiveDate} -Properties LastLogonDate | Remove-ADObject -Recursive -Confirm:$false
```
Ce script supprime les objets ordinateurs inactifs depuis plus de 90 jours. Ajustez le nombre de jours (-90) selon vos besoins. Attention : cette opération est irréversible, utilisez-la avec précaution.

### Audit des modifications de groupe

```powershell
Get-ADGroup -Filter * -Properties * | Where-Object {$_.ModifiedDate -gt (Get-Date).AddDays(-7)} | Select-Object Name, ModifiedDate
```
Cette commande liste les groupes modifiés au cours des 7 derniers jours. Ajustez le nombre de jours (-7) selon vos besoins.

### Gestion des liens GPO

```powershell
Get-GPInheritance -Target "OU=MonOU,DC=votredomaine,DC=com"
```
Cette commande affiche les GPO héritées pour une OU spécifique. Remplacez "OU=MonOU,DC=votredomaine,DC=com" par le chemin de l'OU que vous souhaitez examiner.

Ces exemples supplémentaires couvrent des scénarios plus avancés de gestion Active Directory avec PowerShell. Ils vous permettront d'effectuer des tâches d'administration plus complexes et d'obtenir des informations détaillées sur votre environnement AD.

N'oubliez pas que ces commandes peuvent avoir un impact significatif sur votre environnement AD. Il est toujours recommandé de les tester dans un environnement non-productif avant de les utiliser en production, et de bien comprendre ce que chaque commande fait avant de l'exécuter.
