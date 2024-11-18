### **1. Objectif du TP**
Configurer un environnement réseau robuste et sécurisé, comprenant :
- Un **Contrôleur de Domaine Principal (DC1)**.
- Un **Contrôleur de Domaine Secondaire (DC2)**.
- Un **Read-Only Domain Controller (RODC)**.
- Un routeur **pfSense** pour la gestion réseau.
- Des clients sous **Windows 10/11** pour tester l’intégration.

---

### **2. Matériel et Logiciel Nécessaires**
- **Routeur pfSense** : Assure le routage et la gestion du réseau.
- **VMware** : Pour virtualiser l’environnement (pfSense, serveurs, clients).
- **3 machines virtuelles** avec **Windows Server 2019** pour DC1, DC2 et RODC.
- **1 machine virtuelle** avec Windows 10/11 pour le client.

---

### **3. Schéma Réseau ASCII (Exemple)**

```plaintext
                      +----------------------+
                      |      Internet       |
                      +----------+-----------+
                                 |
                      +----------+-----------+
                      |     pfSense Router    |
                      | LAN: 192.168.1.1     |
                      +----------+-----------+
                                 |
          ------------------------------------------------
          |                  |                 |  
    +-----+-----+     +------+-----+     +-----+-----+
    |   DC1      |     |   DC2      |     |   RODC     |
    | 192.168.1.10 |   | 192.168.1.11 |   | 192.168.1.12 |
    +-------------+     +-------------+     +-----------+
                                 |
                         +----------------+
                         |  Windows 10/11 |
                         | DHCP: 192.168.1.X |
                         +----------------+
```

---

### **4. Configuration Étape par Étape**

#### **Partie 1 : DC1 (Contrôleur de Domaine Principal)**
1. **Installation d'AD DS :**
   - Rôle : Active Directory Domain Services (AD DS).
   - Promouvoir DC1 comme contrôleur de domaine principal.
   - Domaine : `mondomaine.local`.
   - **Capture d’écran :** Résumé de la promotion AD DS.

2. **Configuration DHCP :**
   - Configurez une plage d’adresses : `192.168.1.100 - 192.168.1.150`.
   - **Capture d’écran :** Étendue DHCP configurée.

3. **Configuration DNS :**
   - Ajoutez une zone primaire : `mondomaine.local`.
   - **Capture d’écran :** Zone DNS directe configurée.

---

#### **Partie 2 : DC2 (Contrôleur de Domaine Secondaire)**
1. **Installation d'AD DS :**
   - Promouvoir DC2 comme contrôleur de domaine additionnel.
   - **Capture d’écran :** Configuration de la promotion AD DS.

2. **Configuration DNS :**
   - Synchronisez les zones avec DC1.
   - **Capture d’écran :** Zones synchronisées (directe et inversée).

3. **Redondance DHCP :**
   - Configurez le basculement DHCP avec DC1.
   - **Capture d’écran :** Basculement DHCP activé.

---

#### **Partie 3 : RODC**
1. **Installation d'AD DS :**
   - Promouvoir le serveur comme **Read-Only Domain Controller**.
   - **Capture d’écran :** Fenêtre de promotion RODC.

---

#### **Partie 4 : Clients Windows 10/11**
1. Configurez les clients pour obtenir une IP automatiquement via DHCP.
2. Joignez les clients au domaine `mondomaine.local`.
3. Testez la connectivité :
   - Ping vers DC1, DC2, et RODC.
   - Exécutez `nslookup` pour valider la résolution DNS.
   - **Captures d’écran :** Joindre au domaine, résultats de `ping` et `nslookup`.

---

#### **Partie 5 : Tests de Résilience et Redondance**
1. **DHCP :**
   - Arrêtez DC1 et vérifiez que DC2 attribue les adresses DHCP.
   - **Capture d’écran :** Bail DHCP obtenu du DC2.

2. **DNS :**
   - Avec DC1 hors ligne, vérifiez que RODC résout les requêtes DNS et authentifie les utilisateurs.
   - **Capture d’écran :** Résultats de requêtes DNS et connexions au domaine.

---

### **5. Adresses IP et Cartes Réseau**
Chaque machine virtuelle doit être configurée comme suit :

| Machine           | Adresse IP      | Rôle                         | Carte Réseau VMWARE   |
|--------------------|-----------------|------------------------------|-----------------------|
| pfSense           | 192.168.1.1     | Routeur                      | NAT                   |
| DC1               | 192.168.1.10    | Contrôleur de domaine        | LAN                   |
| DC2               | 192.168.1.11    | Contrôleur de domaine (Sec.) | LAN                   |
| RODC              | 192.168.1.12    | Read-Only Domain Controller  | LAN                   |
| Windows 10/11     | DHCP (dynamic)  | Client                       | LAN                   |

---

### **6. Livrables**
- Rapport structuré avec toutes les étapes détaillées.
- Captures d’écran pour chaque étape.
- Schéma réseau détaillé.
- Vérification des tests de redondance et résilience.

