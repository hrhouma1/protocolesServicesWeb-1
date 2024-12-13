# *Résumé et rappel des concepts : Infrastructure PKI, IIS, DNS, FTP, et Active Directory*

---

# **1. Qu'est-ce qu'une infrastructure PKI ?**

PKI (Public Key Infrastructure) est un **système qui gère les certificats numériques et les clés cryptographiques**. Ces certificats sont utilisés pour sécuriser les communications (comme HTTPS), authentifier les utilisateurs et chiffrer les données.

#### **Éléments principaux d'une PKI :**
- **Certificats numériques** : Des fichiers électroniques qui prouvent l’identité d’un serveur ou d’une personne. Ils sont comme des "cartes d’identité" numériques.
- **Clé publique et clé privée** :
  - La **clé publique** est partagée avec tout le monde.
  - La **clé privée** est gardée secrète et utilisée pour déchiffrer ou signer les données.
- **CA (Certificate Authority)** :
  - Une **autorité de certification** qui délivre des certificats numériques.
  - C’est une entité de confiance qui garantit que le certificat appartient bien au bon serveur ou à la bonne personne.

#### **Pourquoi une PKI est importante ?**
- **Authentification** : Vérifie que vous communiquez avec le bon serveur (par exemple, un site HTTPS).
- **Confidentialité** : Chiffre les données échangées pour qu'elles ne puissent pas être lues par d'autres.
- **Intégrité** : Assure que les données n’ont pas été modifiées pendant le transfert.

---

### **2. Qu'est-ce qu'une CA (Certificate Authority) ?**

Une **CA (autorité de certification)** est une partie centrale de la PKI. Elle délivre, vérifie et gère les certificats numériques. 

#### **Rôles d’une CA :**
1. **Délivrer des certificats** : Par exemple, un certificat pour sécuriser un site web en HTTPS.
2. **Vérifier les identités** : La CA vérifie que l'entité qui demande un certificat est légitime.
3. **Révoquer des certificats** : Si un certificat est compromis, la CA peut le rendre invalide.

#### **Dans un réseau d’entreprise :**
- La CA peut être installée sur un serveur comme **SRV1**.
- Elle permet aux utilisateurs et aux serveurs internes de demander des certificats pour sécuriser les communications.

---

### **3. Qu'est-ce que DNS (Domain Name System) ?**

DNS est le **répertoire téléphonique d’Internet**. Il convertit des noms de domaine faciles à lire (comme www.example.com) en adresses IP (comme 192.168.1.1).

#### **Rôle dans le réseau :**
- **Résolution de noms** : DNS permet aux utilisateurs de se connecter à un serveur en utilisant un nom lisible comme **ftp1.examenf.local** au lieu d'une adresse IP.
- **Enregistrements DNS** :
  - **A Record** : Associe un nom (ftp1.examenf.local) à une IP (192.168.10.3).
  - **CNAME Record** : Alias d’un autre nom.
  
#### **Pourquoi DNS est utile ?**
- Simplifie la gestion : Pas besoin de mémoriser des adresses IP.
- Facilite les connexions entre les services internes au réseau.

---

### **4. Qu'est-ce qu'IIS (Internet Information Services) ?**

IIS est un **serveur web** développé par Microsoft. Il permet d’héberger des sites web et des services comme HTTP, HTTPS, et FTP.

#### **Fonctions d’IIS :**
1. **Héberger des sites web** : Par exemple, le site **www1.examenf.local**.
2. **Prendre en charge HTTPS** :
   - Avec un certificat fourni par la PKI (sécurisation des sites web).
3. **Configurer des répertoires virtuels** :
   - Permet de diviser un site en différentes sections (comme **Documents** et **Ressources**).

#### **Pourquoi IIS est important dans cet examen ?**
- Pour configurer un site web interne qui utilise HTTP et HTTPS.
- Pour sécuriser les communications grâce à un certificat.

---

### **5. Qu'est-ce qu'un site FTP (File Transfer Protocol) ?**

FTP est un protocole utilisé pour **transférer des fichiers** entre un client et un serveur.

#### **Fonctions principales d’un site FTP :**
1. **Partage de fichiers** : Les utilisateurs peuvent envoyer et télécharger des fichiers.
2. **Isolation des utilisateurs** :
   - Chaque utilisateur a un dossier dédié qu’il peut voir.
   - Les autres utilisateurs ne peuvent pas accéder à ce dossier.

#### **Types d’accès FTP :**
- **Accès anonyme** : Toute personne peut se connecter sans identifiant.
- **Accès authentifié** : Requiert un nom d'utilisateur et un mot de passe.

