# 📁 **Révision #1 : Introduction aux Protocoles de Partage de Fichiers - SMB et NFS**

Les **protocoles de partage de fichiers** sont essentiels dans les réseaux modernes pour faciliter la collaboration et centraliser l'accès aux ressources, en particulier dans les environnements d'entreprise. Ce cours couvre les bases des protocoles SMB et NFS, leurs cas d’utilisation, et les bonnes pratiques de sécurité.

#### Objectifs de ce cours
1. **Comprendre** le rôle des protocoles de partage de fichiers.
2. **Explorer** les protocoles SMB et NFS, leurs différences et leurs versions.
3. **Apprendre** les concepts de base en matière de sécurité et de gestion des accès pour le partage de fichiers.

---

### 1️⃣ **Qu’est-ce qu’un Protocole de Partage de Fichiers ?**

Un **protocole de partage de fichiers** est un ensemble de règles qui permet aux ordinateurs de partager des fichiers sur un réseau. Ces protocoles sont essentiels pour :
- **Centraliser les données** : En stockant les fichiers sur un serveur, on facilite les sauvegardes et la gestion des versions.
- **Optimiser les ressources** : En partageant les fichiers sur un serveur central, on réduit la duplication et on économise de l'espace sur les postes de travail.
- **Améliorer la collaboration** : Les utilisateurs peuvent accéder aux mêmes documents de manière sécurisée et en temps réel.

---

### 2️⃣ **Les Protocoles de Partage de Fichiers Courants : SMB et NFS**

Les deux principaux protocoles utilisés aujourd'hui sont **SMB** (Server Message Block) et **NFS** (Network File System). Voici une présentation de leurs caractéristiques et des environnements où ils sont préférés.

#### **2.1 SMB (Server Message Block)**
- **Développé par Microsoft**, SMB est utilisé principalement dans les environnements Windows.
- **Fonctions principales** : Partage de fichiers, d’imprimantes et autres ressources réseau.
- **Versions** :
  - **SMBv1** : La version d’origine, désormais obsolète en raison de failles de sécurité. Elle est désactivée par défaut dans les versions récentes de Windows.
  - **SMBv2** : Introduite dans Windows Vista, cette version améliore la performance en réduisant le nombre de requêtes réseau.
  - **SMBv3** : Introduite avec Windows 8, SMBv3 intègre le chiffrement des données en transit, offrant une sécurité accrue pour les environnements non sécurisés.

#### **2.2 NFS (Network File System)**
- **Développé par Sun Microsystems**, NFS est principalement utilisé dans les environnements UNIX/Linux.
- **Fonctions principales** : Permet aux utilisateurs de monter un système de fichiers distant et de l’utiliser comme s’il était local.
- **Versions** :
  - **NFSv3** : Version répandue avec prise en charge des verrouillages de fichiers et une meilleure performance.
  - **NFSv4** : Améliore la sécurité et la performance, introduisant l’authentification par Kerberos et le montage unique (NFSv4 Unified Namespace).

---

### 3️⃣ **Configuration et Gestion des Partages de Fichiers avec SMB et NFS**

#### **3.1 Configuration des Partages SMB**

1. **Activer/Désactiver SMB** : Dans Windows, utilisez PowerShell pour gérer les versions de SMB. Par exemple :
   ```powershell
   Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
   ```
2. **Créer un dossier partagé** :
   - Allez dans **Explorateur de fichiers**, cliquez droit sur le dossier à partager, sélectionnez **Propriétés** > **Partage**.
   - Définissez les permissions (lecture, modification, contrôle total) pour chaque utilisateur.
