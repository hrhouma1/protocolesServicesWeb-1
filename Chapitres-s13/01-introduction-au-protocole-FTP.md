# **Protocole FTP et variantes sécurisées**

---

### **1. Introduction au protocole FTP**
Le **FTP (File Transfer Protocol)** est un protocole réseau utilisé pour transférer des fichiers entre un client et un serveur sur un réseau informatique, notamment sur Internet. Il fonctionne selon un modèle client-serveur.

#### **Objectifs du FTP**
- Transférer des fichiers entre machines.
- Assurer des opérations comme :
  - Téléchargement de fichiers (serveur vers client).
  - Téléversement de fichiers (client vers serveur).
  - Navigation dans les répertoires distants.
  - Suppression, renommage ou modification des fichiers.

---

### **2. Fonctionnement du protocole FTP**
#### **Architecture et Ports**
FTP utilise deux canaux de communication :
1. **Canal de commande** (port 21) : utilisé pour envoyer des commandes et recevoir des réponses.
2. **Canal de données** (port 20, par défaut) : utilisé pour transférer les données.

#### **Modes de fonctionnement**
1. **Mode actif** :
   - Le client ouvre un port et envoie son adresse IP et son numéro de port au serveur.
   - Le serveur initie la connexion au client pour le transfert des données.

