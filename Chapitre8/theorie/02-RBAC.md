# RBAC dans Windows Server et Active Directory

### Table des Matières
1. **Introduction à RBAC dans Windows Server et Active Directory**
   - Définition et Importance
   - RBAC vs autres modèles de sécurité (DAC, MAC)
   
2. **Concepts Clés de RBAC dans Active Directory**
   - Utilisateurs, Groupes et Unités Organisationnelles (OUs)
   - Rôles et Groupes de Sécurité
   - Permissions et Héritage

3. **Configuration de RBAC dans Windows Server**
   - Création et gestion de Groupes de Sécurité dans Active Directory
   - Définir les rôles et permissions
   - Liaison des rôles aux utilisateurs via les groupes de sécurité

4. **Gestion des Droits d’Accès**
   - Utilisation de `Active Directory Users and Computers (ADUC)`
   - Attribuer des permissions pour différents rôles
   - Créer des stratégies de groupe (GPO) pour renforcer les rôles
   
5. **Cas d’Utilisation Communes de RBAC dans Active Directory**
   - Limiter l’accès des utilisateurs finaux
   - Gestion des accès pour les équipes de support et développement
   - Contrôle des accès pour les services d’application

6. **Meilleures Pratiques en RBAC dans Windows Server**
   - Principe de moindre privilège
   - Auditer les permissions et accès avec les outils AD
   - Automatiser la gestion des permissions via PowerShell

# 7. **Exercices Pratiques**
   - Créer des rôles pour l’administration et l’accès en lecture seule
   - Tester les permissions dans un environnement de test
   - Vérification des permissions et tests d’accès

8. **Cas Pratiques**
   - Gérer les accès des développeurs et des administrateurs
   - Définir des rôles pour des applications spécifiques
   - Limiter les accès de maintenance et des utilisateurs finaux

---

### 1. Introduction à RBAC dans Windows Server et Active Directory

**Définition et Importance :**  
Le Role-Based Access Control (RBAC) dans Windows Server et Active Directory permet de gérer les permissions de manière centralisée en fonction des rôles d'un utilisateur dans l'organisation. En assignant des droits spécifiques à des groupes de sécurité dans Active Directory, il est possible de faciliter la gestion des accès pour les différentes fonctions d'un utilisateur.

**Exemple :**
- Un administrateur peut gérer l’ensemble des systèmes.
- Un utilisateur du service support technique a besoin d’un accès limité pour diagnostiquer et résoudre des problèmes.

### 2. Concepts Clés de RBAC dans Active Directory

- **Utilisateurs et Groupes :** Les utilisateurs sont authentifiés par Active Directory et peuvent être regroupés dans des groupes de sécurité.
- **Rôles et Groupes de Sécurité :** Un rôle regroupe des permissions spécifiques pour une fonction. Dans Active Directory, les groupes de sécurité permettent de lier des utilisateurs à ces rôles.
- **Permissions et Héritage :** Les permissions peuvent être héritées par les unités organisationnelles (OU) pour structurer les accès de manière cohérente et hiérarchique.

### 3. Configuration de RBAC dans Windows Server

**Étapes :**
- **Création de Groupes de Sécurité :** Dans ADUC, créez des groupes pour chaque rôle nécessaire (par ex., `Support_Technique`, `Admin_Systeme`, `Utilisateur_ReadOnly`).
- **Définition des Permissions :** Configurez les permissions pour chaque groupe en fonction des besoins opérationnels, en s'assurant que chaque rôle a uniquement les droits nécessaires.
- **Liaison des Rôles aux Utilisateurs :** Attribuez les utilisateurs aux groupes de sécurité correspondant à leurs rôles.

### 4. Gestion des Droits d’Accès

Utilisez `Active Directory Users and Computers (ADUC)` pour gérer les permissions. Les stratégies de groupe (GPO) permettent d’appliquer des règles strictes, notamment pour restreindre l’accès à certains services.

### 5. Cas d’Utilisation Communes de RBAC dans Active Directory

**Exemples :**
- **Support Technique :** Accès aux logs d’événements et aux services de diagnostic.
- **Développement :** Accès restreint aux serveurs de développement mais limité sur les serveurs de production.
- **Utilisateurs finaux :** Accès en lecture seule aux données nécessaires pour leur rôle.

### 6. Meilleures Pratiques en RBAC

**Principe de moindre privilège :** Assurez-vous que les utilisateurs n’ont accès qu’aux ressources nécessaires.
**Audit des permissions :** Utilisez les outils d’Active Directory pour passer en revue les rôles et permissions régulièrement.
**Automatisation via PowerShell :** Automatisez la gestion des groupes et des permissions pour un contrôle d’accès centralisé.

### 7. Exercices Pratiques

- **Exercice :** Créez un groupe `Admin_ReadOnly` et configurez des permissions en lecture seule pour ce groupe sur des serveurs spécifiques.
- **Test :** Connectez-vous avec un utilisateur du groupe `Admin_ReadOnly` et vérifiez les restrictions d'accès.

### 8. Cas Pratiques

- **Gestion des accès :** Définissez des rôles spécifiques pour les administrateurs et les développeurs.
- **Limiter les accès :** Configurez un rôle d’accès limité pour les utilisateurs finaux dans des environnements de production.


------------------
# Correction de l'exercice - RBAC avec PowerShell pour Windows Server et Active Directory
------------------

