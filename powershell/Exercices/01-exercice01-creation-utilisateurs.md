### **Exercice 1 : Configuration d'Active Directory avec PowerShell**

#### Objectif
Configurer Active Directory en suivant un ordre chronologique : installation des rôles nécessaires, création d'un contrôleur de domaine (DC), puis gestion des unités d'organisation (OU), des utilisateurs et des groupes.

#### Pré-requis
- Serveur Windows avec PowerShell en tant qu'administrateur.
- Active Directory n'est pas encore configuré.

---

#### **Étape 1 : Importation du module Active Directory**

1. Ouvrez PowerShell en mode administrateur.
2. Chargez le module Active Directory pour utiliser les cmdlets AD.

   ```powershell
   Import-Module ActiveDirectory
   ```
   > **Note** : Si cette commande échoue, assurez-vous que le serveur a le rôle ADDS (Active Directory Domain Services) installé.

---

#### **Étape 2 : Installation et configuration d'Active Directory**

1. Installez les services de domaine Active Directory sur le serveur.

   ```powershell
   Install-WindowsFeature AD-Domain-Services
   ```

2. Configurez le contrôleur de domaine en créant une nouvelle forêt et un nouveau domaine. Remplacez `votredomaine` et `com` par le nom de votre domaine.

   ```powershell
   Install-ADDSForest -DomainName "votredomaine.com"
   ```

3. Une fois la configuration terminée, le serveur redémarrera.

---

#### **Étape 3 : Vérification de la configuration AD**

1. Vérifiez que le contrôleur de domaine a été correctement installé en exécutant :

   ```powershell
   Get-ADForest
   Get-ADDomain
   ```

---

#### **Étape 4 : Création d'une Unité d'Organisation (OU)**

1. Créez une nouvelle Unité d'Organisation nommée "Département IT".

   ```powershell
   New-ADOrganizationalUnit -Name "Département IT" -Path "DC=votredomaine,DC=com"
   ```

---

#### **Étape 5 : Création d'un utilisateur dans l'OU "Département IT"**

1. Créez un utilisateur nommé "Alice Dupont" dans l'OU "Département IT".

   ```powershell
   New-ADUser -Name "Alice Dupont" -SamAccountName "adupont" -UserPrincipalName "adupont@votredomaine.com" -Path "OU=Département IT,DC=votredomaine,DC=com" -AccountPassword (ConvertTo-SecureString "MotDePasse123!" -AsPlainText -Force) -Enabled $true
   ```

---

#### **Étape 6 : Création d'un groupe "Groupe IT" dans l'OU "Département IT"**

1. Créez un groupe nommé "Groupe IT" dans l'OU "Département IT".

   ```powershell
   New-ADGroup -Name "Groupe IT" -GroupScope Global -Path "OU=Département IT,DC=votredomaine,DC=com"
   ```

---

#### **Étape 7 : Ajout de l'utilisateur "Alice Dupont" au "Groupe IT"**

1. Ajoutez "Alice Dupont" au groupe "Groupe IT".

   ```powershell
   Add-ADGroupMember -Identity "Groupe IT" -Members "adupont"
   ```

---

### **Conclusion**

Cet exercice vous a permis de configurer un environnement Active Directory de manière complète et cohérente, en partant de l'importation du module AD, jusqu'à la création d'une OU, d'un utilisateur et d'un groupe. Vous avez ainsi suivi l'ordre logique et chronologique de l'installation d'Active Directory.
