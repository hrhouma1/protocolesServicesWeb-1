# Je ne trouve pas AD CS ?

Si vous ne trouvez pas AD CS dans l'interface graphique de Server Manager, voici la procédure complète pour l'installer sur Windows Server 2019 :

## Installation via l'interface graphique

1. Ouvrez Server Manager
2. Cliquez sur **Manage** puis **Add Roles and Features**
3. Dans la liste des rôles, cherchez **Active Directory Certificate Services** (qui peut être sous "Services de certificats Active Directory" en français)

Si vous ne le voyez toujours pas, utilisez PowerShell :

```powershell
# Installation complète avec tous les composants nécessaires
Install-WindowsFeature -Name ADCS-Cert-Authority, ADCS-Web-Enrollment, ADCS-Enroll-Web-Svc -IncludeManagementTools

# Configuration après installation
Install-AdcsCertificationAuthority `
    -CAType EnterpriseRootCA `
    -CACommonName "examenf-CA" `
    -KeyLength 4096 `
    -HashAlgorithm SHA256 `
    -ValidityPeriod Years `
    -ValidityPeriodUnits 5 `
    -Force
```

## Prérequis importants

Avant l'installation, vérifiez que :
- Le serveur est membre du domaine
- L'utilisateur est membre du groupe Enterprise Admins
- Le serveur a une adresse IP statique
- Le nom du serveur est correctement configuré

Si le rôle n'apparaît toujours pas, essayez de :
- Mettre à jour Windows Server
- Vérifier que le Windows Update Service est actif
- Exécuter sfc /scannow pour réparer d'éventuels fichiers système corrompus