2. **Mode passif** :
   - Le serveur ouvre un port et fournit son adresse IP et numéro de port au client.
   - Le client initie la connexion pour le transfert des données.
   - Plus adapté aux pare-feu et aux NAT (très utilisé aujourd'hui).

#### **Commandes FTP courantes**
Les commandes FTP sont envoyées via le canal de commande. Voici quelques exemples :
- `USER <nom_utilisateur>` : Identification de l'utilisateur.
- `PASS <mot_de_passe>` : Envoi du mot de passe.
- `LIST` : Liste les fichiers du répertoire courant.
- `STOR <nom_fichier>` : Téléverse un fichier sur le serveur.
- `RETR <nom_fichier>` : Télécharge un fichier depuis le serveur.
- `QUIT` : Quitte la session FTP.

#### **Transmission des données**
FTP transfère les données en **texte brut**, ce qui peut poser des problèmes de sécurité, car :
- Les identifiants (nom d'utilisateur et mot de passe) sont envoyés sans chiffrement.
- Les fichiers sont également transmis sans chiffrement.

---

### **3. Limitations du FTP**
- **Sécurité faible** : Les données sont envoyées en clair, rendant le protocole vulnérable aux attaques comme l'interception et le vol de données.
- **Dépendance aux ports** : FTP utilise plusieurs ports, ce qui complique la configuration des pare-feu.

---

### **4. Introduction à FTPS (FTP Sécurisé)**
Pour répondre aux problèmes de sécurité, des variantes sécurisées ont été introduites, notamment **FTPS**.

#### **Qu'est-ce que le FTPS ?**
Le FTPS est une extension du FTP qui intègre le chiffrement SSL/TLS (Secure Sockets Layer / Transport Layer Security) pour sécuriser les transferts de données.

#### **Caractéristiques principales**
- **Chiffrement** : Protège les données en transit.
- **Authentification** : Utilise des certificats pour garantir l'identité du serveur (et parfois du client).
- **Compatibilité** : FTPS est basé sur le FTP et utilise les mêmes commandes.

#### **Modes de chiffrement dans FTPS**
1. **FTPS explicite (Explicit FTPS)** :
   - La connexion commence comme un FTP classique.
   - Le client demande au serveur de sécuriser la session via une commande `AUTH TLS`.
   - Si le serveur accepte, le chiffrement est activé.

2. **FTPS implicite (Implicit FTPS)** :
   - Le chiffrement est activé dès le début de la connexion.
   - Utilise généralement le port 990 au lieu du port 21.

#### **Avantages de FTPS**
- Assure la confidentialité et l'intégrité des données.
- Protège les identifiants (nom d'utilisateur et mot de passe).

#### **Inconvénients de FTPS**
- Complexité de la configuration (certificats SSL/TLS, pare-feu, etc.).
- Moins utilisé que des alternatives modernes comme SFTP.

---

### **5. Différences entre FTP, FTPS et SFTP**
| **Protocole** | **Port par défaut** | **Chiffrement**      | **Avantages**                                | **Inconvénients**                     |
|---------------|---------------------|----------------------|----------------------------------------------|---------------------------------------|
| **FTP**       | 21 (commande), 20 (données) | Aucun               | Simple à configurer, rapide                 | Peu sécurisé (données en clair)       |
| **FTPS**      | 21/990             | SSL/TLS              | Sécurisé, basé sur FTP                      | Configuration complexe                |
| **SFTP**      | 22                 | SSH (Secure Shell)   | Très sécurisé, un seul port utilisé         | Différent de FTP (nouveaux outils nécessaires) |

---

### **6. Cas pratiques : Utilisation du FTP et FTPS**
#### **Exemple d'utilisation FTP avec un client (FileZilla, WinSCP, etc.)**
1. Téléchargez et installez un client FTP (FileZilla).
2. Configurez la connexion :
   - Adresse du serveur.
   - Nom d'utilisateur et mot de passe.
   - Port (21 pour FTP, 990 pour FTPS).
3. Naviguez dans les répertoires distants et effectuez des transferts de fichiers.

#### **Configuration d'un serveur FTP/FTPS**
1. Installez un serveur FTP comme **vsftpd**, **ProFTPD** ou **FileZilla Server**.
2. Activez le chiffrement SSL/TLS pour FTPS.
3. Configurez des règles de pare-feu pour autoriser les ports FTP/FTPS.

---

### **7. Résumé**
- **FTP** : Protocole de base pour le transfert de fichiers, mais peu sécurisé.
- **FTPS** : Version sécurisée avec chiffrement SSL/TLS.
- Alternative moderne : **SFTP** (basé sur SSH) offre une meilleure sécurité et simplicité.

#### **Conclusion**
Bien que le FTP reste utile dans certains contextes, ses limites en matière de sécurité ont conduit à son remplacement progressif par des protocoles sécurisés comme FTPS et SFTP. Il est essentiel de choisir le bon protocole en fonction des besoins en sécurité et en compatibilité.



# Annexe 01  - ALternatives de FTP

*Cette table présente uniquement les alternatives directes et comparables au FTP, comme FTPS, SFTP, et d'autres protocoles conçus pour le transfert ou la gestion de fichiers. Les autres technologies mentionnées offrent des fonctionnalités complémentaires mais peuvent aussi répondre à certains cas d'usage similaires.*

```
+------------------+--------------------------------------+--------------------------+
| Protocole        | Caractéristiques                    | Popularité               |
+------------------+--------------------------------------+--------------------------+
| FTP              | Transfert de fichiers basique       | Moyen (sécurité faible)  |
+------------------+--------------------------------------+--------------------------+
| FTPS             | FTP avec chiffrement SSL/TLS        | Populaire (entreprises)  |
+------------------+--------------------------------------+--------------------------+
| SFTP             | Basé sur SSH, sécurisé              | Très populaire (modernité et sécurité) |
+------------------+--------------------------------------+--------------------------+
| SCP              | Copie sécurisée via SSH             | Moins populaire (limité aux transferts directs) |
+------------------+--------------------------------------+--------------------------+
| WebDAV           | Extension HTTP pour gestion de fichiers | Populaire (collaboratif et intégré aux systèmes) |
+------------------+--------------------------------------+--------------------------+
| Rsync            | Synchronisation efficace des fichiers | Populaire (administrateurs systèmes) |
+------------------+--------------------------------------+--------------------------+
| Cloud Storage    | Services comme Google Drive, OneDrive, etc. | Très populaire (usages modernes et collaboratifs) |
+------------------+--------------------------------------+--------------------------+
| TFTP             | FTP simplifié (sans authentification) | Peu populaire (utilisé pour équipements réseau) |
+------------------+--------------------------------------+--------------------------+
```

### **Légende de popularité** :
- **Très populaire** : Largement utilisé dans les environnements modernes, bien adapté aux besoins actuels.
- **Populaire** : Couramment utilisé dans des cas spécifiques ou dans des environnements professionnels.
- **Moyen** : Employé principalement dans des contextes hérités ou limités.
- **Peu populaire** : Usage rare, souvent limité à des niches techniques ou des besoins spécifiques.


--------------
-------------
-----------------

# Annexe 02 - analyse pour chaque protocole 


*Tous les protocoles mentionnés peuvent être utilisés pour l'**upload** (téléversement) de fichiers sur Internet, mais leur usage varie en fonction du contexte, des besoins de sécurité, et de la facilité de mise en œuvre.*

---

### **1. FTP (File Transfer Protocol)**
- **Usage** : Téléversement classique de fichiers vers un serveur.
- **Avantages** :
  - Simple et rapide.
  - Compatible avec presque tous les clients et serveurs FTP.
- **Inconvénients** :
  - Non sécurisé (données et identifiants transmis en clair).
  - Peu adapté aux environnements nécessitant de la confidentialité.
- **Contexte** : Utilisé principalement dans des réseaux privés ou des systèmes hérités.

---

### **2. FTPS (FTP sécurisé via SSL/TLS)**
- **Usage** : Téléversement sécurisé de fichiers vers un serveur.
- **Avantages** :
  - Chiffrement SSL/TLS protégeant les données en transit.
  - Basé sur FTP, donc compatible avec la plupart des outils FTP.
- **Inconvénients** :
  - Configuration plus complexe (certificats SSL/TLS, pare-feu).
- **Contexte** : Environnements professionnels où la sécurité est importante.

---

### **3. SFTP (SSH File Transfer Protocol)**
- **Usage** : Téléversement sécurisé de fichiers via SSH.
- **Avantages** :
  - Très sécurisé (utilise SSH pour le chiffrement).
  - Fonctionne sur un seul port (généralement le port 22), simplifiant la configuration des pare-feu.
  - Compatible avec des outils modernes (FileZilla, WinSCP, etc.).
- **Inconvénients** :
  - Nécessite un serveur SSH configuré.
- **Contexte** : Utilisé dans la majorité des systèmes modernes nécessitant sécurité et simplicité.

---

### **4. SCP (Secure Copy Protocol)**
- **Usage** : Téléversement direct de fichiers via SSH.
- **Avantages** :
  - Très rapide et sécurisé (utilise également SSH).
  - Idéal pour des transferts simples sans gestion de répertoires distants.
- **Inconvénients** :
  - Moins flexible (ne permet pas de naviguer dans les répertoires comme FTP ou SFTP).
- **Contexte** : Téléversements rapides par ligne de commande dans des environnements techniques.

---

### **5. WebDAV (Web Distributed Authoring and Versioning)**
- **Usage** : Téléversement de fichiers via HTTP/HTTPS.
- **Avantages** :
  - Intégré dans les navigateurs et certains systèmes d’exploitation.
  - Peut utiliser HTTPS pour sécuriser les transferts.
- **Inconvénients** :
  - Nécessite une configuration serveur spécifique.
- **Contexte** : Collaboration en ligne ou téléversement de fichiers sur des systèmes cloud privés.

---

### **6. Rsync**
- **Usage** : Téléversement et synchronisation de fichiers.
- **Avantages** :
  - Très efficace pour synchroniser de gros ensembles de fichiers.
  - Sécurisé s’il est utilisé avec SSH.
- **Inconvénients** :
  - Plus complexe à configurer (ligne de commande, scripts).
- **Contexte** : Administration système et déploiement de données.

---

### **7. Cloud Storage (Google Drive, OneDrive, Dropbox, etc.)**
- **Usage** : Téléversement de fichiers vers des plateformes en ligne.
- **Avantages** :
  - Très simple pour l’utilisateur final.
  - Sécurisé par défaut (HTTPS, authentification).
- **Inconvénients** :
  - Dépendance aux fournisseurs de services.
- **Contexte** : Utilisation personnelle et collaborative (documents partagés, etc.).

---

### **8. TFTP (Trivial File Transfer Protocol)**
- **Usage** : Téléversement simple, souvent utilisé pour les appareils réseaux (routeurs, commutateurs).
- **Avantages** :
  - Très léger, fonctionne dans des environnements limités.
- **Inconvénients** :
  - Aucune sécurité.
  - Ne convient pas aux transferts Internet traditionnels.
- **Contexte** : Équipements réseau et applications spécifiques.

---

### **Résumé des usages pour l'upload sur Internet :**

| **Protocole**   | **Adapté à l'upload sur Internet ?** | **Contexte idéal**                                |
|------------------|-------------------------------------|--------------------------------------------------|
| FTP              | Oui, mais pas sécurisé             | Systèmes hérités ou réseaux privés.              |
| FTPS             | Oui                                | Transferts sécurisés en entreprise.              |
| SFTP             | Oui                                | Environnements modernes et sécurisés.            |
| SCP              | Oui                                | Transferts rapides par ligne de commande.        |
| WebDAV           | Oui                                | Collaboration en ligne ou systèmes cloud.        |
| Rsync            | Oui, avec SSH                     | Synchronisation et administration système.       |
| Cloud Storage    | Oui                                | Usage personnel ou professionnel collaboratif.   |
| TFTP             | Non (peu sécurisé)                | Équipements réseau ou téléversements locaux.     |

