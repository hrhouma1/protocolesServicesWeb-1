--------------------------------------------
# 01 - Configuration initiale du laboratoire
--------------------------------------------

```
          +-------------------+
          | Linux Ubuntu      |
          | Serveur           |
          | IP: 192.168.2.10  |
          | Msk: 255.255.255.0|
          +-------------------+
                  |
                  |
          +-------+-------+
          |   NATNetwork  |
          |   192.168.2.0 |
          +-------+-------+
                  |
     +------------+------------+
     |                         |
+----+----+               +----+----+
| Windows 10|             | Linux    |
| IP:       |             | Ubuntu   |
| 192.168.2.11|           | Desktop  |
| Msk:       |            | IP:      |
| 255.255.255.0|          | 192.168.2.12|
| DNS:       |            | Msk:       |
| 192.168.2.10|           | 255.255.255.0|
|             |           | DNS:       |
|             |           | 192.168.2.10|
+-------------+           +--------------+
```

### Configuration

- **Linux Ubuntu Serveur** : IP 192.168.2.10, masque 255.255.255.0
- **Windows 10** : IP 192.168.2.11, masque 255.255.255.0, DNS 192.168.2.10
- **Linux Ubuntu Desktop** : IP 192.168.2.12, masque 255.255.255.0, DNS 192.168.2.10

Les machines utilisent un réseau NAT avec l'adresse de sous-réseau 192.168.2.x pour permettre la communication entre elles, et le serveur Linux Ubuntu agit comme serveur DNS pour les autres machines du réseau.


--------------------------------------------
# 02 - Installation des machines virtuelles
--------------------------------------------


# Installation d'Ubuntu Server

1. **Télécharger l'image ISO :**
   - Rendez-vous sur le site officiel d'Ubuntu pour télécharger l'image ISO du serveur Ubuntu.

2. **Suivre les instructions d'installation :**
   - Choisissez la langue, configurez le réseau, et entrez un nom d'hôte.
   - Configurez le disque dur (utilisez LVM si souhaité).
   - Sélectionnez les services à installer (SSH, LAMP, etc.).

3. **Finaliser l'installation :**
   - Retirez le support de démarrage et redémarrez le serveur[3][4][5].

# Installation d'Ubuntu Desktop

1. **Télécharger l'image ISO :**
   - Téléchargez l'image ISO d'Ubuntu Desktop depuis le site officiel.

2. **Créer un support de démarrage :**
   - Utilisez Rufus pour créer une clé USB bootable.

3. **Démarrer à partir de la clé USB :**
   - Insérez la clé USB dans votre PC et redémarrez. Sélectionnez la clé USB dans le menu de démarrage.

4. **Suivre les instructions d'installation :**
   - Sélectionnez "Installer Ubuntu".
   - Choisissez votre langue, disposition du clavier, et type d'installation (normal ou minimal).
   - Configurez le partitionnement du disque (effacez tout pour une installation propre).

5. **Finaliser l'installation :**
   - Entrez vos informations utilisateur et terminez l'installation[2][7].

# Installation de Windows 10

1. **Télécharger l'outil de création de média :**
   - Téléchargez l'outil depuis le site officiel de Microsoft.

2. **Créer un support de démarrage :**
   - Utilisez l'outil pour créer une clé USB bootable avec Windows 10.

3. **Démarrer à partir de la clé USB :**
   - Insérez la clé USB dans votre PC et redémarrez. Accédez au BIOS/UEFI pour définir la clé USB comme premier périphérique de démarrage.

4. **Suivre les instructions d'installation :**
   - Sélectionnez "Installer maintenant", entrez votre clé produit si nécessaire.
   - Choisissez "Installation personnalisée" pour une installation propre.
   - Formatez le disque dur si nécessaire et sélectionnez-le pour l'installation.

5. **Finaliser l'installation :**
   - Configurez vos préférences utilisateur après le redémarrage[8][9].

Ces étapes vous guideront à travers l'installation des systèmes d'exploitation sur vos machines respectives. Assurez-vous que vos données sont sauvegardées avant de commencer, car certaines étapes peuvent effacer vos disques existants.






--------------------------------------------
# 03 - Configuration du serveur et des adresses IP serveur
--------------------------------------------


# Étape 1 : Accéder au mode superutilisateur

1. **Se connecter au serveur :**
   - Entrez votre nom d'utilisateur et votre mot de passe.

2. **Passer en mode superutilisateur :**
   ```bash
   sudo -s
   ```

# Étape 2 : Configurer le réseau

