# Relais DHCP - Théorie 

#### 1. Introduction au DHCP

Le **DHCP** (Dynamic Host Configuration Protocol) est un protocole de gestion réseau qui permet à un serveur de distribuer dynamiquement des adresses IP et d'autres informations de configuration réseau aux clients qui s'y connectent. Lorsque des appareils (comme des ordinateurs, smartphones, imprimantes) se connectent à un réseau, ils utilisent DHCP pour obtenir automatiquement une adresse IP, ce qui facilite la gestion des réseaux et réduit le besoin de configuration manuelle.

#### 2. Limitation du DHCP : Le concept de diffusion (broadcast)

Le DHCP utilise des messages de diffusion (**broadcasts**) pour permettre aux clients d’envoyer leurs requêtes de configuration réseau au serveur DHCP. Toutefois, dans les réseaux segmentés, les **diffusions** sont limitées à un sous-réseau particulier. Cela signifie qu'un client sur un sous-réseau ne peut pas directement communiquer avec un serveur DHCP situé sur un autre sous-réseau. C’est là qu’intervient le **relai DHCP**.

#### 3. Qu'est-ce qu'un relais DHCP ?

Un **relais DHCP** est un dispositif réseau ou un service qui permet de relayer les requêtes DHCP entre des clients et des serveurs situés sur des sous-réseaux différents. Il agit comme un intermédiaire entre les clients qui demandent une configuration réseau et le serveur DHCP, en redirigeant les messages de demande (DHCP Discover) depuis les clients vers le serveur DHCP approprié, puis en relayant la réponse (DHCP Offer) du serveur DHCP vers les clients.

#### 4. Fonctionnement du relais DHCP

Voici comment fonctionne le processus du relais DHCP en détail :

1. **Le client DHCP envoie une demande de configuration** : 
   - Lorsque le client se connecte pour la première fois au réseau, il envoie un message de diffusion **DHCP Discover**. Ce message est destiné à être reçu par n’importe quel serveur DHCP disponible sur le réseau local.
   - Cependant, si le serveur DHCP se trouve sur un autre sous-réseau, le message de diffusion ne pourra pas le joindre, car les routeurs (ou commutateurs de niveau 3) ne transmettent pas les diffusions entre sous-réseaux.

2. **Le relai DHCP capture la requête** : 
   - Si un relai DHCP est configuré sur le routeur (ou tout autre appareil jouant le rôle d’intermédiaire), il intercepte le message **DHCP Discover** envoyé par le client.
   - Le relai DHCP encapsule ce message dans un **unicast** (transmission point à point), qui est ensuite envoyé directement au serveur DHCP situé sur un autre sous-réseau.

3. **Le serveur DHCP répond** :
   - Une fois que le serveur DHCP reçoit le message relayé, il traite la demande et prépare une réponse sous forme de **DHCP Offer**, contenant une adresse IP et d'autres paramètres réseau (comme la passerelle, le masque de sous-réseau, le DNS, etc.).
   - Le serveur envoie cette réponse au relai DHCP.

4. **Le relai DHCP transmet la réponse au client** :
   - Le relai DHCP reçoit la réponse du serveur et la retransmet au client sur le sous-réseau d'origine, en utilisant une diffusion pour que le client puisse la recevoir.

5. **L’allocation de l’adresse IP** :
   - Le client reçoit l'offre de configuration réseau (adresse IP, masque de sous-réseau, etc.) du serveur DHCP via le relai, et complète alors le processus de négociation DHCP en envoyant une requête DHCP **Request**.
   - Le serveur finalise l’allocation de l’adresse IP en répondant par un message **DHCP Acknowledge**, toujours transmis via le relai.

#### 5. Pourquoi utiliser un relai DHCP ?

Le relai DHCP est nécessaire dans les situations où il n'est pas pratique ou souhaitable d'avoir un serveur DHCP sur chaque sous-réseau. Quelques raisons d'utiliser un relai DHCP incluent :