#### **Pourquoi FTP est important dans cet examen ?**
- Pour permettre le partage sécurisé de fichiers.
- Pour tester l’isolation entre les utilisateurs (user1 et user2).

---

### **6. Qu'est-ce qu'Active Directory (AD) ?**

Active Directory est un **service de gestion des identités et des accès**. Il permet de centraliser la gestion des utilisateurs, des groupes, et des ressources réseau.

#### **Fonctions principales :**
1. **Authentification des utilisateurs** : Chaque utilisateur doit se connecter avec un identifiant unique (par ex. user1).
2. **Gestion centralisée** :
   - Les administrateurs gèrent les utilisateurs, les groupes, et les permissions depuis un endroit unique (SRV1).
3. **Intégration des services** :
   - IIS, FTP et la PKI s’intègrent à Active Directory pour authentifier les utilisateurs.

#### **Pourquoi Active Directory est utile ?**
- Simplifie la gestion des utilisateurs dans le réseau.
- Améliore la sécurité en centralisant les permissions.

---

### **Résumé des concepts de l’examen**

| **Concept**                     | **Résumé**                                                                 |
|----------------------------------|---------------------------------------------------------------------------|
| **PKI**                          | Système qui gère les certificats numériques pour sécuriser les communications. |
| **CA (Certificate Authority)**   | Émet les certificats numériques et garantit leur validité.                |
| **DNS**                          | Traduit les noms de domaine en adresses IP pour simplifier les connexions. |
| **IIS (Internet Information Services)** | Serveur web permettant d’héberger des sites HTTP/HTTPS et FTP.               |
| **FTP**                          | Protocole de transfert de fichiers avec options de sécurité et d’isolation. |
| **Active Directory (AD)**        | Centralise la gestion des utilisateurs et des ressources réseau.          |





-----------------------
# Exemples:
-----------------------

### **1. Exemple : Génération d’un certificat dans une infrastructure PKI**

#### **Objectif :**
Créer un certificat numérique pour sécuriser un site web (HTTPS) via une autorité de certification (CA).

#### **Étapes :**
1. **Configurer une Autorité de Certification (CA) sur SRV1 :**
   - Ouvrez le **Gestionnaire de serveur** > "Ajouter des rôles et fonctionnalités".
   - Installez le rôle **Active Directory Certificate Services (AD CS)**.
   - Activez les services suivants :
     - Certification Authority (CA).
     - Certification Authority Web Enrollment.
   - Configurez SRV1 comme une **CA d’entreprise racine**.

2. **Créer un certificat pour SRV2 (site web HTTPS) :**
   - Sur SRV2, ouvrez **MMC** (Microsoft Management Console).
     - Tapez `mmc` dans la barre de recherche Windows.
   - Ajoutez le composant logiciel enfichable "Certificats" pour **Ordinateur local**.
   - Faites un clic droit sur "Certificats (ordinateur local)" > **Toutes les tâches > Demander un nouveau certificat**.
   - Suivez les étapes pour sélectionner le modèle "Web Server" et soumettre une demande à la CA.

3. **Associer le certificat au site web :**
   - Dans **IIS Manager** sur SRV2 :
     - Allez dans "Bindings du site" pour **www1.examenf.local**.
     - Ajoutez une liaison HTTPS en sélectionnant le certificat émis.

