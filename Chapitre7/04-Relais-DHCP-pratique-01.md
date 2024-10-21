# Relais DHCP sous Windows - pratique

Le **DHCP Relais** est une fonctionnalité clé dans les environnements réseau où les clients DHCP sont répartis sur plusieurs sous-réseaux. Lorsque les clients se trouvent sur un sous-réseau différent de celui du serveur DHCP, le relais DHCP permet à ces clients d’obtenir une adresse IP en relayant leurs requêtes vers le serveur DHCP sur un autre sous-réseau.

#### Objectifs
- Comprendre le rôle du relais DHCP
- Configurer un serveur DHCP avec plusieurs étendues (ranges)
- Configurer un relais DHCP sur un routeur pour relayer les requêtes DHCP

### 1. Comprendre le rôle du DHCP Relais

Dans un environnement de réseau segmenté, les requêtes DHCP (qui sont des diffusions, c'est-à-dire des **broadcasts**) ne traversent pas les routeurs, car ces diffusions sont limitées au sous-réseau local. Le **DHCP Relais** est utilisé pour contourner cette limitation :

1. **Sans relais DHCP** : Un client DHCP sur un sous-réseau différent n’a pas de moyen direct pour contacter le serveur DHCP.
2. **Avec relais DHCP** : Le routeur ou un serveur spécifique joue le rôle de relais, en capturant les requêtes DHCP émises par les clients sur un sous-réseau et en les envoyant au serveur DHCP sur un autre sous-réseau.

Le relais DHCP agit comme un intermédiaire en transmettant les paquets entre le client DHCP et le serveur, permettant au client de recevoir une adresse IP, même s'il n'est pas sur le même réseau que le serveur DHCP.

### 2. Démonstration de configuration d’un DHCP Relais

#### Topologie utilisée
Pour cette démonstration, nous allons simuler deux réseaux :
- **Réseau 1** : 192.168.1.0/24 (serveur DHCP situé sur ce réseau)
- **Réseau 2** : 192.168.2.0/24 (clients sur ce réseau utilisant le relais DHCP)

Le serveur DHCP se trouve sur le **Réseau 1**, tandis que les clients DHCP sont sur le **Réseau 2**. Le relais DHCP sera configuré sur un routeur qui connecte ces deux réseaux.

#### Étape 1 : Création d’une étendue réseau sur le serveur DHCP

**1.1. Lancer la configuration DHCP sur Windows**
- Ouvrez la console DHCP sur le serveur.
- Faites un clic droit sur **IPv4**, puis sélectionnez **Nouvelle étendue**.

**1.2. Ajout d’une étendue pour le réseau 192.168.2.0/24**
- Donnez un nom significatif à l’étendue, par exemple : **Réseau LAN2**.
- Définissez une plage d’adresses IP à attribuer aux clients. Par exemple :
  - Plage d’adresses : 192.168.2.10 à 192.168.2.100
- Excluez les adresses spécifiques si nécessaire (comme les adresses réservées pour les équipements réseau).

**1.3. Définir la durée du bail**
- Par défaut, la durée du bail est fixée à 8 jours. Vous pouvez modifier cette valeur selon vos besoins.

**1.4. Configurer les options du serveur DHCP**
- **Passerelle par défaut** : Entrez l’adresse de la passerelle du réseau 192.168.2.0 (par exemple, l’adresse IP du routeur : 192.168.2.1).
- **Serveur DNS** : Entrez les adresses IP des serveurs DNS à utiliser par les clients.

**1.5. Finalisation**
- Activez l’étendue une fois la configuration terminée. Le serveur DHCP est maintenant prêt à distribuer des adresses IP aux clients du réseau 192.168.2.0.

#### Étape 2 : Configuration du relais DHCP sur un routeur

Pour que les clients du réseau 192.168.2.0 puissent obtenir une adresse IP, il faut configurer le relais DHCP sur le routeur.

**2.1. Ouvrir la console Routage et accès à distance**
- Allez dans **Gestionnaire de serveur**, puis ouvrez la console **Routage et accès à distance**.

**2.2. Ajouter le protocole de relais DHCP**
- Cliquez avec le bouton droit sur **IPv4**, puis sélectionnez **Nouveau protocole de routage**.
- Sélectionnez **DHCP Relay Agent** et cliquez sur **OK**.

**2.3. Ajouter une interface d’écoute**
- Faites un clic droit sur **DHCP Relay Agent** et sélectionnez **Nouveau composant**.
- Choisissez l’interface du réseau où les requêtes DHCP seront reçues (par exemple, l’interface connectée au réseau 192.168.2.0).

**2.4. Ajouter l’adresse du serveur DHCP**
- Cliquez avec le bouton droit sur **DHCP Relay Agent**, puis sélectionnez **Propriétés**.
- Ajoutez l’adresse IP du serveur DHCP (dans notre cas, l’adresse du serveur sur le réseau 192.168.1.0).

Votre relais DHCP est maintenant en place. Toute demande DHCP provenant du réseau 192.168.2.0 sera relayée vers le serveur DHCP sur le réseau 192.168.1.0.

### 3. Tester la configuration

- Connectez un client au réseau 192.168.2.0.
- Configurez ce client pour utiliser DHCP.
- Le client devrait maintenant recevoir une adresse IP du serveur DHCP via le relais. Vous pouvez utiliser la commande `ipconfig` sous Windows ou `ifconfig` sous Linux pour vérifier l’adresse IP attribuée.

### Conclusion

Le **relai DHCP** est une méthode efficace pour gérer la distribution des adresses IP dans des réseaux segmentés sans avoir à déployer un serveur DHCP dans chaque sous-réseau. Grâce à ce système, un seul serveur DHCP peut gérer l’attribution des adresses IP sur plusieurs réseaux distincts.

### Points clés à retenir :
- Le relais DHCP permet de faire transiter les requêtes DHCP entre différents sous-réseaux.
- Il est essentiel dans les réseaux où les clients et le serveur DHCP ne se trouvent pas sur le même sous-réseau.
- La configuration se fait à la fois sur le serveur DHCP (pour ajouter des étendues réseau) et sur le routeur (pour ajouter l’agent de relais DHCP).

