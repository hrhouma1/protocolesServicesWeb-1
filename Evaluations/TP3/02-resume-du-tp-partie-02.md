### **1. Objectif du TP**
Configurer un environnement réseau robuste, sécurisé et évolutif comprenant :
- **DC1 (Contrôleur de Domaine Principal)**.
- **DC2 (Contrôleur de Domaine Secondaire)**.
- **RODC (Read-Only Domain Controller)**.
- Un routeur **pfSense** pour la gestion réseau avec VLANs.
- Des clients Windows 10/11 pour tester l’intégration.

---

### **2. Matériel et Logiciel Nécessaires**
- **Routeur pfSense** : Pour la gestion des VLANs et le routage.
- **VMware** : Pour virtualiser l’environnement.
- **3 machines virtuelles Windows Server 2019** pour DC1, DC2 et RODC.
- **1 machine virtuelle Windows 10/11** pour le client.

---

### **3. Configuration des VLANs et Réseaux**

| Composant            | VLAN   | Réseau             | Adresse IP             | Plage DHCP (si activée) |
|-----------------------|--------|--------------------|------------------------|-------------------------|
| **pfSense - VLAN 10** | 10     | 192.168.10.0/24    | 192.168.10.1           | N/A                     |
| **DC1**               | 10     | 192.168.10.0/24    | 192.168.10.10          | N/A                     |
| **DC2**               | 10     | 192.168.10.0/24    | 192.168.10.11          | N/A                     |
| **RODC**              | 10     | 192.168.10.0/24    | 192.168.10.12          | N/A                     |
| **Clients Windows**   | 20     | 192.168.20.0/24    | DHCP (192.168.20.x)    | 192.168.20.100-192.168.20.150 |

---

### **4. Schéma Réseau ASCII Corrigé**

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
    +-----+-----+     +------+-----+                +-------------+
    |   DC1      |     |   DC2      |                |  Clients    |
    | 192.168.10.10 |   | 192.168.10.11 |             | DHCP: Dynamic|
    +-------------+     +-------------+              +-------------+
          |                    |
    +-------------+
    |   RODC     |
    | 192.168.10.12|
    +-------------+
```

---

### **5. Étapes Complètes**

#### **5.1 Configuration de pfSense**

1. **Installer pfSense dans VMware** avec deux interfaces :
   - **WAN** : Connectée à Internet (Mode NAT VMware).
   - **LAN** : Interface pour les VLANs.

2. **Configurer les VLANs dans pfSense** :
   - VLAN 10 : Serveurs AD (DC1, DC2, RODC).
   - VLAN 20 : Clients Windows.
   - Associer les VLANs aux sous-réseaux respectifs :
     - VLAN 10 : 192.168.10.0/24.
     - VLAN 20 : 192.168.20.0/24.

3. **Configurer DHCP dans pfSense** :
   - VLAN 10 : Désactiver DHCP (adresses statiques pour les serveurs).
   - VLAN 20 : Activer DHCP (plage : 192.168.20.100-192.168.20.150).

---

#### **5.2 Configuration des Contrôleurs de Domaine**

##### **DC1 (Contrôleur Principal)**

1. **Configurer l’interface réseau** :
   - VLAN : 10.
   - IP statique : 192.168.10.10.

2. **Installer Active Directory Domain Services (AD DS)** :
   - Promouvoir DC1 comme contrôleur principal pour `mondomaine.local`.
   - **Capture requise :** Résumé de la promotion AD DS.

3. **Configurer DNS** :
   - Créer une zone DNS primaire pour `mondomaine.local`.
   - **Capture requise :** Configuration de la zone DNS.

---

##### **DC2 (Contrôleur Secondaire)**

1. **Configurer l’interface réseau** :
   - VLAN : 10.
   - IP statique : 192.168.10.11.

2. **Installer et promouvoir DC2** :
   - Rejoindre le domaine existant `mondomaine.local`.
   - Synchroniser les zones DNS.
   - **Capture requise :** Synchronisation DNS et promotion AD DS.

3. **Configurer la redondance DHCP** :
   - Activer le basculement DHCP entre DC1 et DC2.
   - **Capture requise :** Configuration du basculement.

---

##### **RODC (Read-Only Domain Controller)**

1. **Configurer l’interface réseau** :
   - VLAN : 10.
   - IP statique : 192.168.10.12.

2. **Promouvoir RODC** :
   - Configurer comme contrôleur en lecture seule.
   - **Capture requise :** Fenêtre de promotion RODC.

---

#### **5.3 Configuration des Clients Windows**

1. **Configurer l’interface réseau** :
   - VLAN : 20.
   - Obtenir une adresse IP via DHCP.

2. **Joindre au domaine** :
   - Rejoindre `mondomaine.local`.
   - **Captures requises :** Joindre au domaine et résultats `ping` vers les DC.

3. **Tests de connectivité** :
   - **Ping** vers DC1, DC2 et RODC.
   - **nslookup** pour valider la résolution DNS.
   - Accéder aux ressources partagées.

---

#### **5.4 Tests de Résilience et Redondance**

1. **Arrêter DC1** :
   - Vérifier que DC2 prend en charge le DHCP.
   - **Capture requise :** Bail DHCP obtenu de DC2.

2. **Vérifier RODC** :
   - Avec DC1 et DC2 hors ligne, vérifier que le RODC authentifie les utilisateurs.
   - **Capture requise :** Résultats de connexion via RODC.

---

### **6. Adresses IP et VLANs**

| Machine               | VLAN   | Adresse IP      | Rôle                          | Type de Configuration |
|------------------------|--------|-----------------|-------------------------------|------------------------|
| **pfSense - LAN**      | 10/20  | 192.168.10.1    | Routeur pour les VLANs        | Statique              |
| **DC1**               | 10     | 192.168.10.10   | Contrôleur Principal          | Statique              |
| **DC2**               | 10     | 192.168.10.11   | Contrôleur Secondaire         | Statique              |
| **RODC**              | 10     | 192.168.10.12   | Contrôleur Lecture Seule      | Statique              |
| **Client Windows**    | 20     | DHCP (dynamique)| Poste utilisateur             | Dynamique             |

---

### **7. Livrables**
- Rapport structuré avec captures d’écran :
  - Promotion AD DS, configuration DNS, redondance DHCP.
  - Tests de connectivité (`ping`, `nslookup`).
  - Validation des VLANs (bail DHCP pour VLAN 20).
- Schéma réseau avec VLANs.

