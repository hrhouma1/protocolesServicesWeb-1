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






--------------------------------------------
# Étape 8 - Confuguration de postfix
--------------------------------------------


Pour configurer Postfix sur un serveur Ubuntu, voici les étapes détaillées, y compris les commentaires pour chaque section du fichier `main.cf` :

## Installation de Postfix

Exécutez la commande suivante pour installer Postfix :

```bash
apt install postfix -y
```

Lors de l'installation, choisissez les options suivantes :
- **Type de configuration du mail** : Internet Site
- **Nom du système de messagerie** : ubuntu

## Configuration du fichier `/etc/postfix/main.cf`

Éditez le fichier de configuration principal :

```bash
nano /etc/postfix/main.cf
```

### Partie 1 : Paramètres de base

```plaintext
# Debian specific: Specifying a file name will cause the first
# line of that file to be used as the name. The Debian default
# is /etc/mailname.
myorigin = /etc/mailname

mydomain = ns.local
home_mailbox = Maildir/
message_size_limit = 52428800

smtp_sasl_auth_enable = yes
smtp_use_tls = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options =
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

smtp_sasl_auth_enable = yes

smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h

readme_directory = no
```

### Partie 2 : Paramètres TLS et compatibilité

```plaintext
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
delay_warning_time = 4h

readme_directory = no

# See http://www.postfix.org/COMPATIBILITY_README.html -- default to 2 on
# fresh installs.
compatibility_level = 2

# TLS parameters
smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
smtpd_tls_security_level=may

smtp_tls_CApath=/etc/ssl/certs
smtp_tls_security_level=may
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache

# See /usr/share/doc/postfix/TLS_README.gz in the postfix-doc package for
# information on enabling SSL in the smtp client.
```

### Partie 3 : Restrictions de relais et réseaux

```plaintext
# TLS parameters (repeated for clarity)
smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
smtpd_tls_security_level=may

smtp_tls_CApath=/etc/ssl/certs
smtp_tls_security_level=may
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache

smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination
myhostname = ubuntu.ns.local
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
mydestination = $myhostname, ubuntu, localhost.localdomain, ns.local, mail.ns.local, localhost
relayhost = [smtp.gmail.com]:587
virtual_alias_maps = hash:/etc/postfix/virtual
mynetworks = 127.0.0.0/8, 192.168.2.0/24
mailbox_size_limit = 0
recipient_delimiter = +
```

## Redémarrage du service Postfix

Après avoir modifié le fichier de configuration, redémarrez le service Postfix pour appliquer les modifications :

```bash
systemctl restart postfix
```

Assurez-vous que le fichier `/etc/postfix/sasl_passwd` est correctement configuré pour l'authentification SMTP si nécessaire.








--------------------------------------------
# Étape 9 - Configuration du RELAIS SMTP pour le service Postfix
--------------------------------------------


- Étapes suivantes pour configurer le relais SMTP dans Postfix sur un serveur Ubuntu :

1. **Édition du fichier `/etc/postfix/sasl_passwd` :**
   - Utilisez la commande :
     ```bash
     nano /etc/postfix/sasl_passwd
     ```
   - Ajoutez la ligne suivante avec vos informations d'identification :
     ```
     [smtp.gmail.com]:587 AdresseMail:MotdePasse
     ```

2. **Création de la base de données de mots de passe :**
   - Exécutez la commande suivante pour créer le fichier de base de données :
     ```bash
     postmap /etc/postfix/sasl_passwd
     ```

Ces étapes permettent de configurer l'authentification SMTP pour l'envoi d'e-mails via Gmail. Assurez-vous de remplacer `AdresseMail` et `MotdePasse` par vos informations réelles.





--------------------------------------------
# Étape 10 - Ajout d'utilisateurs et configuration de relais SMTP avec Postfix sur Ubuntu
--------------------------------------------

---------------------------------------------------
# AU NIVEAU DE UBUNTU SERVER
---------------------------------------------------