3. **Configurer les ACL (Listes de Contrôle d'Accès)** :
   - Accédez à l’onglet **Sécurité** pour définir des permissions détaillées, comme l'accès en lecture seule ou en lecture/écriture.
   - Si le réseau utilise **Active Directory (AD)**, SMB peut lier les autorisations aux utilisateurs et groupes AD pour simplifier la gestion.

#### **3.2 Configuration des Partages NFS**

1. **Configurer `/etc/exports`** :
   - Les partages NFS sont définis dans le fichier `/etc/exports` sur le serveur NFS.
   - Exemple de configuration pour restreindre l’accès à un réseau spécifique :
     ```plaintext
     /srv/nfs_share 192.168.1.0/24(rw,sync,no_root_squash)
     ```
2. **Monter un partage NFS** :
   - Utilisez la commande `mount` dans l’invite de commande Windows pour monter un partage NFS :
     ```shell
     mount \\<NFS_Server_IP>\nfs_share X:
     ```
3. **Contrôle d'accès basé sur IP** :
   - Contrairement à SMB, NFS utilise des adresses IP pour restreindre l'accès, plutôt que des ACL.
   - Pour une sécurité accrue, il est recommandé de coupler NFSv4 avec Kerberos.

---

### 4️⃣ **Sécurité et Gestion des Accès dans le Partage de Fichiers**

La sécurité est cruciale pour protéger les données partagées contre les accès non autorisés et les interceptions.

#### **4.1 Authentification et Autorisation**

- **SMB** : Dans un environnement Windows, SMB peut être configuré pour utiliser l'authentification Active Directory, ce qui permet de centraliser les droits d'accès par utilisateur et groupe.
- **NFS** : NFSv4 prend en charge l’authentification Kerberos, qui renforce la sécurité dans les réseaux UNIX/Linux.

#### **4.2 Chiffrement des Données en Transit**

- **SMBv3** : SMBv3 intègre le chiffrement des données pour garantir que les fichiers en transit sont protégés contre les interceptions. Activez-le avec PowerShell :
  ```powershell
  Set-SmbServerConfiguration -EncryptData $true
  ```
- **NFS** : Bien que NFS ne chiffre pas les données nativement, IPSec peut être utilisé pour sécuriser les connexions NFS, en particulier pour les versions antérieures à NFSv4.2.

#### **4.3 Quotas de Stockage**

Pour éviter une utilisation excessive des ressources, des quotas de stockage peuvent être définis :
- **SMB** : Windows permet de configurer des quotas par utilisateur via les paramètres de gestion des disques.
- **NFS** : Les quotas sont configurés directement sur le système de fichiers, avec des options comme `usrquota` et `grpquota`.

---

### 5️⃣ **Comparaison des Protocoles : SMB vs NFS**

| Caractéristique            | **SMB**                                   | **NFS**                                 |
|----------------------------|-------------------------------------------|-----------------------------------------|
| **Compatibilité**          | Windows (support Linux avec Samba)        | UNIX/Linux (support Windows avec NFS)   |
| **Sécurité**               | ACL, Active Directory, chiffrement SMBv3  | Contrôle d'accès par IP, Kerberos       |
| **Performance**            | Optimisé pour les environnements Windows  | Meilleure performance pour UNIX/Linux   |
| **Cas d’utilisation**      | Environnements d’entreprise sous Windows  | Systèmes techniques, UNIX/Linux         |

---

### 6️⃣ **Bonnes Pratiques de Sécurité**

1. **Utiliser les dernières versions** : SMBv3 et NFSv4 offrent des fonctionnalités de sécurité plus avancées.
2. **Activer les quotas** pour gérer l’espace disque efficacement.
3. **Implémenter le chiffrement** pour protéger les données en transit.
4. **Surveiller les journaux d’accès** pour détecter toute activité suspecte.
5. **Limiter les accès aux adresses IP spécifiques** pour NFS si Kerberos n’est pas utilisé.

---

### 📌 **Conclusion**

Les protocoles de partage de fichiers comme **SMB** et **NFS** permettent une collaboration centralisée et sécurisée dans les réseaux modernes. Le choix du protocole dépend des besoins en matière de compatibilité, de sécurité et de gestion des accès. En appliquant les bonnes pratiques de configuration et de sécurité, on peut garantir un environnement réseau optimisé pour le partage de fichiers.

---

### 📝 **Révision et Questions**

1. Quel protocole est préféré dans les environnements UNIX/Linux ?
2. Quelle version de SMB intègre le chiffrement de bout en bout ?
3. Comment NFS gère-t-il les contrôles d’accès par défaut ?
4. Quels mécanismes de sécurité peut-on activer pour sécuriser SMB et NFS ?