4. **Validation :**
   - Depuis un navigateur, accédez à **https://www1.examenf.local**.
   - Assurez-vous que la connexion est sécurisée (cadenas dans la barre d'adresse).

---

### **2. Exemple : Ajouter des enregistrements DNS**

#### **Objectif :**
Associer des noms DNS (**ftp1.examenf.local** et **www1.examenf.local**) à l’adresse IP du serveur SRV2.

#### **Étapes :**
1. **Ajouter des enregistrements A dans DNS :**
   - Sur SRV1, ouvrez **Outils > DNS**.
   - Sélectionnez la zone **examenf.local**.
   - Clic droit > "Nouvel hôte (A)" :
     - Nom : **ftp1**.
     - Adresse IP : **192.168.10.3**.
   - Répétez pour **www1** avec la même IP.

2. **Valider la configuration DNS :**
   - Sur SRV2, ouvrez l’invite de commandes et exécutez :
     ```bash
     nslookup ftp1.examenf.local
     nslookup www1.examenf.local
     ```
   - Les deux noms doivent retourner l’adresse **192.168.10.3**.

---

### **3. Exemple : Configurer un site web avec IIS**

#### **Objectif :**
Créer un site web accessible en HTTP et HTTPS, sécurisé par un certificat PKI.

#### **Étapes :**
1. **Installer IIS :**
   - Sur SRV2, ouvrez le **Gestionnaire de serveur** > "Ajouter des rôles et fonctionnalités".
   - Installez le rôle **Web Server (IIS)** et activez les modules HTTP et HTTPS.

2. **Créer un site web :**
   - Ouvrez **IIS Manager**.
   - Clic droit sur "Sites" > **Ajouter un site web** :
     - Nom : **www1.examenf.local**.
     - Répertoire physique : **C:\www1** (créez ce dossier).
     - Liaison :
       - **HTTP** : Port **80**.
       - **HTTPS** : Port **443** (après avoir ajouté le certificat).

3. **Ajouter des répertoires virtuels :**
   - Dans **C:\www1**, créez deux dossiers :
     - **Documents**.
     - **Ressources**.
   - Dans IIS Manager, ajoutez-les comme répertoires virtuels.

4. **Validation :**
   - Accédez au site via **http://www1.examenf.local** et **https://www1.examenf.local**.
   - Naviguez dans les répertoires virtuels.

---

### **4. Exemple : Création d’utilisateurs dans Active Directory**

#### **Objectif :**
Créer deux utilisateurs, **user1** et **user2**, dans le domaine.

#### **Étapes :**
1. **Ouvrir Active Directory Users and Computers sur SRV1 :**
   - Allez dans **Outils > Active Directory Users and Computers**.
   - Sélectionnez une unité d’organisation (OU) ou créez-en une nouvelle pour les utilisateurs.

2. **Créer les utilisateurs :**
   - Faites un clic droit sur l’OU > **Nouvel utilisateur**.
   - Configurez les informations :
     - Nom complet : **User1**.
     - Nom d’utilisateur : **user1**.
     - Mot de passe : **Passw0rd123!** (ou un mot de passe similaire).
   - Répétez pour **user2**.

3. **Validation :**
   - Connectez-vous à un poste client ou à SRV2 avec **user1** et **user2** pour vérifier les comptes.

---

### **5. Exemple : Configurer un site FTP avec isolation des utilisateurs**

#### **Objectif :**
Configurer un site FTP où chaque utilisateur dispose de son propre dossier privé.

#### **Étapes :**
1. **Installer FTP sur IIS :**
   - Sur SRV2, ouvrez le **Gestionnaire de serveur** > "Ajouter des rôles et fonctionnalités".
   - Activez **FTP Server** dans les options IIS.

2. **Créer un site FTP :**
   - Dans IIS Manager, faites un clic droit sur "Sites" > **Ajouter un site FTP** :
     - Nom du site : **FTP_Site**.
     - Répertoire racine : **C:\FTP_Site** (créez ce dossier).
     - Liaison : Port **21**.

3. **Configurer l’isolation des utilisateurs :**
   - Dans IIS Manager > Paramètres FTP, activez **Isoler les utilisateurs**.
   - Structure des dossiers :
     - **C:\FTP_Site\Anonymous** : Accessible à tous (anonyme).
     - **C:\FTP_Site\User1** : Accessible uniquement par **user1**.
     - **C:\FTP_Site\User2** : Accessible uniquement par **user2**.

4. **Configurer les permissions :**
   - Cliquez avec le bouton droit sur chaque dossier > **Propriétés > Sécurité**.
   - Attribuez les permissions :
     - **Anonymous** : Lecture/écriture pour "Anonymous Logon".
     - **User1** : Lecture/écriture uniquement pour **user1**.
     - **User2** : Lecture/écriture uniquement pour **user2**.

5. **Validation :**
   - Utilisez un client FTP comme FileZilla pour tester :
     - **Anonyme** : Accédez au dossier **Anonymous**.
     - **User1** : Accédez au dossier **User1** avec les identifiants **user1 / Passw0rd123!**.
     - **User2** : Accédez au dossier **User2**.

---

### **Résumé des validations à réaliser :**

1. **PKI :**
   - Testez l’accès HTTPS au site sécurisé avec un certificat PKI.

2. **DNS :**
   - Validez les enregistrements DNS avec `nslookup`.

3. **IIS :**
   - Testez l’accès au site web en HTTP et HTTPS.
   - Naviguez dans les répertoires virtuels.

4. **Active Directory :**
   - Connectez-vous avec **user1** et **user2** pour vérifier leurs comptes.

5. **FTP :**
   - Testez l’accès aux dossiers FTP avec les comptes **Anonymous**, **user1**, et **user2**.
