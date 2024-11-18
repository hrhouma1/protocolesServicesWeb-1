# **TP3 : Configuration des Services Réseau et Sécurisation sur Windows Server 2019**

---

### **Objectif**

Mettre en place un environnement réseau sécurisé et performant comprenant :
- **DC1 (Contrôleur de Domaine Principal)**.
- **DC2 (Contrôleur de Domaine Secondaire)**.
- **RODC (Read-Only Domain Controller)**.
- **Routeur pfSense** pour la gestion du réseau avec VLANs.
- **Clients Windows 10/11** connectés au réseau via un VLAN dédié.

---

### **Configuration des VLANs et des Adresses IP**

Chaque composant sera sur un réseau **VLAN distinct**, avec une plage d’adresses IP spécifique.

| **Composant**         | **VLAN** | **Réseau**         | **Adresse IP Exemple** | **Plage DHCP**         |
|------------------------|----------|--------------------|-------------------------|------------------------|
| **pfSense - VLAN 10**  | 10       | 192.168.10.0/24    | 192.168.10.1           | N/A                    |
| **DC1 (Principal)**    | 10       | 192.168.10.0/24    | 192.168.10.10          | N/A                    |
| **DC2 (Secondaire)**   | 10       | 192.168.10.0/24    | 192.168.10.11          | N/A                    |
| **RODC**               | 10       | 192.168.10.0/24    | 192.168.10.12          | N/A                    |
| **Clients VLAN 20**    | 20       | 192.168.20.0/24    | DHCP                   | 192.168.20.100-192.168.20.150 |

---

### **Étapes Complètes**

#### **1. Configuration de pfSense**

1. **Installer pfSense sur VMware** et configurer deux interfaces réseau :
   - **WAN** : Connectée à Internet (Mode NAT VMware).
   - **LAN** : Gère les VLANs internes.

2. **Configurer les VLANs dans pfSense :**
   - VLAN 10 : Pour les serveurs (DC1, DC2, RODC).
   - VLAN 20 : Pour les clients Windows.
   - **Assignation** :
     - LAN Interface = Trunk (Marquage VLAN 802.1Q).
     - VLAN 10 = 192.168.10.1.
     - VLAN 20 = 192.168.20.1.

3. **Configurer les plages DHCP par VLAN :**
   - VLAN 10 : Désactiver DHCP (adresses IP statiques pour les serveurs).
   - VLAN 20 : Activer DHCP avec la plage `192.168.20.100 - 192.168.20.150`.

---

#### **2. Configuration des Contrôleurs de Domaine**

##### **2.1. DC1 (Contrôleur de Domaine Principal)**
1. **Installer Windows Server 2019 sur DC1.**
2. **Configurer l’interface réseau** :
   - Connecter au VLAN 10.
   - Adresse IP statique : `192.168.10.10`.
   - Masque : `255.255.255.0`.
   - Passerelle : `192.168.10.1` (pfSense VLAN 10).
   - DNS : `192.168.10.10` (lui-même).

3. **Installer et configurer Active Directory Domain Services (AD DS) :**
   - Créer une nouvelle forêt : `mondomaine.local`.
   - Promouvoir le serveur comme **Contrôleur de Domaine Principal**.
   - **Capture requise :** Résumé de la promotion AD DS.

4. **Configurer DNS :**
   - Créer une zone DNS primaire : `mondomaine.local`.
   - **Capture requise :** Configuration de la zone DNS.

5. **Configurer le DHCP (optionnel pour VLAN 10 si nécessaire)**.

---

##### **2.2. DC2 (Contrôleur de Domaine Secondaire)**
1. **Installer Windows Server 2019 sur DC2.**
2. **Configurer l’interface réseau :**
   - Connecter au VLAN 10.
   - Adresse IP statique : `192.168.10.11`.

3. **Promouvoir DC2 en tant que Contrôleur de Domaine Additionnel :**
   - Rejoindre le domaine `mondomaine.local`.
   - Synchroniser DNS avec DC1.
   - **Capture requise :** Promotion réussie de DC2.

4. **Configurer la redondance DHCP :**
   - Activer le **basculement DHCP** entre DC1 et DC2.
   - **Capture requise :** Configuration de basculement.

---

##### **2.3. RODC (Read-Only Domain Controller)**
1. **Installer Windows Server 2019 sur RODC.**
2. **Configurer l’interface réseau :**
   - Connecter au VLAN 10.
   - Adresse IP statique : `192.168.10.12`.

3. **Promouvoir RODC :**
   - Configurer comme **Read-Only Domain Controller**.
   - **Capture requise :** Promotion réussie en RODC.

---

#### **3. Configuration des Clients Windows 10/11**

1. **Configurer l’interface réseau :**
   - Connecter au VLAN 20 (DHCP activé).
   - Vérifier que le client reçoit une adresse IP du serveur DHCP VLAN 20.

2. **Joindre au domaine :**
   - Rejoindre `mondomaine.local` à l’aide des informations d’identification.
   - **Capture requise :** Propriétés système montrant l’appartenance au domaine.

3. **Tests de validation :**
   - **Ping** vers DC1, DC2 et RODC.
   - Résolution DNS avec `nslookup`.
   - Accès aux ressources partagées.

---

#### **4. Tests de Résilience et de Redondance**

1. **Déconnecter DC1 :**
   - Vérifier que DC2 gère le DHCP et la redondance DNS.
   - **Capture requise :** Client obtenant une adresse via DHCP de DC2.

2. **Tester le RODC :**
   - Déconnecter DC1 et DC2.
   - Vérifier que le RODC peut authentifier les utilisateurs.
   - **Capture requise :** Résultats de connexion avec RODC.

---

### **Schéma Réseau ASCII Corrigé**

```plaintext
                     +----------------------+
                     |      Internet       |
                     +----------+-----------+
                                |
                     +----------+-----------+
                     |     pfSense Router    |
                     | VLAN 10: 192.168.10.1 |
                     | VLAN 20: 192.168.20.1 |
                     +----------+-----------+
                                |
         ------------------------------------------------
         | VLAN 10                                VLAN 20 |
    +-------------+    +-------------+              +-------------+
    |    DC1      |    |    DC2      |              |  Clients    |
    | 192.168.10.10|    | 192.168.10.11|             | DHCP (Auto) |
    +-------------+    +-------------+              +-------------+
         |                   |                       
    +-------------+                                     
    |    RODC     |                                       
    | 192.168.10.12|                                      
    +-------------+                                     
```

---

### **Livrables et Captures d'Écran**
1. Rapport détaillé.
2. Toutes les étapes avec captures d’écran :
   - Promotion AD DS, configuration DNS, basculement DHCP, et joignage des clients.
   - Résultats des tests de ping, nslookup, et redondance.

