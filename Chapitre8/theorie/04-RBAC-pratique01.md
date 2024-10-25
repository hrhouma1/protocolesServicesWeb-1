# 🛠️ Correction de l'exercice - RBAC avec PowerShell pour Windows Server et Active Directory

---

### 📚 Table des Matières
1. [**Introduction et Configuration de PowerShell pour Active Directory**](#1-introduction-et-configuration-de-powershell-pour-active-directory)
2. [**Création de Groupes de Sécurité pour RBAC**](#2-création-de-groupes-de-sécurité-pour-rbac)
3. [**Ajout d'Utilisateurs aux Groupes de Sécurité**](#3-ajout-dutilisateurs-aux-groupes-de-sécurité)
4. [**Définition des Permissions sur les Ressources**](#4-définition-des-permissions-sur-les-ressources)
5. [**Auditer les Permissions avec PowerShell**](#5-auditer-les-permissions-avec-powershell)
6. [**Cas Pratiques et Exemples**](#6-cas-pratiques-et-exemples)

---

### 1. Introduction et Configuration de PowerShell pour Active Directory
[🔝 Retour à la Table des Matières](#-table-des-matières)

Avant de commencer à configurer les accès avec RBAC (Role-Based Access Control), vérifiez que PowerShell est correctement configuré pour interagir avec Active Directory. 

#### 🔹 Étape 1 : Vérifier si le Module Active Directory est installé
Le module **Active Directory** est nécessaire pour gérer les objets AD avec PowerShell. Si vous ne l'avez pas installé, installez-le d'abord.

```powershell
# Vérifier si le module est déjà disponible
Get-Module -ListAvailable -Name ActiveDirectory
```

#### 🔹 Étape 2 : Importer le Module Active Directory
S'il n'est pas automatiquement chargé, importez le module pour commencer.

```powershell
# Importer le module Active Directory
Import-Module ActiveDirectory
```

---

### 2. Création de Groupes de Sécurité pour RBAC
[🔝 Retour à la Table des Matières](#-table-des-matières)

Les **groupes de sécurité** permettent de regrouper des utilisateurs selon leurs rôles. En leur assignant des permissions précises, vous pouvez facilement gérer l'accès aux ressources dans Active Directory.

#### 🔹 Créer des Groupes de Sécurité
Utilisez `New-ADGroup` pour créer des groupes avec des descriptions claires.

```powershell
# Exemple de création de groupes pour différents rôles
New-ADGroup -Name "Support_Technique" -SamAccountName "Support_Technique" -GroupScope Global -GroupCategory Security -Description "Groupe pour les techniciens de support"
New-ADGroup -Name "Admin_Systeme" -SamAccountName "Admin_Systeme" -GroupScope Global -GroupCategory Security -Description "Groupe pour les administrateurs système"
New-ADGroup -Name "Utilisateur_ReadOnly" -SamAccountName "Utilisateur_ReadOnly" -GroupScope Global -GroupCategory Security -Description "Groupe pour les utilisateurs en lecture seule"
```

---

### 3. Ajout d'Utilisateurs aux Groupes de Sécurité
[🔝 Retour à la Table des Matières](#-table-des-matières)

Après avoir créé les groupes, ajoutez-y des utilisateurs selon leurs rôles. Cette étape est essentielle pour appliquer les bonnes permissions.

#### 🔹 Ajouter des Utilisateurs aux Groupes
La commande `Add-ADGroupMember` permet d'ajouter facilement des utilisateurs à un groupe de sécurité.

```powershell
# Ajout d'un utilisateur aux groupes de sécurité
Add-ADGroupMember -Identity "Support_Technique" -Members "jdoe"
Add-ADGroupMember -Identity "Admin_Systeme" -Members "asmith"
Add-ADGroupMember -Identity "Utilisateur_ReadOnly" -Members "kbrown"
```

#### 🔹 Vérifier les Membres d'un Groupe
Pour afficher les membres d’un groupe, utilisez `Get-ADGroupMember`.

```powershell
# Afficher tous les membres du groupe Support_Technique
Get-ADGroupMember -Identity "Support_Technique" | Select-Object Name, SamAccountName
```

---

### 4. Définition des Permissions sur les Ressources
[🔝 Retour à la Table des Matières](#-table-des-matières)

Les permissions sur les ressources, comme les dossiers et les fichiers, peuvent être définies pour chaque groupe afin de restreindre l'accès en fonction des besoins.

#### 🔹 Accorder des Permissions de Lecture avec `icacls`
Pour attribuer des permissions, la commande `icacls` permet de gérer les accès aux fichiers et dossiers.

```powershell
# Donner un accès en lecture seule à Utilisateur_ReadOnly pour le dossier C:\DossierPartagé
icacls "C:\DossierPartagé" /grant Utilisateur_ReadOnly:(R)
```

#### 🔹 Définir des Permissions Avancées avec `Set-Acl`
Pour des permissions spécifiques dans AD, `Set-Acl` permet d’effectuer des configurations avancées.

---

### 5. Auditer les Permissions avec PowerShell
[🔝 Retour à la Table des Matières](#-table-des-matières)

L’audit des permissions permet de vérifier que chaque groupe a les bons droits. Cela assure une bonne sécurité et permet de corriger rapidement les erreurs.

#### 🔹 Obtenir les Permissions pour un Dossier
Avec `Get-Acl`, récupérez les permissions d’un dossier pour un groupe ou un utilisateur spécifique.

```powershell
# Afficher les permissions actuelles d'un dossier
Get-Acl "C:\DossierPartagé" | Format-List
```

#### 🔹 Auditer les Groupes et les Accès
Vous pouvez vérifier les permissions pour chaque membre d'un groupe afin de s'assurer que seuls les utilisateurs autorisés ont accès.

```powershell
# Vérifier les accès des membres du groupe Admin_Systeme
Get-ADGroupMember -Identity "Admin_Systeme" | ForEach-Object {
    Get-Acl -Path "C:\CheminAcces" -Audit | Format-List
}
```

#### 🔹 Lister les Utilisateurs d’une Unité Organisationnelle (OU)
Pour obtenir tous les utilisateurs dans une unité organisationnelle spécifique, utilisez `Get-ADUser`.

```powershell
# Liste des utilisateurs dans une unité organisationnelle
Get-ADUser -Filter * -SearchBase "OU=Support,DC=entreprise,DC=local" | Select-Object Name, SamAccountName
```

---

### 6. Cas Pratiques et Exemples
[🔝 Retour à la Table des Matières](#-table-des-matières)

Ces cas pratiques vous permettront d’appliquer et de tester les concepts de gestion des accès.

#### 🔹 Exercice : Créer un Groupe d’Administrateurs Lecture-Seule et Configurer des Permissions
1. **Objectif** : Créer un groupe `Admin_ReadOnly` pour les administrateurs avec accès en lecture seule.
2. **Étapes** :
   - Créer le groupe `Admin_ReadOnly`.
   - Configurer des permissions en lecture seule sur un dossier spécifique.

```powershell
# Créer le groupe Admin_ReadOnly
New-ADGroup -Name "Admin_ReadOnly" -SamAccountName "Admin_ReadOnly" -GroupScope Global -GroupCategory Security -Description "Accès lecture seule pour les administrateurs"

# Ajouter un utilisateur au groupe
Add-ADGroupMember -Identity "Admin_ReadOnly" -Members "jdoe"

# Configurer les permissions de lecture seule sur C:\AdminOnly
icacls "C:\AdminOnly" /grant Admin_ReadOnly:(R)
```

#### 🔹 Exercice : Vérifier les Permissions et Accès des Groupes de Sécurité
Utilisez les commandes d’audit pour vous assurer que seules les personnes autorisées peuvent accéder aux ressources.

1. **Vérifier les Permissions d'un Groupe**
   
   ```powershell
   # Obtenir les permissions actuelles pour le dossier de Support_Technique
   Get-Acl -Path "C:\DossierSupport" | Select-Object Path, AccessToString
   ```

2. **Restreindre les Permissions pour les Autres Utilisateurs**

   ```powershell
   # Appliquer des restrictions d'écriture pour tous les autres utilisateurs
   icacls "C:\DossierSupport" /deny *S-1-1-0:(W)
   ```
