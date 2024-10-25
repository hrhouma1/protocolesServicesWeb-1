# **Contenu Approfondi : Services d'annuaire et d'authentification**

1. [Introduction aux services d'annuaire et d'authentification](#intro)
2. [Active Directory](#active-directory)
   - [Définition et rôle](#definition-role)
   - [Structure d'un domaine AD](#structure-ad)
   - [Installation de l'AD](#installation-ad)
   - [Configuration de base](#configuration-base)
3. [Modèle de contrôle d'accès et privilèges](#controle-acces)
   - [Concept de privilèges et permissions](#privilèges-permissions)
   - [Modèle de contrôle d'accès basé sur les rôles (RBAC)](#rbac)
   - [Configuration des permissions dans l'AD](#configuration-permissions)
4. [Protocoles d'authentification](#protocoles-auth)
   - [Kerberosv5](#kerberos)
     - [Fonctionnement de Kerberos](#fonctionnement-kerberos)
     - [Avantages et limitations](#avantages-limitations)
   - [NTLM](#ntlm)
     - [Fonctionnement de NTLM](#fonctionnement-ntlm)
     - [Comparaison NTLM et Kerberos](#comparaison)
5. [Étapes de configuration avancée](#config-avancee)
   - [Configuration des politiques de groupe (GPO)](#gpo)
   - [Mise en place d'une authentification multifacteur (MFA)](#mfa)
6. [Conclusion](#conclusion)

<a name="top"></a>













#### <a name="active-directory"></a>2. Active Directory

##### <a name="definition-role"></a>Définition et rôle
**Active Directory (AD)** est un service d'annuaire de Microsoft qui permet la gestion centralisée des identités et des accès dans un réseau. L’AD regroupe les utilisateurs, ordinateurs, et autres objets dans un domaine et permet l'application de politiques de sécurité uniformes.

**Exemple pratique** : 
Pour une entreprise, Active Directory peut regrouper tous les utilisateurs par département dans des groupes spécifiques, leur attribuer des privilèges, et assurer la sécurité du réseau.

<a href="#top">[Retour en haut](#)</a>

##### <a name="structure-ad"></a>Structure d'un domaine AD
Un domaine AD est organisé de manière hiérarchique :
   - **Forêt** : Ensemble de domaines avec une racine commune.
   - **Domaine** : Unité administrative principale, contenant les objets.
   - **OU (Organizational Units)** : Sous-ensembles logiques pour organiser les objets et appliquer les politiques de sécurité.

<a href="#top">[Retour en haut](#)</a>

##### <a name="installation-ad"></a>Installation de l'AD
L'installation d’Active Directory sur un serveur Windows Server suit ces étapes :
1. Ouvrir **Server Manager**.
2. Ajouter un rôle et sélectionner **Active Directory Domain Services (AD DS)**.
3. Promouvoir le serveur en contrôleur de domaine et configurer un nouveau domaine.

**Commandes PowerShell** :
```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "exemple.local"
```

<a href="#top">[Retour en haut](#)</a>

##### <a name="configuration-base"></a>Configuration de base
Une fois AD installé, la configuration de base inclut :
   - **Création des utilisateurs et groupes**.
   - **Définition des politiques de groupe (GPO)** pour la sécurité.
   - **Configuration des droits d'accès**.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="controle-acces"></a>3. Modèle de contrôle d'accès et privilèges

##### <a name="privilèges-permissions"></a>Concept de privilèges et permissions
Les **privilèges** sont les autorisations spécifiques allouées aux utilisateurs pour accéder aux ressources réseau. 
   - **Permissions sur les objets AD** : On peut définir des permissions spécifiques pour des objets dans AD (lecture, écriture, suppression).
   - **Groupes de sécurité** : Utilisés pour gérer les permissions pour des groupes d'utilisateurs.

<a href="#top">[Retour en haut](#)</a>

##### <a name="rbac"></a>Modèle de contrôle d'accès basé sur les rôles (RBAC)
Le modèle **RBAC** permet d'attribuer des permissions en fonction des rôles des utilisateurs dans l’organisation.
   - **Création de groupes de rôles** : Par exemple, les groupes “Admins”, “IT Support”, “Marketing” auront chacun des privilèges spécifiques.

<a href="#top">[Retour en haut](#)</a>

##### <a name="configuration-permissions"></a>Configuration des permissions dans l'AD
Configuration pratique pour attribuer des droits :
   - **Accéder aux propriétés de l'objet**.
   - **Définir les permissions spécifiques** (Lecture, Modification).
   - **Utilisation des GPO** pour définir des règles applicables à des groupes.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="protocoles-auth"></a>4. Protocoles d'authentification

##### <a name="kerberos"></a>Kerberosv5

###### <a name="fonctionnement-kerberos"></a>Fonctionnement de Kerberos
Le protocole **Kerberos** repose sur un système de **tickets** délivrés par un serveur d’authentification, permettant de sécuriser l’accès sans envoyer de mot de passe.
   - **Processus** : L’utilisateur envoie une demande au serveur d’authentification, reçoit un ticket qui est ensuite utilisé pour accéder aux services.

**Schéma de communication** :
1. Demande de **ticket TGT** au KDC (Key Distribution Center).
2. Utilisation du TGT pour obtenir un **ticket de session** pour le service demandé.
   
<a href="#top">[Retour en haut](#)</a>

###### <a name="avantages-limitations"></a>Avantages et limitations
   - **Avantages** : Sécurité accrue, minimise les mots de passe envoyés en clair.
   - **Limitations** : Dépendance à l’horloge synchronisée entre systèmes, complexité d’installation.

<a href="#top">[Retour en haut](#)</a>

##### <a name="ntlm"></a>NTLM

###### <a name="fonctionnement-ntlm"></a>Fonctionnement de NTLM
NTLM (NT LAN Manager) est un protocole d’authentification ancien, basé sur des **hash** de mots de passe.
   - Utilisé comme alternative à Kerberos dans des environnements qui ne supportent pas ce dernier.
   - **Limitations** : Moins sécurisé que Kerberos, vulnérable aux attaques par interception de hash.

<a href="#top">[Retour en haut](#)</a>

###### <a name="comparaison"></a>Comparaison NTLM et Kerberos
   - **Kerberos** est plus sûr et efficace pour les environnements modernes.
   - **NTLM** reste utilisé pour la compatibilité avec les anciens systèmes.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="config-avancee"></a>5. Étapes de configuration avancée

##### <a name="gpo"></a>Configuration des politiques de groupe (GPO)
Les **GPO** permettent de centraliser les règles de sécurité et les configurations réseau.
   - Exemple : Créer une GPO qui oblige le changement de mot de passe tous les 90 jours.

**Commandes PowerShell pour GPO** :
```powershell
New-GPO -Name "Sécurité des mots de passe"
Set-GPOption -Name "Sécurité des mots de passe" -Enforce
```

<a href="#top">[Retour en haut](#)</a>

##### <a name="mfa"></a>Mise en place d'une authentification multifacteur (MFA)
**MFA** ajoute une couche de sécurité supplémentaire, souvent un code unique ou une application d’authentification. Elle est appliquée en complément de l’authentification par mot de passe, particulièrement pour les accès sensibles.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="conclusion"></a>Conclusion
Les services d’annuaire et d’authentification sont essentiels pour la gestion sécurisée des ressources réseau. Grâce à **Active Directory**, **Kerberos**, **NTLM**, et les **GPO**, les administrateurs IT peuvent sécuriser et contrôler efficacement l’accès aux informations de l’organisation.

<a href="#top">[Retour en haut](#)</a>