#### Table des Matières
1. **Introduction et Configuration de PowerShell pour Active Directory**
2. **Création de Groupes de Sécurité pour RBAC**
3. **Ajout d'Utilisateurs aux Groupes de Sécurité**
4. **Définition des Permissions sur les Ressources**
5. **Auditer les Permissions avec PowerShell**
6. **Cas Pratiques et Exemples**

---

### 1. Introduction et Configuration de PowerShell pour Active Directory

Avant de commencer, assurez-vous que PowerShell dispose du module Active Directory installé. Ce module est requis pour gérer les objets AD. Pour les systèmes Windows Server, ce module est souvent inclus lors de l'installation du rôle Active Directory.

```powershell
# Vérifier si le module est installé
Get-Module -ListAvailable -Name ActiveDirectory

# Importer le module si nécessaire
Import-Module ActiveDirectory
```

### 2. Création de Groupes de Sécurité pour RBAC

Les groupes de sécurité représentent des rôles (comme `Support_Technique`, `Admin_Systeme`, `Utilisateur_ReadOnly`). Utilisez PowerShell pour les créer et leur assigner des descriptions.

```powershell
# Créer des groupes de sécurité pour chaque rôle
New-ADGroup -Name "Support_Technique" -SamAccountName "Support_Technique" -GroupScope Global -GroupCategory Security -Description "Groupe pour les techniciens de support"

New-ADGroup -Name "Admin_Systeme" -SamAccountName "Admin_Systeme" -GroupScope Global -GroupCategory Security -Description "Groupe pour les administrateurs système"

New-ADGroup -Name "Utilisateur_ReadOnly" -SamAccountName "Utilisateur_ReadOnly" -GroupScope Global -GroupCategory Security -Description "Groupe pour les utilisateurs en lecture seule"
```

### 3. Ajout d'Utilisateurs aux Groupes de Sécurité

Une fois les groupes créés, ajoutez les utilisateurs à ces groupes en fonction de leurs rôles.

```powershell
# Ajouter un utilisateur existant aux groupes de sécurité
Add-ADGroupMember -Identity "Support_Technique" -Members "jdoe"
Add-ADGroupMember -Identity "Admin_Systeme" -Members "asmith"
Add-ADGroupMember -Identity "Utilisateur_ReadOnly" -Members "kbrown"
```

Pour vérifier les membres d'un groupe de sécurité :

```powershell
# Afficher les membres du groupe
Get-ADGroupMember -Identity "Support_Technique" | Select-Object Name, SamAccountName
```

### 4. Définition des Permissions sur les Ressources

Pour définir les permissions, utilisez PowerShell avec les cmdlets `icacls` (pour les fichiers et dossiers) ou `Set-Acl` pour des configurations avancées. Voici un exemple d’utilisation de `icacls` pour configurer des permissions d’accès en lecture seule pour le groupe `Utilisateur_ReadOnly` sur un dossier partagé.

```powershell
# Définir l'accès en lecture seule sur un dossier pour Utilisateur_ReadOnly
icacls "C:\DossierPartagé" /grant Utilisateur_ReadOnly:(R)
```

Pour définir des permissions spécifiques dans AD (comme la délégation de permissions), utilisez des outils comme `Set-ACL`.

### 5. Auditer les Permissions avec PowerShell

L'audit est essentiel pour s'assurer que les permissions sont correctes et mises à jour régulièrement. Voici comment obtenir les permissions actuelles d'un groupe ou d'un utilisateur pour un dossier spécifique.

```powershell
# Obtenir les permissions pour un dossier spécifique
Get-Acl "C:\DossierPartagé" | Format-List

# Obtenir les membres et permissions des groupes pour vérifier les accès
Get-ADGroupMember -Identity "Admin_Systeme" | ForEach-Object {
    Get-ACL -Path "C:\CheminAcces" -Audit | Format-List
}
```

Pour auditer les utilisateurs et groupes dans un OU, vous pouvez utiliser une requête AD :

```powershell
# Liste des utilisateurs dans une unité organisationnelle spécifique
Get-ADUser -Filter * -SearchBase "OU=Support,DC=entreprise,DC=local" | Select-Object Name, SamAccountName
```

### 6. Cas Pratiques et Exemples

#### Exercice : Créer un Groupe d’Administrateurs Lecture-Seule et Configurer des Permissions

1. Créer un groupe `Admin_ReadOnly`.
2. Définir les permissions en lecture seule pour ce groupe sur le répertoire `C:\AdminOnly`.

```powershell
# Créer le groupe Admin_ReadOnly
New-ADGroup -Name "Admin_ReadOnly" -SamAccountName "Admin_ReadOnly" -GroupScope Global -GroupCategory Security -Description "Accès lecture seule pour les administrateurs"

# Assigner un utilisateur au groupe
Add-ADGroupMember -Identity "Admin_ReadOnly" -Members "jdoe"

# Configurer les permissions en lecture seule
icacls "C:\AdminOnly" /grant Admin_ReadOnly:(R)
```

#### Exercice : Vérifier les Permissions et Accès des Groupes de Sécurité

1. Utiliser PowerShell pour lister les permissions actuelles des dossiers attribués au groupe `Support_Technique`.
   
```powershell
# Auditer les permissions de Support_Technique
Get-ACL -Path "C:\DossierSupport" | Select-Object Path, AccessToString
```

2. Assurer que seuls les membres de `Support_Technique` ont les droits d'écriture sur ce dossier.

```powershell
# Appliquer les permissions en lecture seule à tous les autres utilisateurs
icacls "C:\DossierSupport" /deny *S-1-1-0:(W)
```

