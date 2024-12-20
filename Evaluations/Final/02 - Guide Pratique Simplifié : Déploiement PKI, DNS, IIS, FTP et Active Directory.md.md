
# Instructions simplifiées

*1. Création d'une infrastructure PKI pour distribuer des certificats dans le domaine EXAMEMF.LOCAL*

1. **Ajoutez le rôle de Certification sur SRV1** :
   - Sur SRV1, ouvrez le **Gestionnaire de serveur**.
   - Cliquez sur "Ajouter des rôles et fonctionnalités".
   - Choisissez **Active Directory Certificate Services (AD CS)** et suivez les étapes pour installer le rôle.
   - Activez les services :
     - **Certification Authority (CA)**.
     - **Certification Authority Web Enrollment**.

2. **Configurez la CA (Certification Authority)** :
   - Pendant l’installation, choisissez "Autorité de certification d’entreprise".
   - Configurez-la comme une **autorité de certification racine**.

3. **Validez** :
   - Ouvrez la console "Autorité de certification" sur SRV1.
   - Assurez-vous que le service est démarré.

---

*2. Ajouter deux enregistrements DNS : ftp1.examenf.local et www1.examenf.local*

1. **Accédez à la console DNS sur SRV1** :
   - Ouvrez **Outils > DNS**.
   - Sélectionnez la zone **examenf.local**.

2. **Créez les enregistrements A** :
   - Faites un clic droit sur la zone **examenf.local** > "Nouvel hôte (A)".
   - Entrez **ftp1** comme nom et l’adresse **192.168.10.3**.
   - Répétez pour **www1** avec la même IP.

3. **Validez** :
   - Exécutez les commandes suivantes sur SRV1 ou SRV2 pour vérifier :
     ```bash
     nslookup ftp1.examenf.local
     nslookup www1.examenf.local
     ```

---

*3. Installation et configuration de IIS sur SRV2*

1. **Installer IIS sur SRV2** :
   - Ouvrez le **Gestionnaire de serveur** sur SRV2.
   - Ajoutez le rôle **Web Server (IIS)**.
   - Activez les fonctionnalités **HTTP** et **HTTPS**.

2. **Créer le site web** :
   - Ouvrez **IIS Manager**.
   - Faites un clic droit sur "Sites" > "Ajouter un site web".
   - Configurez :
     - Nom du site : **www1.examenf.local**.
     - Répertoire physique : **C:\www1**.
     - Liaison : Port **80** pour HTTP et **443** pour HTTPS.

3. **Obtenir un certificat HTTPS** :
   - Ouvrez **IIS Manager** sur SRV2.
   - Allez dans le panneau **SSL**.
   - Demandez un certificat signé par SRV1 (PKI).

4. **Configurer les répertoires virtuels** :
   - Créez deux sous-dossiers dans **C:\www1** :
     - **Documents**.
     - **Ressources**.
   - Configurez les deux comme répertoires virtuels dans IIS.

5. **Testez l’accès** :
   - Accédez depuis un navigateur à :
     - **http://www1.examenf.local**.
     - **https://www1.examenf.local**.

---

*4. Création de deux utilisateurs user1 et user2 dans le domaine*

1. **Créer les utilisateurs dans Active Directory** :
   - Sur SRV1, ouvrez **Active Directory Users and Computers**.
   - Faites un clic droit sur l’unité d’organisation (OU) appropriée > **Nouvel utilisateur**.
   - Créez deux utilisateurs :
     - Nom d’utilisateur : **user1** et **user2**.
     - Mot de passe : Attribuez des mots de passe simples à des fins de test.

---

*5. Configuration d’un site FTP avec isolation des utilisateurs*

1. **Installer FTP sur IIS (SRV2)** :
   - Sur SRV2, ouvrez le **Gestionnaire de serveur**.
   - Ajoutez le rôle **FTP Server** à IIS.

2. **Créer le site FTP** :
   - Ouvrez **IIS Manager**.
   - Faites un clic droit sur "Sites" > **Ajouter un site FTP**.
   - Configurez :
     - Nom du site : **FTP_Site**.
     - Répertoire : **C:\FTP_Site**.
     - Liaison : Port **21**.
   - Activez **Isolation d’utilisateur** dans les paramètres.

3. **Créer la structure des répertoires** :
   - Dans **C:\FTP_Site**, créez les dossiers suivants :
     - **Anonymous** : Accessible en lecture/écriture pour tout le monde.
     - **User1** : Accessible uniquement pour **user1**.
     - **User2** : Accessible uniquement pour **user2**.

4. **Configurer les permissions** :
   - Configurez les permissions NTFS sur chaque dossier :
     - **Anonymous** : Permissions pour "Anonymous Logon".
     - **User1** : Lecture/écriture uniquement pour **user1**.
     - **User2** : Lecture/écriture uniquement pour **user2**.

5. **Testez l’accès** :
   - Testez l’accès avec un client FTP comme **FileZilla** :
     - Utilisez **Anonymous** pour le dossier public.
     - Utilisez les comptes **user1** et **user2** pour accéder à leurs dossiers respectifs.

---

### **Captures d’écran demandées**
Pour chaque section, capturez les étapes importantes :
1. **PKI** : Configuration de l’autorité de certification sur SRV1.
2. **DNS** : Liste des enregistrements DNS dans la console DNS.
3. **IIS** :
   - Configuration des bindings HTTP/HTTPS.
   - Structure des répertoires (Documents, Ressources).
   - Test d’accès via un navigateur.
4. **FTP** :
   - Configuration du site FTP.
   - Structure des dossiers FTP.
   - Test de connexion avec les comptes **Anonymous**, **user1**, et **user2**.

