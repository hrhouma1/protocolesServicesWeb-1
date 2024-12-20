*Démonstration pratique*


# *1. Créer une infrastructure PKI pour distribuer des certificats dans le domaine EXAMEMF.LOCAL*

**Explication pédagogique :**
La PKI (Public Key Infrastructure) permet de créer et gérer des certificats numériques pour sécuriser les communications. Ici, nous allons configurer une autorité de certification (CA) sur SRV1 pour émettre des certificats internes.

---

#### Étapes détaillées :
1. **Ajout du rôle de Certification sur SRV1** :
   - Sur **SRV1**, ouvrez le **Gestionnaire de serveur**.
   - Cliquez sur **Gérer > Ajouter des rôles et fonctionnalités**.
   - Choisissez "Installation basée sur un rôle ou une fonctionnalité".
   - Sélectionnez **Active Directory Certificate Services (AD CS)**.
   - Cochez les services suivants :
     - **Certification Authority**.
     - **Certification Authority Web Enrollment**.
   - Cliquez sur **Installer**.

2. **Configuration de la CA :**
   - Une fois l’installation terminée, cliquez sur **Configurer Active Directory Certificate Services**.
   - Sélectionnez :
     - **Certification Authority d’entreprise**.
     - **Autorité de certification racine**.
   - Configurez une clé privée (option "Créer une nouvelle clé privée").
   - Validez les paramètres par défaut et terminez.

3. **Vérification :**
   - Ouvrez la console **Autorité de certification** (Tapez `certsrv.msc` dans le menu Démarrer).
   - Assurez-vous que le service est actif (une icône verte apparaît).

---

**Validation :**
- Demandez un certificat pour SRV2 via MMC (Microsoft Management Console).
- Installez ce certificat sur IIS (dans la section HTTPS).

**Captures demandées :**
- Console **Autorité de certification** montrant la configuration réussie.

---

# *2. Ajouter deux enregistrements DNS : ftp1.examenf.local et www1.examenf.local*

**Explication pédagogique :**
Le DNS permet de traduire les noms de domaine en adresses IP. Ici, nous associons les noms **ftp1.examenf.local** et **www1.examenf.local** à l’adresse IP de SRV2.

---

#### Étapes détaillées :
1. **Accéder à la console DNS sur SRV1 :**
   - Ouvrez le **Gestionnaire de serveur > Outils > DNS**.
   - Naviguez jusqu’à la zone **examenf.local**.

2. **Créer les enregistrements A :**
   - Clic droit sur la zone **examenf.local** > **Nouvel hôte (A)**.
   - Pour **ftp1** :
     - Nom : **ftp1**.
     - IP : **192.168.10.3**.
   - Pour **www1** :
     - Nom : **www1**.
     - IP : **192.168.10.3**.

3. **Validation :**
   - Ouvrez une invite de commande sur SRV2.
   - Tapez :
     ```bash
     nslookup ftp1.examenf.local
     nslookup www1.examenf.local
     ```

---

**Captures demandées :**
- Enregistrements DNS dans la console DNS.

---

# *3. Installez IIS sur le serveur SRV2 et configurez un serveur web*

**Explication :**
IIS (Internet Information Services) est un serveur web qui permet d’héberger des sites web en HTTP/HTTPS. Nous allons configurer IIS pour héberger un site sécurisé avec un certificat PKI.

---

#### Étapes détaillées :
1. **Installer IIS sur SRV2 :**
   - Ouvrez le **Gestionnaire de serveur** sur SRV2.
   - Ajoutez le rôle **Serveur Web (IIS)**.
   - Cochez les fonctionnalités **HTTP** et **HTTPS**.
   - Installez.

2. **Créer un site web :**
   - Ouvrez **IIS Manager**.
   - Faites un clic droit sur "Sites" > **Ajouter un site web** :
     - Nom : **www1.examenf.local**.
     - Répertoire physique : **C:\www1**.
     - Liaison HTTP : Port **80**.
     - Liaison HTTPS : Port **443** (après ajout du certificat).

3. **Ajouter des répertoires virtuels :**
   - Créez deux dossiers dans **C:\www1** :
     - **Documents**.
     - **Ressources**.
   - Retournez dans IIS Manager :
     - Faites un clic droit sur le site > **Ajouter un répertoire virtuel** pour chaque dossier.

4. **Validation :**
   - Accédez à **http://www1.examenf.local** et **https://www1.examenf.local**.

---

**Captures demandées :**
- Liaison HTTPS dans IIS.
- Structure des dossiers **Documents** et **Ressources**.

---

# *4. Création de deux utilisateurs user1 et user2 dans le domaine AD*

**Explication pédagogique :**
Active Directory permet de gérer les utilisateurs du domaine. Nous allons créer deux utilisateurs pour des tests.

---

#### Étapes détaillées :
1. **Ouvrir Active Directory Users and Computers sur SRV1 :**
   - Ouvrez **Outils > Active Directory Users and Computers**.

2. **Créer les utilisateurs :**
   - Faites un clic droit sur l’OU **Utilisateurs** > **Nouvel utilisateur**.
   - Créez les utilisateurs :
     - Nom d’utilisateur : **user1**, **user2**.
     - Mot de passe : **Passw0rd123**.

3. **Validation :**
   - Connectez-vous avec ces utilisateurs sur un poste client pour vérifier.

---

**Captures demandées :**
- Liste des utilisateurs dans Active Directory.

---

# *5. Configuration d’un site FTP avec isolation des utilisateurs*

**Explication  :**
Le FTP (File Transfer Protocol) est utilisé pour échanger des fichiers. L’isolation des utilisateurs permet à chacun d’avoir son propre espace privé.

---

#### Étapes détaillées :
1. **Installer FTP sur IIS (SRV2) :**
   - Ajoutez le rôle **Serveur FTP** via le **Gestionnaire de serveur**.

2. **Créer un site FTP :**
   - Ouvrez **IIS Manager**.
   - Faites un clic droit sur "Sites" > **Ajouter un site FTP** :
     - Nom : **FTP_Site**.
     - Répertoire : **C:\FTP_Site**.
     - Liaison : Port **21**.

3. **Configurer l’isolation :**
   - Allez dans **Paramètres FTP > Isolation des utilisateurs**.
   - Activez **Isoler les utilisateurs dans des dossiers spécifiques**.

4. **Créer la structure des dossiers :**
   - Dans **C:\FTP_Site**, créez :
     - **Anonymous** (lecture/écriture pour tout le monde).
     - **User1** (privé pour user1).
     - **User2** (privé pour user2).

5. **Configurer les permissions NTFS :**
   - **Anonymous** : Autoriser "Anonymous Logon".
   - **User1/User2** : Lecture/écriture uniquement pour leurs comptes respectifs.

6. **Validation :**
   - Testez avec un client FTP (FileZilla) :
     - **Anonymous** : Accède au répertoire public.
     - **User1/User2** : Accèdent à leurs dossiers respectifs.

---

**Captures demandées :**
- Paramètres IIS du site FTP.
- Structure des dossiers FTP.
- Test d’accès avec **user1**, **user2**, et **Anonymous**.

