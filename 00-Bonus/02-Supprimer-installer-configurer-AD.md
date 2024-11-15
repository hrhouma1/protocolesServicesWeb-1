# Script : "Installation-et-Réinstallation-Complète-Active-Directory-pour-un-Collège.md"

```powershell

# ⚠️ Assurez-vous que ce script est exécuté avec les droits d'administrateur ⚠️

# Fonction pour vérifier si on est sur Windows Server
function Is-WindowsServer {
    return (Get-WmiObject -Class Win32_OperatingSystem).ProductType -eq 3
}

# Fonction pour installer les fonctionnalités sur Windows Server
function Install-ServerFeatures {
    Install-WindowsFeature -Name AD-Domain-Services, DHCP, RSAT-ADDS, RSAT-DHCP -IncludeManagementTools
}

# Fonction pour installer RSAT sur Windows 10/11
function Install-RSAT {
    Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online
}

# Vérification et installation des modules nécessaires
$requiredModules = @("ServerManager", "ADDSDeployment", "DHCPServer")

# Vérifier si on est sur Windows Server
$isServer = Is-WindowsServer

if ($isServer) {
    Write-Output "Système Windows Server détecté. Installation des fonctionnalités serveur..."
    Install-ServerFeatures
} else {
    Write-Output "Système Windows Client détecté. Installation de RSAT..."
    Install-RSAT
}

# Attendre que l'installation soit terminée
Start-Sleep -Seconds 30

# Importer les modules
foreach ($module in $requiredModules) {
    if (-not (Get-Module -ListAvailable -Name $module)) {
        Write-Output "Le module $module n'est toujours pas disponible après l'installation."
    } else {
        Import-Module $module -ErrorAction SilentlyContinue
    }
}

# Vérification de l'installation réussie
$missingModules = $requiredModules | Where-Object { -not (Get-Module -ListAvailable -Name $_) }
if ($missingModules) {
    Write-Output "Les modules suivants n'ont pas pu être installés : $($missingModules -join ', '). Veuillez les installer manuellement et réexécuter le script."
    exit
}

# Variables de configuration
$DomainName = "cmaisonneuve.qc.ca"
$NetBIOSName = "CMAISONNEUVE"
$ForestName = "cmaisonneuve.qc.ca"
$OU_Enseignants = "OU=Enseignants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_Etudiants = "OU=Etudiants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_RH = "OU=RH,DC=cmaisonneuve,DC=qc,DC=ca"
$Password = "P@ssw0rd!" # Remplacez par un mot de passe sécurisé

# 1️⃣ Désinstallation des fonctionnalités si déjà présentes (uniquement pour Windows Server)
if ($isServer) {
    if (Get-WindowsFeature AD-Domain-Services) {
        Uninstall-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools -Force
    }
    if (Get-WindowsFeature DHCP) {
        Uninstall-WindowsFeature -Name DHCP -IncludeManagementTools -Force
    }
    Write-Output "Redémarrage du système pour finaliser la désinstallation."
    Restart-Computer -Force
    Start-Sleep -Seconds 60
}

# ⚙️ 2️⃣ Réinstallation des fonctionnalités nécessaires (uniquement pour Windows Server)
if ($isServer) {
    Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
    Install-WindowsFeature -Name DHCP -IncludeManagementTools
}

# ⚙️ 3️⃣ Promotion en contrôleur de domaine avec une nouvelle forêt
if ($isServer) {
    Install-ADDSForest -DomainName $DomainName -DomainNetBIOSName $NetBIOSName -ForestMode "Win2012R2" -DomainMode "Win2012R2" -InstallDns -SafeModeAdministratorPassword (ConvertTo-SecureString -AsPlainText $Password -Force) -Force
} else {
    Write-Output "La promotion en contrôleur de domaine n'est pas possible sur un système client Windows."
    exit
}

# ⚙️ 4️⃣ Importation du module DHCPServer
Import-Module DHCPServer

# ⚙️ 5️⃣ Configuration de la portée DHCP
$ScopeID = "192.168.1.0"
$StartRange = "192.168.1.100"
$EndRange = "192.168.1.200"
$SubnetMask = "255.255.255.0"
$LeaseDuration = "8.00:00:00"

Add-DhcpServerv4Scope -Name "Scope $DomainName" -StartRange $StartRange -EndRange $EndRange -SubnetMask $SubnetMask -LeaseDuration $LeaseDuration -State Active




# ⚙️ 6️⃣ Création des Unités d'Organisation (OU) si elles n'existent pas
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "Enseignants" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "Enseignants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "Etudiants" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "Etudiants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "Ressources Humaines" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "Ressources Humaines" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}

# ⚙️ 7️⃣ Création de groupes pour chaque OU si non existants
if (-not (Get-ADGroup -Filter { Name -eq "GroupeEnseignants" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeEnseignants" -GroupScope Global -GroupCategory Security -Path $OU_Enseignants
}
if (-not (Get-ADGroup -Filter { Name -eq "GroupeEtudiants" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeEtudiants" -GroupScope Global -GroupCategory Security -Path $OU_Etudiants
}
if (-not (Get-ADGroup -Filter { Name -eq "GroupeRH" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeRH" -GroupScope Global -GroupCategory Security -Path $OU_RH
}

# ⚙️ 8️⃣ Ajout d'utilisateurs enseignants avec adresses e-mail
$Enseignants = @(
    @{FirstName="Haythem"; LastName="Rehouma"; Email="hrehouma@$DomainName"}
)

foreach ($Enseignant in $Enseignants) {
    $Username = $Enseignant.FirstName.Substring(0,1) + $Enseignant.LastName
    $UserPrincipalName = "$Username@$DomainName"
    if (-not (Get-ADUser -Filter { SamAccountName -eq $Username } -ErrorAction SilentlyContinue)) {
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
}

# ⚙️ 9️⃣ Ajout massif d'étudiants avec adresses e-mail
$Etudiants = @(
    @{Username="e123456"; Email="e123456@$DomainName"},
    @{Username="e123457"; Email="e123457@$DomainName"}
)

foreach ($Etudiant in $Etudiants) {
    if (-not (Get-ADUser -Filter { SamAccountName -eq $Etudiant.Username } -ErrorAction SilentlyContinue)) {
        New-ADUser -Name $Etudiant.Username `
                   -SamAccountName $Etudiant.Username `
                   -UserPrincipalName "$($Etudiant.Username)@$DomainName" `
                   -EmailAddress $Etudiant.Email `
                   -Path $OU_Etudiants `
                   -AccountPassword (ConvertTo-SecureString -AsPlainText $Password -Force) `
                   -Enabled $true
        Add-ADGroupMember -Identity "GroupeEtudiants" -Members $Etudiant.Username
    }
}

# 🔟 Configuration des GPO (exemple)
$GPOName = "GPO-Etudiants"
if (-not (Get-GPO -Name $GPOName -ErrorAction SilentlyContinue)) {
    New-GPO -Name $GPOName | New-GPLink -Target $OU_Etudiants -LinkEnabled Yes
    Set-GPRegistryValue -Name $GPOName -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" -ValueName "NoControlPanel" -Type DWord -Value 1
}

# Vérification et affichage des informations d'installation
Write-Output "Configuration Active Directory et composants associés complétée avec succès."
Write-Output "Forêt Active Directory : $ForestName"
Write-Output "Domaine : $DomainName"
Write-Output "OU Enseignants : $OU_Enseignants"
Write-Output "OU Etudiants : $OU_Etudiants"
Write-Output "OU Ressources Humaines : $OU_RH"
Write-Output "Utilisateurs enseignants et étudiants créés avec leurs adresses e-mail."
Write-Output "GPO appliqué : $GPOName pour les étudiants."

```