1. **Ouvrir le fichier de configuration Netplan :**
   ```bash
   nano /etc/netplan/00-installer-config.yaml
   ```

2. **Modifier le fichier avec les paramètres suivants :**

   ```yaml
   network:
     version: 2
     ethernets:
       enp0s3:
         addresses: [192.168.2.10/24]
         gateway4: 192.168.2.1
         dhcp4: no
         nameservers:
           addresses: [8.8.8.8]
   ```

3. **Enregistrer et quitter l'éditeur Nano :**
   - Appuyez sur `CTRL + X`, puis `Y` pour confirmer, et `Enter` pour enregistrer.

# Étape 3 : Appliquer la configuration réseau

1. **Appliquer les modifications Netplan :**
   ```bash
   netplan apply
   ```

2. **Vérifier la configuration réseau :**
   ```bash
   ip a
   ```

Ces étapes configurent votre serveur Ubuntu avec une adresse IP statique, un gateway, et un serveur DNS, permettant ainsi une connexion réseau stable et fiable.





--------------------------------------------
# 04 - Configuration des adresses IPs au niveau des clients
--------------------------------------------


Pour configurer Ubuntu Desktop et Windows 10 avec des adresses IP statiques, désactiver le pare-feu Windows, et tester la connectivité, suivez ces étapes :

## Configuration d'Ubuntu Desktop

1. **Ouvrir les paramètres réseau :**
   - Cliquez sur l'icône réseau en haut à droite et sélectionnez "Paramètres".

2. **Configurer l'adresse IP statique :**
   - Sélectionnez l'interface réseau (par exemple, "Filaire").
   - Cliquez sur l'icône d'engrenage pour ouvrir les paramètres.
   - Allez dans l'onglet "IPv4".
   - Changez la méthode IPv4 en "Manuel".
   - Entrez les informations suivantes :
     - Adresse : 192.168.2.12
     - Masque de réseau : 255.255.255.0
     - Passerelle : 192.168.2.1
     - DNS : 192.168.2.10, 8.8.8.8
   - Cliquez sur "Appliquer".

## Configuration de Windows 10

1. **Accéder aux paramètres réseau :**
   - Ouvrez le Panneau de configuration > Réseau et Internet > Centre Réseau et partage.
   - Cliquez sur "Modifier les paramètres de la carte".

2. **Configurer l'adresse IP statique :**
   - Faites un clic droit sur la connexion Ethernet et sélectionnez "Propriétés".
   - Double-cliquez sur "Protocole Internet version 4 (TCP/IPv4)".
   - Sélectionnez "Utiliser l'adresse IP suivante" et entrez :
     - Adresse IP : 192.168.2.11
     - Masque de sous-réseau : 255.255.255.0
     - Passerelle par défaut : 192.168.2.1
     - DNS préféré : 192.168.2.10
   - Cliquez sur "OK" pour enregistrer.

3. **Désactiver le pare-feu Windows :**
   - Ouvrez le Panneau de configuration > Système et sécurité > Pare-feu Windows Defender.
   - Cliquez sur "Activer ou désactiver le Pare-feu Windows Defender".
   - Sélectionnez "Désactiver le Pare-feu Windows Defender" pour les réseaux privés et publics.

## Tester la connectivité

1. **Pinguer depuis Ubuntu Desktop :**
   ```bash
   ping -c 3 192.168.2.10  # Vers le serveur Ubuntu
   ping -c 3 192.168.2.11  # Vers Windows 10
   ping -c 3 192.168.2.12  # Vers Ubuntu Desktop (elle-même)
   ```

2. **Pinguer depuis Windows 10 :**
   ```cmd
   ping 192.168.2.10 -n 3  # Vers le serveur Ubuntu
   ping 192.168.2.11 -n 3  # Vers Windows 10 (elle-même)
   ping 192.168.2.12 -n 3  # Vers Ubuntu Desktop
   ```

3. **Pinguer depuis le Serveur Ubuntu :**
   ```bash
   ping -c 3 192.168.2.10  # Vers le serveur Ubuntu (lui-même)
   ping -c 3 192.168.2.11  # Vers Windows 10
   ping -c 3 192.168.2.12  # Vers Ubuntu Desktop
   ```

Ces étapes permettent de configurer les adresses IP statiques, désactiver le pare-feu sur Windows, et vérifier la connectivité entre toutes les machines du réseau en utilisant des tests de ping dans chaque direction possible entre elles.




--------------------------------------------
# 06 - Installation et configuration de Bind9
--------------------------------------------

