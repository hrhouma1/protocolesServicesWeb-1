
------------
# 1. Configuration de l'infrastructure PKI
------------

Pour configurer la PKI sur SRV1 (contrôleur de domaine), exécutez les commandes PowerShell suivantes:

```powershell
# Installation du rôle AD CS
Install-WindowsFeature -Name AD-Certificate -IncludeManagementTools

# Configuration de l'autorité de certification
Install-AdcsCertificationAuthority -CAType EnterpriseRootCA -CACommonName "examenf-CA" -Force
```

------------
# 2. Configuration des enregistrements DNS
------------

Pour ajouter les enregistrements DNS pour ftp1 et www1, utilisez ces commandes PowerShell:

```powershell
# Ajout des enregistrements A
Add-DnsServerResourceRecordA -Name "ftp1" -ZoneName "examenf.local" -IPv4Address "192.168.10.3"
Add-DnsServerResourceRecordA -Name "www1" -ZoneName "examenf.local" -IPv4Address "192.168.10.3"
```

------------
# 3. Installation et Configuration IIS
------------

```powershell
# Installation d'IIS avec les outils de gestion
Install-WindowsFeature -Name Web-Server -IncludeManagementTools

# Création des répertoires virtuels
New-Item -Path "C:\inetpub\wwwroot\Documents" -ItemType Directory
New-Item -Path "C:\inetpub\wwwroot\Ressources" -ItemType Directory

# Demande du certificat depuis la PKI
$cert = Get-Certificate -Template WebServer -DnsName "www1.examenf.local" -CertStoreLocation "cert:\LocalMachine\My"

# Configuration du site web avec HTTP et HTTPS
New-WebBinding -Name "Default Web Site" -Protocol "https" -Port 443 -HostHeader "www1.examenf.local" -SslFlags 1
$binding = Get-WebBinding -Name "Default Web Site" -Protocol "https"
$binding.AddSslCertificate($cert.Certificate.Thumbprint, "My")
```

------------
# 4. Création des utilisateurs AD
------------

```powershell
# Création des utilisateurs
New-ADUser -Name "user1" -SamAccountName "user1" -UserPrincipalName "user1@examenf.local" -Enabled $true -AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force)
New-ADUser -Name "user2" -SamAccountName "user2" -UserPrincipalName "user2@examenf.local" -Enabled $true -AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force)
```

------------
# 5. Configuration du serveur FTP
------------

```powershell
# Installation du service FTP
Install-WindowsFeature Web-Ftp-Server -IncludeManagementTools

# Création des répertoires FTP
New-Item -Path "C:\inetpub\ftproot\anonymous" -ItemType Directory
New-Item -Path "C:\inetpub\ftproot\user1" -ItemType Directory
New-Item -Path "C:\inetpub\ftproot\user2" -ItemType Directory

# Configuration du site FTP avec isolation utilisateur
New-WebFtpSite -Name "FTP Site" -Port 21 -PhysicalPath "C:\inetpub\ftproot" -Force

# Configuration de l'authentification
Set-WebConfigurationProperty -Filter "/system.ftpServer/security/authentication/basicAuthentication" -Name "enabled" -Value "true"
Set-WebConfigurationProperty -Filter "/system.ftpServer/security/authentication/anonymousAuthentication" -Name "enabled" -Value "true"

# Configuration de l'isolation utilisateur
Set-WebConfigurationProperty -Filter "/system.applicationHost/sites/site[@name='FTP Site']/ftpServer/userIsolation" -Name "mode" -Value "IsolateUsingActiveDirectory"
```

**Points importants à vérifier:**
- Vérifiez l'accès HTTPS à https://www1.examenf.local
- Testez l'accès FTP anonyme au dossier anonymous
- Vérifiez que user1 et user2 peuvent accéder uniquement à leurs dossiers respectifs
- Assurez-vous que les permissions NTFS sont correctement configurées sur les dossiers

**Structure des répertoires attendue:**
```
C:\inetpub\
├── wwwroot\
│   ├── Documents\
│   └── Ressources\
└── ftproot\
    ├── anonymous\
    ├── user1\
    └── user2\
```

------------
# Annexe 01 : Installation des prérequis
------------

==> commandes PowerShell plus complètes pour installer et configurer l'AD CS sur un serveur Windows Server 2019 vierge :

## Installation des prérequis

```powershell
# Installation du rôle AD CS avec tous les composants nécessaires
Install-WindowsFeature -Name AD-Certificate, ADCS-Cert-Authority, ADCS-Web-Enrollment, ADCS-Enroll-Web-Pol, ADCS-Enroll-Web-Svc -IncludeManagementTools

# Création du fichier CAPolicy.inf pour configurer les paramètres cryptographiques
$CAPolicyContent = @"
[Version]
Signature="$Windows NT$"
[Certsrv_Server]
RenewalKeyLength=4096
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=10
LoadDefaultTemplates=0
AlternateSignatureAlgorithm=1
[CRLDistributionPoint]
[AuthorityInformationAccess]
"@

Set-Content -Path C:\Windows\CAPolicy.inf -Value $CAPolicyContent

# Configuration de l'autorité de certification
Install-AdcsCertificationAuthority `
    -CAType EnterpriseRootCA `
    -CACommonName "examenf-CA" `
    -KeyLength 4096 `
    -HashAlgorithm SHA256 `
    -CryptoProviderName "RSA#Microsoft Software Key Storage Provider" `
    -ValidityPeriod Years `
    -ValidityPeriodUnits 10 `
    -Force
```

## Vérification et configuration supplémentaire

```powershell
# Vérification de l'installation
Get-Service -Name CertSvc
certutil -ping

# Redémarrage du service Certificate Authority
Restart-Service -Name CertSvc

# Publication du CRL (Certificate Revocation List)
certutil -crl
```

**Points importants:**
- Le paramètre KeyLength est fixé à 4096 bits pour une meilleure sécurité
- L'algorithme de hachage utilisé est SHA256
- La validité du certificat racine est de 10 ans
- Le provider cryptographique RSA est explicitement spécifié
- Le fichier CAPolicy.inf est créé pour définir les paramètres de base de la PKI

Si vous rencontrez des erreurs, vérifiez que :
- Le serveur est bien membre du domaine
- L'utilisateur a les droits Enterprise Admin
- Le nom du serveur et l'adresse IP sont correctement configurés

------------
# Annexe 02 - C'est quoi Active Directory Certificate Services ?
------------

Active Directory Certificate Services (AD CS) est un rôle de serveur Windows qui permet de mettre en place une infrastructure à clé publique (PKI) au sein d'une organisation. 

## Fonctionnalités principales

AD CS permet de :
- Émettre et gérer des certificats numériques pour les utilisateurs, ordinateurs et services du réseau[3]
- Sécuriser les communications via le chiffrement
- Vérifier l'identité des utilisateurs et des appareils
- Faciliter l'échange sécurisé de données

## Applications supportées

AD CS prend en charge de nombreuses applications professionnelles[5], notamment :
- La messagerie sécurisée (S/MIME)
- Les connexions VPN 
- Le protocole RDP
- SSL/TLS
- IPsec

## Avantages clés

**Sécurité renforcée**
- Chiffrement des données pour la confidentialité
- Authentification par certificats
- Signatures numériques pour l'intégrité[3]

**Gestion simplifiée**
- Administration centralisée des certificats
- Automatisation des processus d'émission et de renouvellement
- Intégration avec l'infrastructure Active Directory existante

AD CS constitue ainsi une solution complète pour la gestion des certificats numériques dans un environnement Windows Server, permettant aux organisations de renforcer significativement leur sécurité informatique.