**1. Accéder au serveur**  
Connectez-vous à votre serveur Ubuntu via SSH ou ouvrez votre terminal si vous êtes directement connecté à la machine.

**2. Ajouter des utilisateurs**  
Vous devez ajouter des utilisateurs qui auront accès pour envoyer des e-mails via le serveur SMTP. Voici comment ajouter un utilisateur :

```bash
sudo adduser [nom_utilisateur]
```

Remplacez `[nom_utilisateur]` par le nom réel de l'utilisateur. Par exemple :

```bash
sudo adduser lombard
sudo adduser luc
sudo adduser vincent
sudo adduser christelle
sudo adduser arnaud
sudo adduser karine
sudo adduser michel
```

Chaque commande vous demandera de définir un mot de passe et de remplir éventuellement des informations utilisateur supplémentaires.

**3. Installer Postfix**  
Si Postfix n'est pas encore installé, vous pouvez l'installer en exécutant :

```bash
sudo apt update
sudo apt install postfix
```

Lors de l'installation, sélectionnez "Site Internet" et configurez le nom de domaine comme demandé.

**4. Configurer la cartographie virtuelle de Postfix**  
Postfix peut utiliser une cartographie virtuelle pour diriger les e-mails pour différents utilisateurs. Éditez le fichier de cartographie virtuelle :

```bash
sudo nano /etc/postfix/virtual
```

Ajoutez des mappages au format `utilisateur@example.com   nom_utilisateur`, où `utilisateur@example.com` est l'adresse e-mail et `nom_utilisateur` est l'utilisateur local qui recevra les e-mails :

```
lombard@example.com    lombard
luc@example.com        luc
vincent@example.com    vincent
christelle@example.com christelle
arnaud@example.com     arnaud
karine@example.com     karine
michel@example.com     michel
```

Enregistrez et quittez le fichier (`CTRL+X`, puis `Y` et `Entrée`).

**5. Appliquer la configuration de Postfix**  
Pour appliquer la configuration, vous devez hasher les cartographies virtuelles et recharger Postfix :

```bash
sudo postmap /etc/postfix/virtual
sudo systemctl reload postfix
```

**6. Tester la fonctionnalité d'envoi d'e-mails**  
Testez la configuration en envoyant un e-mail de test depuis la ligne de commande ou en utilisant un client de messagerie configuré pour utiliser le serveur.

```bash
echo "Ceci est un e-mail de test." | mail -s "E-mail de test" utilisateur@example.com
```

Remplacez `utilisateur@example.com` par l'adresse e-mail réelle que vous avez configurée.























--------------------------------------------
# Étape 11 - Tests du relais SMTP
--------------------------------------------

---------------------------------------------------
# AU NIVEAU DE UBUNTU SERVER
---------------------------------------------------

## Partie 11.1 

1. `postmap /etc/postfix/virtual`
2. `nano /etc/postfix/`
3. `nano /etc/postfix/master.cf`

Ces commandes sont utilisées dans la configuration de Postfix sur un serveur Ubuntu :

- **`postmap /etc/postfix/virtual`** : Cette commande crée une table de recherche de base de données pour Postfix à partir du fichier de configuration `/etc/postfix/virtual`. Cela est nécessaire pour que Postfix puisse gérer correctement les redirections et les alias email spécifiés dans ce fichier.
  
- **`nano /etc/postfix/`** : Cela semble être une tentative de lancer l'éditeur de texte nano pour modifier les fichiers dans le répertoire `/etc/postfix/`. Toutefois, cette commande est incomplète car elle doit spécifier un fichier à ouvrir, comme `/etc/postfix/main.cf` ou `/etc/postfix/master.cf`.

- **`nano /etc/postfix/master.cf`** : Cette commande ouvre le fichier `master.cf` de Postfix avec l'éditeur nano. Ce fichier de configuration contient divers paramètres qui contrôlent les services de messagerie et les options de livraison de Postfix. Modifier ce fichier permet de personnaliser la façon dont Postfix traite les emails, les connexions réseau, et plus encore.