# Installation de Bind9

1. **Installer Bind9 :**
   ```bash
   apt install bind9
   ```

2. **Supprimer le fichier de configuration local par défaut :**
   ```bash
   rm /etc/bind/named.conf.local
   ```

3. **Créer un nouveau fichier de configuration local :**
   ```bash
   nano /etc/bind/named.conf.local
   ```

4. **Ajouter la configuration de zone :**
   ```plaintext
   zone "ns.local" {
       type master;
       file "/etc/bind/db.ns.local";
   };
   ```

# Configuration de la zone DNS

5. **Copier le fichier de zone par défaut :**
   ```bash
   cp /etc/bind/db.local /etc/bind/db.ns.local
   ```

6. **Éditer le fichier de zone :**
   ```bash
   nano /etc/bind/db.ns.local
   ```
   - Ajoutez les enregistrements DNS nécessaires :
     ```plaintext
     @    IN  SOA ns.local. root.ns.local. (
         604800 ; Refresh
         86400  ; Retry
         2419200; Expire
         604800 ); Negative Cache TTL

     @       IN  NS    ns.local.
     ns      IN  A     192.168.2.10
     www     IN  CNAME ns.local.
     mail    IN  A     192.168.2.10
     ftp     IN  A     192.168.2.10
     smtp    IN  A     192.168.2.10
     pop     IN  A     192.168.2.10
     pop3    IN  A     192.168.2.10
     haythem IN  CNAME ns.local.
     ```

# Vérification et redémarrage

7. **Redémarrer Bind9 :**
   ```bash
   service bind9 restart
   ```

8. **Vérifier le statut de Bind9 :**
   ```bash
   service bind9 status
   ```

9. **Vérifier la configuration de Bind9 :**
   ```bash
   named-checkconf /etc/bind/named.conf
   named-checkzone ns.local /etc/bind/db.ns.local
   ```

# Configuration DNS locale

10. **Installer resolvconf :**
    ```bash
    apt install resolvconf
    ```

11. **Configurer resolvconf :**
    - Éditer `/etc/resolvconf/resolv.conf.d/head` :
      ```plaintext
      nameserver 192.168.2.10
      nameserver 8.8.8.8
      search ns.local
      ```

12. **Redémarrer le serveur :**
    ```bash
    reboot
    ```

Ces étapes vous permettront d'ajouter divers enregistrements DNS, y compris des enregistrements A et CNAME, pour montrer à vos étudiants comment configurer un serveur DNS avec Bind9 sur Ubuntu.




--------------------------------------------
# Étape 7 - Test de Bind9
--------------------------------------------



---------------------------------------------------
# AU NIVEAU DE UBUNTU SERVER
---------------------------------------------------

```bash
nano /etc/resolv.conf
```

Contenu du fichier `/etc/resolv.conf` :

```
# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
# DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
# 127.0.0.53 is the systemd-resolved stub resolver.
# run "systemd-resolve --status" to see details about the actual nameservers.
nameserver 192.168.2.10
nameserver 8.8.8.8
search ns.local
nameserver 127.0.0.53
```

Commandes de test :

1. `ping ns.local`
2. `ping dns.ns.local`
3. `ping pop.ns.local`
4. `ping pop3.ns.local`
5. `ping mail.ns.local`
6. `ping smtp.ns.local`

---------------------------------------------------
# AU NIVEAU DE UBUNTU DESKTOP
---------------------------------------------------

```bash
sudo -s
nano /etc/hosts
```

Contenu du fichier `/etc/hosts` :

```
127.0.0.1       localhost
127.0.1.1       hrehouma-VirtualBox
192.168.2.10    ns.local

# The following lines are desirable for IPv6 capable hosts
::1             ip6-localhost ip6-loopback
fe00::0         ip6-localnet
ff00::0         ip6-mcastprefix
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
```

Commandes de test :

1. `ping ns.local`

```bash
nano /etc/resolv.conf
```

Contenu du fichier `/etc/resolv.conf` :

```
nameserver 192.168.2.10
nameserver 8.8.8.8
search ns.local
```

2. `ping smtp.ns.local`

---------------------------------------------------
# AU NIVEAU DE WINDOWS 10
---------------------------------------------------

Commandes de test :

1. `ping ns.local`
2. `ping imap.ns.local`
3. `nslookup pop.ns.local`


---------------------------------------------------
# AU NIVEAU DE UBUNTU DESKTOP
---------------------------------------------------

Commandes de test :

1. `ping smtp.ns.local`
2. `dig mail.ns.local`



