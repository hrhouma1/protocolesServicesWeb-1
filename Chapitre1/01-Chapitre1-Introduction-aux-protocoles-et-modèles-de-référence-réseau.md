# Chapitre 1: Introduction aux protocoles et modèles de référence réseau
<a name="table-des-matieres"></a>
## Table des matières

1. [Introduction aux protocoles et modèles de référence réseau](#introduction)
2. [Objectifs du Module](#objectifs)
3. [Processus de communications réseau](#processus-communications)
   1. [Réseaux de tailles diverses](#tailles-reseaux)
   2. [Les réseaux Peer-to-Peer et SOHO](#p2p-soho)
   3. [Les réseaux de taille moyenne à grande](#reseaux-grande-taille)
   4. [Les réseaux mondiaux et l'Internet](#reseaux-mondiaux)
   5. [Communications client-serveur](#client-serveur)
      1. [Les rôles des serveurs](#roles-serveurs)
      2. [Les rôles des clients](#roles-clients)
      3. [Exemples de serveurs courants](#exemples-serveurs)
   6. [Sessions typiques](#sessions-typiques)
      1. [Utilisation du réseau à l'école](#utilisation-ecole)
      2. [Jeux en ligne et réseaux domestiques](#jeux-ligne)
      3. [Consultations médicales et Cloud](#consultations-medicales)
   7. [Identifier le chemin réseau](#chemin-reseau)
      1. [Le rôle des FAI et des PoP](#role-fai)
4. [Protocoles de communication](#protocoles-communication)
   1. [En quoi consistent les protocoles ?](#definition-protocoles)
   2. [Structure de message et encapsulation](#structure-message)
   3. [Suite de protocoles TCP/IP](#suite-protocoles)
      1. [Les protocoles d'application](#protocoles-application)
         1. [DNS et le système de noms de domaine](#dns)
         2. [DHCP et SLAAC pour l'adressage IP](#dhcp-slaac)
         3. [Les protocoles de messagerie électronique](#protocoles-messagerie)
         4. [Les protocoles de transfert de fichiers](#protocoles-transfert-fichiers)
         5. [Les protocoles Web et les services REST](#protocoles-web)
      2. [Les protocoles de transport](#protocoles-transport)
         1. [TCP: Transmission Control Protocol](#tcp)
         2. [UDP: User Datagram Protocol](#udp)
      3. [Les protocoles Internet](#protocoles-internet)
         1. [IPv4 et IPv6](#ipv4-ipv6)
         2. [NAT et la translation d'adresses réseau](#nat)
         3. [ICMP pour les diagnostics de réseau](#icmp)
      4. [Les protocoles de routage](#protocoles-routage)
         1. [OSPF, EIGRP, et BGP](#ospf-eigrp-bgp)
      5. [Les protocoles de couche accès réseau](#couche-acces)
         1. [ARP et les protocoles de liaison de données](#arp)
         2. [Ethernet et WLAN](#ethernet-wlan)
   4. [Modèle de référence OSI](#modele-osi)
      1. [Les sept couches du modèle OSI](#couches-osi)
         1. [Couche 7 - Application](#couche-7)
         2. [Couche 6 - Présentation](#couche-6)
         3. [Couche 5 - Session](#couche-5)
         4. [Couche 4 - Transport](#couche-4)
         5. [Couche 3 - Réseau](#couche-3)
         6. [Couche 2 - Liaison de données](#couche-2)
         7. [Couche 1 - Physique](#couche-1)
   5. [Le modèle de référence TCP/IP](#modele-tcp-ip)
      1. [Les quatre couches du modèle TCP/IP](#couches-tcp-ip)
         1. [Couche 4 - Application](#tcp-ip-couche-4)
         2. [Couche 3 - Transport](#tcp-ip-couche-3)
         3. [Couche 2 - Internet](#tcp-ip-couche-2)
         4. [Couche 1 - Accès réseau](#tcp-ip-couche-1)
5. [L'encapsulation des données](#encapsulation-donnees)
   1. [Segmentation des messages](#segmentation-messages)
      1. [Le processus de segmentation](#processus-segmentation)
      2. [Avantages de la segmentation](#avantages-segmentation)
   2. [Unités de Données du Protocole (PDU)](#unites-donnees)
      1. [Les différents types de PDU](#types-pdu)
      2. [La transformation des PDU à travers les couches](#transformation-pdu)
   3. [Exemple d'Encapsulation](#exemple-encapsulation)
      1. [L'encapsulation du segment TCP](#encapsulation-tcp)
      2. [L'encapsulation du paquet IP](#encapsulation-ip)
   4. [Exemple de Désencapsulation](#exemple-desencapsulation)
      1. [Le processus de désencapsulation](#processus-desencapsulation)
   5. [Travaux pratiques – Présentation de Wireshark](#presentation-wireshark)
      1. [Introduction à Wireshark](#introduction-wireshark)
      2. [Capturer et analyser le trafic réseau](#capturer-analyser)
6. [Récapitulation des protocoles du réseau](#recapitulation)
   1. [Résumé des concepts clés](#resume-concepts)
   2. [Les points essentiels à retenir](#points-essentiels)

---

### <a name="introduction"></a> Introduction aux protocoles et modèles de référence réseau

Ce chapitre est une introduction essentielle aux concepts fondamentaux des réseaux, en se concentrant sur les protocoles et les modèles de référence qui permettent à des dispositifs hétérogènes de communiquer entre eux. Vous comprendrez comment les réseaux sont structurés, les rôles que jouent différents types de réseaux, et les protocoles qui gouvernent ces communications.

[Retour à la Table des matières](#table-des-matieres)

### <a name="objectifs"></a> Objectifs du Module

- **Titre du module:** Protocoles réseau
- **Objectif du Module:** L'objectif principal de ce module est de vous fournir une compréhension approfondie des protocoles réseau et de la manière dont ils permettent d'exploiter pleinement le potentiel des réseaux, qu'il s'agisse de simples réseaux domestiques ou de réseaux mondiaux complexes. Vous apprendrez également comment ces protocoles sont essentiels pour la communication et le transfert de données.

[Retour à la Table des matières](#table-des-matieres)

### <a name="processus-communications"></a> Processus de communications réseau

#### <a name="tailles-reseaux"></a> Réseaux de tailles diverses

Les réseaux peuvent varier considérablement en taille, allant de simples réseaux de deux ordinateurs à des réseaux reliant des millions d'appareils à travers le monde. Ce sous-chapitre explore les différents types de réseaux en fonction de leur taille et leur complexité.

- **Réseaux domestiques**: Typiquement, ces réseaux connectent un nombre limité d'ordinateurs et de dispositifs à internet, offrant des fonctionnalités de base comme la navigation web et l'accès aux e-mails.
- **Réseaux d'entreprise**: Utilisés par les entreprises pour consolider, stocker, et accéder à des informations partagées sur des serveurs réseau. Ces réseaux permettent des communications internes comme le courrier électronique, la messagerie instantanée, et la collaboration entre employés.
- **Réseau Peer-to-Peer (P2P)**: Dans les petites entreprises et les foyers, les ordinateurs peuvent fonctionner à la fois comme serveurs et clients. Ce type de réseau est appelé Peer-to-Peer et ne nécessite pas un serveur dédié.

#### <a name="p2p-soho"></a> Les réseaux Peer-to-Peer et SOHO

Les réseaux P2P et les réseaux SOHO (Small Office/Home Office) sont communs dans les environnements domestiques et les petites entreprises. Un réseau P2P est décentralisé, où chaque ordinateur a un rôle égal. Le réseau SOHO, quant à lui, permet aux petits bureaux et aux bureaux à domicile de se connecter à des réseaux d'entreprise plus grands.

#### <a name="reseaux-grande-taille"></a> Les réseaux de taille moyenne à grande

Les réseaux de taille moyenne à grande sont souvent utilisés par des entreprises plus importantes et des institutions éducatives. Ils peuvent comporter des centaines ou des milliers de périphériques interconnectés sur plusieurs sites. Ce type de réseau requiert des protocoles et des technologies plus avancés pour gérer le volume de données et

 les exigences de sécurité.

#### <a name="reseaux-mondiaux"></a> Les réseaux mondiaux et l'Internet

L'internet est l'exemple le plus connu d'un réseau mondial. Il est composé de nombreux réseaux interconnectés, permettant à des centaines de millions d'ordinateurs de communiquer à travers le globe. Cette section explore comment l'internet est structuré, les rôles des fournisseurs d'accès à internet (FAI), et comment les réseaux locaux se connectent à l'internet.

#### <a name="client-serveur"></a> Communications client-serveur

Les communications client-serveur sont au cœur de la plupart des interactions réseau modernes. Dans ce modèle, les serveurs fournissent des ressources et des services aux clients, qui sont des dispositifs finaux (ordinateurs, smartphones, etc.) demandant des données.

##### <a name="roles-serveurs"></a> Les rôles des serveurs

Les serveurs jouent plusieurs rôles cruciaux sur un réseau, tels que:

- **Serveur de fichiers**: Stocke les fichiers d'entreprise et d'utilisateurs dans un emplacement centralisé. Les clients accèdent à ces fichiers via un logiciel client.
- **Serveur web**: Héberge des sites web et les met à disposition des clients via des navigateurs web.
- **Serveur de messagerie**: Gère l'envoi et la réception de courriels, permettant aux clients d'accéder à leurs messages via des clients de messagerie comme Microsoft Outlook.

##### <a name="roles-clients"></a> Les rôles des clients

Les clients sont les dispositifs finaux qui accèdent aux services fournis par les serveurs. Un client peut être un ordinateur de bureau, un smartphone, une tablette, ou tout autre appareil capable de se connecter à un réseau et d'utiliser des applications client.

##### <a name="exemples-serveurs"></a> Exemples de serveurs courants

Les serveurs web, de fichiers, et de messagerie sont parmi les plus couramment utilisés. Chaque type de serveur utilise des protocoles spécifiques pour fournir ses services aux clients, comme HTTP pour les serveurs web, et SMTP pour les serveurs de messagerie.

#### <a name="sessions-typiques"></a> Sessions typiques

Dans cette section, nous examinerons des exemples typiques de sessions réseau, en montrant comment les données circulent entre les dispositifs et les serveurs.

##### <a name="utilisation-ecole"></a> Utilisation du réseau à l'école

Un étudiant peut se connecter au réseau de son école pour accéder à des ressources d'apprentissage en ligne. Par exemple, Terry utilise son smartphone pour rechercher des informations via le réseau Wi-Fi de l'école. Sa demande de recherche passe par plusieurs étapes, y compris l'encodage, la transmission via différents médias, et le traitement par un moteur de recherche avant que les résultats ne soient renvoyés.

##### <a name="jeux-ligne"></a> Jeux en ligne et réseaux domestiques

Michelle utilise une console de jeu pour jouer en ligne. Son réseau domestique est connecté à internet via un modem câble, et les données du jeu sont transmises rapidement au réseau du fournisseur de jeu grâce à des protocoles de haute vitesse, puis renvoyées sous forme de graphiques et de sons.

##### <a name="consultations-medicales"></a> Consultations médicales et Cloud

Les hôpitaux utilisent de plus en plus des services cloud pour stocker des données médicales sensibles, y compris des radiographies. Lorsqu'un médecin consulte ces images, les données sont chiffrées pour assurer la sécurité pendant leur transfert vers le cloud, où elles sont stockées et accessibles à distance.

#### <a name="chemin-reseau"></a> Identifier le chemin réseau

Les données ne se déplacent pas aléatoirement sur un réseau; elles suivent des chemins bien définis, souvent à travers des infrastructures complexes reliant différents fournisseurs d'accès à internet (FAI).

##### <a name="role-fai"></a> Le rôle des FAI et des PoP

Les FAI mondiaux de niveaux 1 et 2 relient certaines parties du web via des points d'échange internet (IXP). Ces points de présence (PoP) sont essentiels pour connecter les réseaux de grande envergure à l'internet.

[Retour à la Table des matières](#table-des-matieres)

### <a name="protocoles-communication"></a> Protocoles de communication

Les protocoles de communication sont les règles et normes qui définissent comment les données sont échangées sur un réseau. Ces protocoles permettent aux dispositifs de comprendre les messages qu'ils échangent, assurant ainsi une communication efficace et fiable.

#### <a name="definition-protocoles"></a> En quoi consistent les protocoles ?

Un protocole est un ensemble de règles et de conventions qui définissent comment les données sont formatées, transmises, et interprétées sur un réseau. Sans protocole, les dispositifs ne pourraient pas communiquer de manière cohérente.

- **Encodage**: Le processus de transformation des données en une forme qui peut être transmise.
- **Formatage**: L'organisation des données selon une structure préétablie.
- **Encapsulation**: L'enveloppement des données avec des informations de protocole supplémentaires à chaque couche de la pile réseau.

#### <a name="structure-message"></a> Structure de message et encapsulation

Chaque message envoyé sur un réseau doit être structuré selon des règles spécifiques. Cette section explore les principes de base du formatage des messages et du processus d'encapsulation.

- **Encapsulation des messages**: Le processus consistant à encapsuler les données à chaque couche du modèle réseau, ajoutant les en-têtes nécessaires pour la transmission correcte.

#### <a name="suite-protocoles"></a> Suite de protocoles TCP/IP

La suite TCP/IP est le fondement d'internet et des réseaux modernes. Elle comprend un ensemble de protocoles standards ouverts utilisés pour gérer la communication sur un réseau.

##### <a name="protocoles-application"></a> Les protocoles d'application

Les protocoles de la couche d'application gèrent les communications spécifiques à des applications particulières, comme la navigation web ou l'envoi d'e-mails.

###### <a name="dns"></a> DNS et le système de noms de domaine

Le DNS (Domain Name System) est un protocole qui traduit les noms de domaine lisibles par l'homme (comme www.example.com) en adresses IP utilisables par les machines.

###### <a name="dhcp-slaac"></a> DHCP et SLAAC pour l'adressage IP

- **DHCPv4**: Permet d'attribuer dynamiquement des adresses IPv4 à des clients sur le réseau.
- **DHCPv6**: Similaire à DHCPv4 mais pour l'adressage IPv6.
- **SLAAC**: Permet à un périphérique de configurer automatiquement son adresse IPv6 sans avoir besoin d'un serveur DHCPv6.

###### <a name="protocoles-messagerie"></a> Les protocoles de messagerie électronique

- **SMTP**: Permet l'envoi de courriers électroniques entre serveurs de messagerie.
- **POP3**: Permet aux clients de télécharger des e-mails à partir d'un serveur de messagerie.
- **IMAP**: Permet aux clients d'accéder à leurs e-mails directement sur le serveur, tout en maintenant les messages sur le serveur.

###### <a name="protocoles-transfert-fichiers"></a> Les protocoles de transfert de fichiers

- **FTP**: Définit les règles pour le transfert de fichiers entre hôtes sur un réseau.
- **SFTP**: Une version sécurisée de FTP qui chiffre les données lors du transfert.
- **TFTP**: Un protocole simple pour le transfert de fichiers sans connexion, généralement utilisé pour les petites tâches comme le transfert de configurations de routeurs.

###### <a name="protocoles-web"></a> Les protocoles Web et les services REST

- **HTTP**: Le protocole qui permet l'échange de données sur le web.
- **HTTPS**: Une version sécurisée d'HTTP qui chiffre les données échangées.
- **REST**: Un style d'architecture pour les services web, utilisant des API et des requêtes HTTP pour interagir avec les applications.

##### <a name="protocoles-transport"></a> Les protocoles de transport

Les protocoles de transport gèrent la transmission des données entre hôtes sur un réseau.

###### <a name="tcp"></a> TCP: Transmission Control Protocol

TCP est un protocole de transport fiable qui assure la livraison correcte des paquets de données en utilisant des accusés de réception et des retransmissions en cas de perte de paquets.

###### <a name="udp"></a> UDP: User Datagram Protocol

UDP est un protocole de transport non fiable qui ne garantit pas la livraison des paquets, mais offre des avantages en termes de vitesse pour certaines applications comme la diffusion de vidéos en direct.

##### <a name="protocoles-internet"></a> Les protocoles Internet

Les protocoles de la couche Internet sont responsables de l'acheminement des paquets de données sur un réseau.

###### <a name="ipv4-ipv6"></a> IPv4 et IPv6

- **IPv4**: Utilise une adresse 32 bits pour identifier les dispositifs sur un réseau.
- **IPv6**: Le successeur d'IPv4, utilisant une adresse 128 bits pour résoudre le problème d'épuisement des adresses IPv4.

###### <a name="nat"></a> NAT et la translation d'adresses réseau

Le NAT (Network Address Translation) permet de masquer plusieurs adresses IP privées derrière une seule adresse IP

 publique, une méthode courante dans les réseaux domestiques et d'entreprise.

###### <a name="icmp"></a> ICMP pour les diagnostics de réseau

ICMP (Internet Control Message Protocol) est utilisé pour diagnostiquer des problèmes de réseau, par exemple en envoyant des messages d'erreur ou en vérifiant la connectivité via des commandes comme `ping`.

##### <a name="protocoles-routage"></a> Les protocoles de routage

Les protocoles de routage déterminent le meilleur chemin pour acheminer les données entre différents réseaux.

###### <a name="ospf-eigrp-bgp"></a> OSPF, EIGRP, et BGP

- **OSPF**: Un protocole de routage interne basé sur l'état des liens, utilisé dans des environnements réseau de grande taille.
- **EIGRP**: Un protocole de routage propriétaire de Cisco, utilisant une métrique composite basée sur plusieurs facteurs.
- **BGP**: Utilisé pour le routage entre fournisseurs de services internet, jouant un rôle clé dans la connectivité globale de l'internet.

##### <a name="couche-acces"></a> Les protocoles de couche accès réseau

Ces protocoles gèrent l'accès aux supports physiques sur lesquels les données sont transmises.

###### <a name="arp"></a> ARP et les protocoles de liaison de données

- **ARP**: Utilisé pour mapper dynamiquement une adresse IP à une adresse MAC (adresse matérielle) sur un réseau local.
- **Ethernet**: Définit les normes de câblage et de signalisation pour les réseaux filaires.
- **WLAN**: Régit les communications sans fil, notamment sur les fréquences 2,4 GHz et 5 GHz.

[Retour à la Table des matières](#table-des-matieres)

### <a name="modele-osi"></a> Modèle de référence OSI

Le modèle OSI (Open Systems Interconnection) est un modèle de référence standard qui décrit les fonctions d'un système de communication en termes de couches abstraites. Ce modèle aide à standardiser les communications réseau et à assurer l'interopérabilité entre les produits et les logiciels de différents fournisseurs.

#### <a name="couches-osi"></a> Les sept couches du modèle OSI

##### <a name="couche-7"></a> Couche 7 - Application

La couche application contient les protocoles utilisés pour les communications de processus à processus. Elle est responsable de l'interface utilisateur et de la gestion des communications de haut niveau.

##### <a name="couche-6"></a> Couche 6 - Présentation

Cette couche assure une représentation commune des données transférées entre les services de la couche application. Elle gère les conversions de format de données, le cryptage/décryptage, et la compression.

##### <a name="couche-5"></a> Couche 5 - Session

La couche session gère les dialogues entre applications, établissant, gérant, et terminant les sessions de communication.

##### <a name="couche-4"></a> Couche 4 - Transport

Cette couche est responsable de la segmentation, du transfert, et du ré-assemblage des données pour les communications entre périphériques finaux. Elle assure également la fiabilité des transmissions grâce à des mécanismes de contrôle de flux et d'erreur.

##### <a name="couche-3"></a> Couche 3 - Réseau

La couche réseau fournit les services nécessaires pour échanger des données sur des réseaux multiples. Elle est responsable de l'adressage logique et du routage des paquets à travers le réseau.

##### <a name="couche-2"></a> Couche 2 - Liaison de données

Cette couche gère l'échange de trames de données entre des périphériques sur un support commun, assurant la détection et la correction des erreurs.

##### <a name="couche-1"></a> Couche 1 - Physique

La couche physique décrit les moyens mécaniques, électriques, fonctionnels et méthodologiques pour activer, gérer, et désactiver les connexions physiques pour la transmission de bits sur le support.

[Retour à la Table des matières](#table-des-matieres)

### <a name="modele-tcp-ip"></a> Le modèle de référence TCP/IP

Le modèle TCP/IP est souvent appelé modèle Internet. Il décrit les fonctions des protocoles au sein de la suite TCP/IP et est utilisé comme modèle de référence pour l'interopérabilité des réseaux.

#### <a name="couches-tcp-ip"></a> Les quatre couches du modèle TCP/IP

##### <a name="tcp-ip-couche-4"></a> Couche 4 - Application

Cette couche est similaire à la couche application du modèle OSI et est responsable de l'interface utilisateur ainsi que de la gestion des communications de haut niveau.

##### <a name="tcp-ip-couche-3"></a> Couche 3 - Transport

Elle prend en charge la communication fiable entre des dispositifs sur différents réseaux, en utilisant des protocoles comme TCP et UDP.

##### <a name="tcp-ip-couche-2"></a> Couche 2 - Internet

Cette couche détermine le meilleur chemin à travers le réseau pour acheminer les données.

##### <a name="tcp-ip-couche-1"></a> Couche 1 - Accès réseau

Elle contrôle les périphériques matériels et les supports qui constituent le réseau, incluant la gestion de l'accès au support physique.

[Retour à la Table des matières](#table-des-matieres)

### <a name="encapsulation-donnees"></a> L'encapsulation des données

L'encapsulation est un processus essentiel dans les communications réseau, où les données sont emballées avec les informations nécessaires à chaque couche pour la transmission sur le réseau.

#### <a name="segmentation-messages"></a> Segmentation des messages

##### <a name="processus-segmentation"></a> Le processus de segmentation

La segmentation consiste à diviser un flux de données en unités plus petites, appelées segments, avant leur transmission. Cela est crucial pour garantir que les données peuvent être envoyées même si une liaison dans le réseau échoue.

##### <a name="avantages-segmentation"></a> Avantages de la segmentation

- **Augmente la vitesse**: La segmentation permet d'envoyer de grandes quantités de données sur le réseau sans saturer les liaisons, en multipliant les conversations simultanées, un processus connu sous le nom de multiplexage.
- **Augmente l'efficacité**: Si un segment ne parvient pas à atteindre sa destination, seul ce segment doit être retransmis, au lieu de renvoyer l'intégralité du flux de données.

#### <a name="unites-donnees"></a> Unités de Données du Protocole (PDU)

Les PDU (Protocol Data Units) sont les différentes formes que prennent les données à chaque couche de la pile réseau.

##### <a name="types-pdu"></a> Les différents types de PDU

- **Données**: Le terme générique pour les PDU à la couche application.
- **Segment**: Le PDU de la couche transport.
- **Paquet**: Le PDU de la couche réseau.
- **Trame**: Le PDU de la couche liaison de données.
- **Bits**: Le PDU utilisé à la couche physique pour la transmission sur le support.

##### <a name="transformation-pdu"></a> La transformation des PDU à travers les couches

Au fur et à mesure que les données descendent la pile de protocoles pour être transmises, elles sont encapsulées dans des PDU de plus en plus spécifiques, adaptés à chaque couche.

#### <a name="exemple-encapsulation"></a> Exemple d'Encapsulation

##### <a name="encapsulation-tcp"></a> L'encapsulation du segment TCP

Lors de l'encapsulation à la couche transport, les données sont emballées dans un segment TCP, qui inclut des informations sur les ports source et destination, le numéro de séquence, et d'autres informations essentielles à la communication.

##### <a name="encapsulation-ip"></a> L'encapsulation du paquet IP

Le segment TCP est ensuite encapsulé dans un paquet IP, avec des informations sur les adresses IP source et destination, la durée de vie du paquet (TTL), et d'autres en-têtes nécessaires pour le routage sur le réseau.

#### <a name="exemple-desencapsulation"></a> Exemple de Désencapsulation

##### <a name="processus-desencapsulation"></a> Le processus de désencapsulation

Le processus de désencapsulation est l'inverse de l'encapsulation. À mesure que les données remontent la pile de protocoles, chaque en-tête est retiré et les données sont réassemblées pour être interprétées par l'application de l'utilisateur final.

#### <a name="presentation-wireshark"></a> Travaux pratiques – Présentation de Wireshark

##### <a name="introduction-wireshark"></a> Introduction à Wireshark

Wireshark est un analyseur de protocoles (ou analyseur de paquets) utilisé pour dépanner les réseaux, effectuer des analyses, développer des logiciels et des protocoles, et s'informer sur les communications réseau.

##### <a name="capturer-analyser"></a> Capturer et analyser le trafic réseau

Dans cette section pratique, vous apprendrez à capturer des paquets sur un réseau à l'aide de Wireshark et à analyser les informations encapsulées dans les PDU pour comprendre le fonctionnement des protocoles réseau en temps réel.

[Retour à la Table des matières](#table-des-matieres)

### <a name="recapitulation"></a> Récapitulation des protocoles du réseau

#### <a name="resume-concepts"></a> Résumé des concepts clés

Ce module a couvert un large éventail de concepts essentiels pour comprendre les protocoles et les modèles de référence réseau, y compris les types de réseaux, les modèles OSI et TCP/IP, la segmentation, l'encapsulation, et l'utilisation d'outils d'analyse comme Wireshark.

#### <a name="points-essentiels"></a> Les points essentiels à retenir

- **Les réseaux existent dans toutes les tailles**, des réseaux domestiques aux réseaux mondiaux.
- **Les serveurs et les clients** jouent des rôles spécifiques dans les communications réseau, avec des serveurs fournissant des services et des clients accédant à ces services.
- **Les protocoles réseau** définissent les règles de communication, y compris l'encodage, le formatage, l'encapsulation, et la remise des messages.
- **Les modèles OSI et TCP/IP** fournissent des cadres de référence standardisés pour comprendre et développer les communications réseau.
- **L'encapsulation et la désencapsulation** sont des processus clés pour la transmission et la réception des données sur un réseau.
- **Wireshark** est un outil puissant pour analyser les communications réseau en capturant et en décodant les PDU.

[Retour à la Table des matières](#table-des-matieres)