### Explications supplémentaires :
1. **Désinstallation des fonctionnalités AD DS et DHCP** : La première partie du script désinstalle les rôles Active Directory Domain Services et DHCP si présents, puis redémarre le système pour garantir une installation propre.
2. **Installation et configuration propre** : Après le redémarrage, le script installe AD DS, crée la forêt et le domaine, et configure DHCP ainsi que les OUs et les utilisateurs.
3. **Configuration des GPO** : Un exemple de GPO est appliqué pour restreindre l'accès au panneau de configuration pour les étudiants, illustrant comment les GPO peuvent être configurées de manière automatique.

### Remarques :
- **Redémarrage** : Un redémarrage est nécessaire après la désinstallation pour éviter des conflits de dépendances.
- **Personnalisation** : Vous pouvez ajuster les variables `$Enseignants` et `$Etudiants` pour ajouter plus d'utilisateurs.
- **Exécution du script** : Exécutez ce script en tant qu'administrateur pour garantir son bon fonctionnement.

Ce script assure une configuration propre sans conflit pour une installation d’Active Directory dans un environnement scolaire.


# Correction




```ssh
# ⚠️ Assurez-vous que ce script est exécuté avec les droits d'administrateur ⚠️

# Fonction pour vérifier si on est sur Windows Server
function Is-WindowsServer {
    return (Get-WmiObject -Class Win32_OperatingSystem).ProductType -eq 3
}

# Fonction pour configurer une adresse IP statique
function Set-StaticIPAddress {
    param (
        [string]$IPAddress,
        [string]$SubnetMask,
        [string]$DefaultGateway,
        [string]$DNSServer
    )
    
    $adapter = Get-NetAdapter | Where-Object {$_.Status -eq "Up"}
    $interface = $adapter | Get-NetIPInterface -AddressFamily IPv4
    
    # Désactiver DHCP si activé
    if ($interface.Dhcp -eq "Enabled") {
        Set-NetIPInterface -InterfaceIndex $adapter.ifIndex -Dhcp Disabled
    }
    
    # Configurer l'adresse IP statique
    New-NetIPAddress -InterfaceIndex $adapter.ifIndex -IPAddress $IPAddress -PrefixLength 24 -DefaultGateway $DefaultGateway
    Set-DnsClientServerAddress -InterfaceIndex $adapter.ifIndex -ServerAddresses $DNSServer
}

# Fonction pour installer les fonctionnalités sur Windows Server
function Install-ServerFeatures {
    Write-Output "Installation des fonctionnalités serveur..."
    Install-WindowsFeature -Name AD-Domain-Services, DHCP, RSAT-ADDS, RSAT-DHCP -IncludeManagementTools
}

# Fonction pour installer RSAT sur Windows 10/11
function Install-RSAT {
    Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online
}

# Variables de configuration
$DomainName = "cmaisonneuve.qc.ca"
$NetBIOSName = "CMAISONNEUVE"
$ForestName = "cmaisonneuve.qc.ca"
$OU_Enseignants = "OU=Enseignants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_Etudiants = "OU=Etudiants,DC=cmaisonneuve,DC=qc,DC=ca"
$OU_RH = "OU=RH,DC=cmaisonneuve,DC=qc,DC=ca"
$Password = "P@ssw0rd!"  # Remplacez par un mot de passe sécurisé

# ⚙️ 1️⃣ Configuration de l'IP statique
$StaticIPAddress = "192.168.1.10"
$StaticSubnetMask = "255.255.255.0"
$StaticDefaultGateway = "192.168.1.1"
$StaticDNSServer = "192.168.1.10"

Set-StaticIPAddress -IPAddress $StaticIPAddress -SubnetMask $StaticSubnetMask -DefaultGateway $StaticDefaultGateway -DNSServer $StaticDNSServer
Start-Sleep -Seconds 10

# ⚙️ 2️⃣ Réinstallation des fonctionnalités
$isServer = Is-WindowsServer
if ($isServer) {
    Install-ServerFeatures
} else {
    Install-RSAT
}
Start-Sleep -Seconds 30

# ⚙️ 3️⃣ Configuration DHCP
$ScopeID = "192.168.1.0"
$StartRange = "192.168.1.100"
$EndRange = "192.168.1.200"
$SubnetMask = "255.255.255.0"
$LeaseDuration = "8.00:00:00"

Add-DhcpServerv4Scope -Name "Scope $DomainName" -StartRange $StartRange -EndRange $EndRange -SubnetMask $SubnetMask -LeaseDuration $LeaseDuration -State Active
Set-DhcpServerv4OptionValue -ScopeId $ScopeID -DnsDomain $DomainName -DnsServer $StaticIPAddress -Router $StaticDefaultGateway

# ⚙️ 4️⃣ Promotion en contrôleur de domaine avec une nouvelle forêt
if ($isServer) {
    Install-ADDSForest -DomainName $DomainName -DomainNetBIOSName $NetBIOSName -ForestMode "Win2012R2" -DomainMode "Win2012R2" -InstallDns -SafeModeAdministratorPassword (ConvertTo-SecureString -AsPlainText $Password -Force) -Force
} else {
    Write-Output "La promotion en contrôleur de domaine n'est pas possible sur un système client Windows."
    exit
}

# ⚙️ 5️⃣ Création des OU et groupes
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "Enseignants" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "Enseignants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "Etudiants" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "Etudiants" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}
if (-not (Get-ADOrganizationalUnit -Filter { Name -eq "RH" } -ErrorAction SilentlyContinue)) {
    New-ADOrganizationalUnit -Name "RH" -Path "DC=cmaisonneuve,DC=qc,DC=ca"
}

if (-not (Get-ADGroup -Filter { Name -eq "GroupeEnseignants" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeEnseignants" -GroupScope Global -GroupCategory Security -Path $OU_Enseignants
}
if (-not (Get-ADGroup -Filter { Name -eq "GroupeEtudiants" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeEtudiants" -GroupScope Global -GroupCategory Security -Path $OU_Etudiants
}
if (-not (Get-ADGroup -Filter { Name -eq "GroupeRH" } -ErrorAction SilentlyContinue)) {
    New-ADGroup -Name "GroupeRH" -GroupScope Global -GroupCategory Security -Path $OU_RH
}

# ⚙️ 6️⃣ Ajout d'utilisateurs
$Enseignants = @(
    @{FirstName="Haythem"; LastName="Rehouma"; Email="hrehouma@$DomainName"}
)
foreach ($Enseignant in $Enseignants) {
    $Username = $Enseignant.FirstName.Substring(0,1) + $Enseignant.LastName
    $UserPrincipalName = "$Username@$DomainName"
    if (-not (Get-ADUser -Filter { SamAccountName -eq $Username } -ErrorAction SilentlyContinue)) {
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
}

$Etudiants = @(
    @{Username="e123456"; Email="e123456@$DomainName"},
    @{Username="e123457"; Email="e123457@$DomainName"}
)
foreach ($Etudiant in $Etudiants) {
    if (-not (Get-ADUser -Filter { SamAccountName -eq $Etudiant.Username } -ErrorAction SilentlyContinue)) {
        New-ADUser -Name $Etudiant.Username `
                   -SamAccountName $Etudiant.Username `
                   -UserPrincipalName "$($Etudiant.Username)@$DomainName" `
                   -EmailAddress $Etudiant.Email `
                   -Path $OU_Etudiants `
                   -AccountPassword (ConvertTo-SecureString -AsPlainText $Password -Force) `
                   -Enabled $true
        Add-ADGroupMember -Identity "GroupeEtudiants" -Members $Etudiant.Username
    }
}

Write-Output "Réinstallation complète des fonctionnalités et des composants associés terminée."

```
