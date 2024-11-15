# Description

Je vous propose un script PowerShell complet pour installer Active Directory, créer une forêt, ajouter un domaine, configurer DHCP, créer des utilisateurs avec des adresses e-mail spécifiques, 
et organiser les unités d’organisation (OU) de manière cohérente avec ce que font les administrateurs d’un collège. 
Ce script est structuré étape par étape, avec des commentaires pour chaque section.

*Assurez-vous d'exécuter ce script avec les privilèges d'administrateur et que votre environnement Windows est configuré pour prendre en charge les rôles AD DS et DHCP.*

```powershell
# ⚠️ Assurez-vous que ce script est exécuté avec les droits d'administrateur ⚠️

# Variables de configuration
$DomainName = "cmaisonneuve.qc.ca"
$NetBIOSName = "CMAISONNEUVE"
$ForestName = "cmaisonneuve.qc.ca"
$OU_Enseignants = "OU=Enseignants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_Etudiants = "OU=Etudiants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_RH = "OU=RH,DC=cmaisonneuve,DC=qc,DC=ca"
$Password = "P@ssw0rd!" # Remplacez par un mot de passe sécurisé

# 1️⃣ Installation des fonctionnalités nécessaires
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-WindowsFeature -Name DHCP -IncludeManagementTools

# 2️⃣ Promotion du serveur en contrôleur de domaine avec une nouvelle forêt
Install-ADDSForest -DomainName $DomainName -DomainNetBIOSName $NetBIOSName -ForestMode "WinThreshold" -DomainMode "WinThreshold" -InstallDns -SafeModeAdministratorPassword (ConvertTo-SecureString -AsPlainText $Password -Force) -Force

# 3️⃣ Configuration du DHCP pour le domaine
Import-Module DHCPServer

# Configuration de la portée DHCP (adapter les adresses IP selon votre réseau)
$ScopeID = "192.168.1.0"
$StartRange = "192.168.1.100"
$EndRange = "192.168.1.200"
$SubnetMask = "255.255.255.0"
$LeaseDuration = "8.00:00:00"

Add-DhcpServerv4Scope -Name "Scope $DomainName" -StartRange $StartRange -EndRange $EndRange -SubnetMask $SubnetMask -LeaseDuration $LeaseDuration -State Active

# 4️⃣ Création des Unités d'Organisation (OU)
New-ADOrganizationalUnit -Name "Enseignants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
New-ADOrganizationalUnit -Name "Etudiants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
New-ADOrganizationalUnit -Name "Ressources Humaines" -Path "DC=cmaisonneuve,DC=qc,DC=ca"

# 5️⃣ Création de groupes pour chaque OU
New-ADGroup -Name "GroupeEnseignants" -GroupScope Global -GroupCategory Security -Path $OU_Enseignants
New-ADGroup -Name "GroupeEtudiants" -GroupScope Global -GroupCategory Security -Path $OU_Etudiants
New-ADGroup -Name "GroupeRH" -GroupScope Global -GroupCategory Security -Path $OU_RH

# 6️⃣ Ajout d'utilisateurs enseignants avec adresses e-mail
$Enseignants = @(
    @{FirstName="Haythem"; LastName="Rehouma"; Email="hrehouma@$DomainName"}
    # Ajoutez d'autres enseignants ici
)

foreach ($Enseignant in $Enseignants) {
    $Username = $Enseignant.FirstName.Substring(0,1) + $Enseignant.LastName
    $UserPrincipalName = "$Username@$DomainName"
    New-ADUser -Name "$($Enseignant.FirstName) $($Enseignant.LastName)" `
               -GivenName $Enseignant.FirstName `
               -Surname $Enseignant.LastName `
               -UserPrincipalName $UserPrincipalName `
               -SamAccountName $Username `
               -EmailAddress $Enseignant.Email `
               -Path $OU_Enseignants `
               -AccountPassword (ConvertTo-SecureString -AsPlainText $Password -Force) `
               -Enabled $true
    Add-ADGroupMember -Identity "GroupeEnseignants" -Members $Username
}

# 7️⃣ Ajout massif d'étudiants avec adresses e-mail
$Etudiants = @(
    @{Username="e123456"; Email="e123456@$DomainName"}
    @{Username="e123457"; Email="e123457@$DomainName"}
    # Ajoutez d'autres étudiants ici
)

foreach ($Etudiant in $Etudiants) {
    New-ADUser -Name $Etudiant.Username `
               -SamAccountName $Etudiant.Username `
               -UserPrincipalName "$($Etudiant.Username)@$DomainName" `
               -EmailAddress $Etudiant.Email `
               -Path $OU_Etudiants `
               -AccountPassword (ConvertTo-SecureString -AsPlainText $Password -Force) `
               -Enabled $true
    Add-ADGroupMember -Identity "GroupeEtudiants" -Members $Etudiant.Username
}

# 8️⃣ Configuration des GPO (exemple)
# Exemple de GPO : interdire l'accès au Panneau de configuration pour les étudiants
$GPOName = "GPO-Etudiants"
New-GPO -Name $GPOName | New-GPLink -Target $OU_Etudiants -LinkEnabled Yes
Set-GPRegistryValue -Name $GPOName -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" -ValueName "NoControlPanel" -Type DWord -Value 1

# 🔄 Vérification et affichage des informations d'installation
Write-Output "Active Directory et les composants associés ont été installés et configurés avec succès."
Write-Output "Forêt Active Directory : $ForestName"
Write-Output "Domaine : $DomainName"
Write-Output "OU Enseignants : $OU_Enseignants"
Write-Output "OU Etudiants : $OU_Etudiants"
Write-Output "OU Ressources Humaines : $OU_RH"
Write-Output "Les utilisateurs enseignants et étudiants ont été créés avec leurs adresses e-mail."
Write-Output "GPO appliqué : $GPOName pour les étudiants."
```

### Explications des principales étapes :
1. **Installation des fonctionnalités** : Installe les services AD DS et DHCP.
2. **Promotion en contrôleur de domaine** : Crée une nouvelle forêt et configure le domaine.
3. **Configuration DHCP** : Définit une portée DHCP pour distribuer des adresses IP.
4. **Création des OUs** : Organise les utilisateurs dans des OUs (Enseignants, Étudiants, RH).
5. **Création de groupes** : Crée des groupes de sécurité pour chaque OU.
6. **Ajout des utilisateurs enseignants** : Crée des comptes pour les enseignants avec adresses e-mail personnalisées.
7. **Ajout des utilisateurs étudiants** : Crée des comptes pour les étudiants avec des adresses e-mail de type matricule.
8. **Configuration des GPO** : Exemple de GPO pour restreindre l'accès au Panneau de configuration pour les étudiants.

### Notes :
- **Mot de passe** : Assurez-vous d'utiliser un mot de passe sécurisé dans un environnement de production.
- **Modifications à apporter** : Vous pouvez ajouter davantage d’utilisateurs en remplissant les tableaux `$Enseignants` et `$Etudiants`.
- **GPO personnalisées** : Vous pouvez adapter les GPO selon les besoins de votre organisation.

Ce script couvre les principales tâches administratives qu’un administrateur de collège effectue de façon régulière pour la gestion d’un Active Directory.
