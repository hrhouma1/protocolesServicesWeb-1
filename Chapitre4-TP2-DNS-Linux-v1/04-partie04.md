
# Histoire de la tâche 4 : 

Après avoir configuré avec succès votre base de données chez **TechFuture-PC**, les responsables techniques vous confient un nouveau défi. Afin d’assurer une connexion réseau stable et sécurisée, vous devez passer votre machine à une **configuration IP statique**.

---

## 📝 Tâche 4 : Configuration Statique du Réseau 📝

### Objectif :
Mettre en place une configuration IP statique sur votre interface réseau pour stabiliser la connexion et garantir une meilleure gestion du réseau.

---

## Étapes de la procédure

### 1. Consultation de la configuration actuelle :
Avant de configurer une adresse IP statique, il est essentiel de connaître la configuration actuelle de votre réseau. Utilisez la commande suivante pour afficher les détails de votre interface réseau :
```bash
ip addr
```

Cette commande vous montrera l'adresse IP actuelle, les interfaces disponibles, et d'autres informations utiles.

---

### 2. Déterminez votre passerelle actuelle :
La passerelle est l'adresse IP du routeur ou de l'appareil par lequel tout le trafic réseau est dirigé pour accéder à des réseaux externes. Utilisez la commande suivante pour identifier l'adresse de votre passerelle par défaut :
```bash
ip route | grep default
```

**Exemple de résultats :**
- **Adresse IP :** `192.168.168.128/24`
- **Passerelle :** `192.168.168.2`

---

### 3. Configuration de l'interface Ethernet :
Maintenant que vous avez collecté les informations nécessaires, il est temps de mettre en place la configuration IP statique. Modifiez le fichier de configuration Netplan, qui gère les interfaces réseau sous Ubuntu.

Ouvrez le fichier de configuration :
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Modifiez le contenu du fichier pour ressembler à ceci, en remplaçant les `??` par vos propres valeurs :
```yaml
network:
  ethernets:
    ens33:
      dhcp4: false
      addresses: [192.168.168.10/24]
      gateway4: 192.168.168.2
      nameservers:
        addresses: [127.0.0.1]
  version: 2
```

**Explication :**
- `dhcp4: false` : Désactive l'attribution d'adresse dynamique via DHCP.
- `addresses` : Indique l'adresse IP statique à attribuer.
- `gateway4` : Définit la passerelle par défaut.
- `nameservers` : Configure le résolveur DNS.

Enregistrez et fermez le fichier en appuyant sur **CTRL + O** puis **CTRL + X**.

---

### 4. Application des modifications :
Pour que les changements prennent effet, vous devez appliquer la nouvelle configuration réseau à l’aide de la commande suivante :
```bash
sudo netplan apply
```

Cela redémarrera le service réseau avec les nouvelles configurations IP statiques.

---

## Questions :
1. **Testez la connectivité internet** et la résolution des noms de domaine après avoir appliqué la configuration statique. Est-ce que tout fonctionne correctement ?
2. **Quelle est la différence entre une adresse IP dynamique et une adresse IP statique ?**
3. **Comment identifier le nom de l'interface réseau** de votre machine ?
4. **Pourquoi est-il important de spécifier une version** dans le fichier de configuration ?

---

## Suite de l’histoire de la tâche 4 : 

Votre configuration IP statique est désormais en place chez **TechFuture-PC**. Cependant, pour des raisons de sécurité et d'isolement, les responsables techniques vous demandent d’ajouter une autre carte réseau **host-only** à votre machine et de configurer un serveur **SSH** pour un accès à distance sécurisé.

---

## 📝 Tâche : Ajout d'une Interface Réseau Host-Only et Configuration du Serveur SSH

### Objectif :
Ajouter une interface réseau **host-only** et configurer un serveur SSH pour accéder à distance à la machine.

---

### 1. Ajout de la carte Host-Only :
L'ajout d'une carte réseau **host-only** se fait généralement via l'interface graphique de votre hyperviseur, comme **VirtualBox** ou **VMware**. Cette carte permet de créer un réseau interne isolé entre la machine hôte et les machines virtuelles.

**Étapes :**
- Dans VirtualBox, accédez aux **Paramètres** de la VM, puis à la section **Réseau**.
- Ajoutez une nouvelle carte réseau et configurez-la en mode **Host-Only**.

---

### 2. Installation du serveur SSH :
Pour permettre la connexion distante à votre machine, installez et configurez un serveur **SSH** :
```bash
sudo apt update
sudo apt install openssh-server
```

---

### 3. Vérification de l'état du service SSH :
Assurez-vous que le service SSH est bien démarré et fonctionne correctement :
```bash
sudo systemctl status ssh
```

---

### 4. Autorisation du service SSH à travers le pare-feu :
Pour autoriser les connexions SSH, ouvrez le port **22** sur le pare-feu en exécutant la commande suivante :
```bash
sudo ufw allow ssh
```

---

### 5. Connexion SSH depuis une machine Windows :
Pour vous connecter à distance depuis une machine Windows, ouvrez une **invite de commande** ou **PowerShell** et utilisez la commande suivante :
```bash
ssh <username>@<IP_address>
```
Remplacez `<username>` par le nom d'utilisateur de votre machine Linux, et `<IP_address>` par l'adresse IP de l'interface **host-only**.

---

## Questions :
1. **Pourquoi est-il utile d'avoir une carte réseau "host-only" ?**
2. **Comment le protocole SSH assure-t-il une connexion sécurisée ?**
3. **Quelles sont les différences entre les interfaces réseau "NAT", "Bridge" et "Host-Only" ?**

---

Avec ces configurations, vous avez réussi à stabiliser votre connexion réseau grâce à l'IP statique, et vous avez également sécurisé l'accès à votre machine via SSH, tout en isolant une partie de votre réseau avec une interface **host-only**.

---

**La prochaine section portera sur la configuration des zones DNS pour TechProg et DataDyn.**