- **Centralisation de la gestion** : En utilisant un relai DHCP, un seul serveur DHCP peut gérer plusieurs sous-réseaux. Cela permet de centraliser la gestion des adresses IP et autres paramètres réseau, réduisant ainsi le nombre de serveurs DHCP nécessaires.
- **Facilité de gestion** : Gérer les paramètres réseau à partir d'un seul endroit (un serveur DHCP centralisé) est plus simple et plus efficace que de devoir configurer et surveiller plusieurs serveurs DHCP sur chaque sous-réseau.
- **Économie de ressources** : Installer un serveur DHCP sur chaque sous-réseau peut être coûteux et consommer des ressources. En utilisant un relai DHCP, vous pouvez réduire le besoin de matériel supplémentaire et limiter l'utilisation des ressources.

#### 6. Avantages et inconvénients du relais DHCP

**Avantages :**
- **Centralisation** : Il permet de centraliser la configuration réseau, simplifiant la gestion et la maintenance.
- **Économie de matériel** : Un seul serveur DHCP est nécessaire pour gérer de nombreux sous-réseaux, ce qui réduit les coûts matériels et la complexité.
- **Simplicité de mise en œuvre** : La configuration d'un relai DHCP est relativement simple et ne nécessite pas de grandes modifications dans l'infrastructure réseau existante.

**Inconvénients :**
- **Dépendance** : Si le routeur ou l’appareil jouant le rôle de relais DHCP rencontre un problème, les clients sur d'autres sous-réseaux ne pourront plus obtenir une adresse IP.
- **Performance** : Bien que le trafic DHCP soit généralement léger, il pourrait y avoir un impact sur la latence si le relai DHCP est mal configuré ou s'il y a beaucoup de sous-réseaux à gérer via un seul relais.
- **Complexité dans les grands réseaux** : Pour les très grands réseaux ou ceux avec des besoins spécifiques (exemple : VLANs complexes), la configuration du relais DHCP peut devenir difficile à maintenir si elle n’est pas bien organisée.

#### 7. Cas d'utilisation typique

Imaginez un grand campus universitaire où plusieurs sous-réseaux segmentent les bâtiments (réseau pour les résidences étudiantes, réseau pour les bureaux administratifs, réseau pour les laboratoires, etc.). Il serait fastidieux et coûteux d'installer un serveur DHCP dans chaque bâtiment. Au lieu de cela, le campus peut installer un seul serveur DHCP centralisé qui attribue des adresses IP pour tous les sous-réseaux, avec des relais DHCP configurés sur chaque routeur reliant les sous-réseaux au serveur central.

#### 8. Protocole et structure des messages

Le protocole DHCP, lorsqu’il est relayé, conserve la même structure de messages mais est encapsulé dans des paquets unicast entre le relai et le serveur DHCP. Le **champ 'giaddr'** (Gateway IP Address) dans le message DHCP est utilisé pour indiquer l'adresse IP du relai DHCP, permettant au serveur DHCP de savoir de quel sous-réseau provient la requête et donc quelle étendue d'adresses IP utiliser pour répondre.

#### 9. Alternatives au relai DHCP

Dans certains cas, d’autres solutions peuvent être envisagées pour résoudre le problème de la distribution des adresses IP dans un environnement multi-sous-réseau :
- **DHCP sur chaque sous-réseau** : Cela implique d'installer un serveur DHCP dans chaque sous-réseau, mais cela peut devenir coûteux et difficile à maintenir.
- **Proxy DHCP** : Fonctionne de manière similaire au relai DHCP, mais peut être implémenté avec des fonctionnalités supplémentaires comme le filtrage des requêtes.
- **Utilisation de VLANs et de sous-réseaux plus larges** : Si possible, réduire la segmentation des sous-réseaux peut également limiter le besoin de relais DHCP.

### Conclusion

Le **relai DHCP** est un mécanisme essentiel pour les réseaux segmentés où un serveur DHCP unique doit desservir plusieurs sous-réseaux. Il permet aux clients DHCP de recevoir des adresses IP et autres informations de configuration réseau sans qu’un serveur DHCP soit présent dans chaque sous-réseau. Cela simplifie la gestion des réseaux et permet une plus grande flexibilité dans la manière dont les sous-réseaux sont conçus et administrés.
