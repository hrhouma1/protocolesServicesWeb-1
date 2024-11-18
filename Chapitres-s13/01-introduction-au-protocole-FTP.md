### **Cours détaillé : Protocole FTP et variantes sécurisées**

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