## Partie 11.2


```plaintext
smtp      inet  n       -       y       -       -       smtpd
submission inet n       -       y       -       -       smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_tls_auth_only=yes
  -o smtpd_reject_unlisted_recipient=no
  -o smtpd_recipient_restrictions=
  -o smtpd_helo_restrictions=
  -o smtpd_sender_restrictions=
  -o smtpd_recipient_restrictions=
  -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
```

Ces lignes configurent deux services dans Postfix :

1. **smtp** - Le service smtpd qui écoute sur le port SMTP standard pour les connexions entrantes.
2. **submission** - Le service smtpd configuré pour écouter sur le port de soumission (587). Ce service est configuré pour utiliser des niveaux de sécurité plus élevés, ce qui est recommandé pour les connexions de soumission où les utilisateurs s'authentifient avant d'envoyer des emails.

Chacune des options `-o` spécifie des paramètres pour le service `submission` :
- `syslog_name=postfix/submission` : Utilise un nom syslog spécifique pour distinguer les logs de ce service.
- `smtpd_tls_security_level=encrypt` : Force l'utilisation de TLS pour ce service.
- `smtpd_sasl_auth_enable=yes` : Active l'authentification SASL.
- `smtpd_tls_auth_only=yes` : N'autorise l'authentification que si TLS est utilisé.
- `smtpd_reject_unlisted_recipient=no` : Ne rejette pas les emails pour les destinataires non listés dans la configuration locale.
- `smtpd_recipient_restrictions`, `smtpd_helo_restrictions`, `smtpd_sender_restrictions` : Défini les restrictions spécifiques pour les destinataires, les commandes HELO et les expéditeurs.
- `smtpd_relay_restrictions=permit_sasl_authenticated,reject` : Permet le relais pour les utilisateurs authentifiés via SASL, rejette tous les autres.



## Résumé

#### Fichier `master.cf` de Postfix

```plaintext
smtp      inet  n       -       y       -       -       smtpd
submission inet n       -       y       -       -       smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_tls_auth_only=yes
  -o smtpd_reject_unlisted_recipient=no
  -o smtpd_recipient_restrictions=
  -o smtpd_helo_restrictions=
  -o smtpd_sender_restrictions=
  -o smtpd_recipient_restrictions=
  -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
```

- Ces lignes spécifient la configuration des services SMTP et Submission pour Postfix. 
- Chaque option `-o` modifie le comportement par défaut du service de soumission pour améliorer la sécurité et gérer l'authentification des utilisateurs.


## Partie 11.3



1. **Redémarrer Postfix en utilisant init.d (méthode traditionnelle) :**
   ```bash
   /etc/init.d/postfix restart
   ```

2. **Redémarrer Postfix en utilisant service (méthode courante sur les systèmes plus anciens) :**
   ```bash
   service postfix restart
   ```

3. **Redémarrer Postfix en utilisant systemctl (recommandé sur les systèmes utilisant systemd) :**
   ```bash
   systemctl restart postfix
   ```

4. **Vérifier le statut de Postfix :**
   ```bash
   service postfix status
   ```








## Partie 11.4 - Tester l'email via la ligne de commandes

```bash
mail -s "Test POSTFIX" votre_adresse@email.com
```

Remplacez `votre_adresse@email.com` par l'adresse e-mail que vous souhaitez tester. Vous pouvez également ajouter un corps au message en entrant le texte après la commande, puis en terminant avec `CTRL-D` pour envoyer. Voici un exemple complet :

```bash
mail -s "Test POSTFIX" votre_adresse@email.com
Ceci est un Test du Service POSTFIX pour le Relais SMTP
CTRL-D
```

Cette commande ouvre une interface dans le terminal pour écrire l'e-mail (sous l'en-tête "Cc:" pour ajouter des destinataires en copie carbone, si nécessaire), et l'email est envoyé à l'adresse spécifiée.
