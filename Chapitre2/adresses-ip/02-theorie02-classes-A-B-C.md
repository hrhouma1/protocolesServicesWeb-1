# 01 - Introduction aux adresses IP

Avant de parler de CIDR et de classes, commençons par les bases :

- **Adresse IP** : C'est comme une adresse postale, mais pour les ordinateurs sur un réseau. Chaque appareil connecté à un réseau a une adresse IP, ce qui permet de l'identifier et de communiquer avec d'autres appareils.

Une adresse IP se présente sous la forme de quatre nombres séparés par des points, comme `192.168.1.1`. Chaque nombre peut aller de 0 à 255.

# Le système des classes d'adresses IP (Classe A, B, C)

Au début d'Internet, les adresses IP étaient organisées en classes pour faciliter leur gestion. Il y avait trois classes principales pour les adresses IPv4 :

#### 1. **Classe A**
- **Plage d'adresses** : `0.0.0.0` à `127.255.255.255`
- **Utilisation** : Ces adresses étaient destinées aux très grands réseaux, comme ceux des gouvernements ou des grandes entreprises.
- **Nombre d'hôtes (appareils)** : Une classe A permet de connecter environ 16 millions d'appareils dans un seul réseau.
- **Exemple d'adresse** : `10.0.0.1`
  
Les classes A sont assez rares car elles permettent de connecter tellement d'appareils qu'elles ne sont utilisées que pour des réseaux énormes.

#### 2. **Classe B**
- **Plage d'adresses** : `128.0.0.0` à `191.255.255.255`
- **Utilisation** : Pour les réseaux de taille moyenne, comme ceux des entreprises de taille moyenne ou des campus universitaires.
- **Nombre d'hôtes** : Une classe B permet de connecter environ 65 000 appareils.
- **Exemple d'adresse** : `172.16.0.1`
  
Les classes B sont plus communes et permettent de gérer un réseau plus large qu'une classe C mais plus petit qu'une classe A.

#### 3. **Classe C**
- **Plage d'adresses** : `192.0.0.0` à `223.255.255.255`
- **Utilisation** : Pour les petits réseaux, comme ceux des petites entreprises ou des réseaux domestiques.
- **Nombre d'hôtes** : Une classe C permet de connecter 254 appareils.
- **Exemple d'adresse** : `192.168.1.1`
  
Les classes C sont les plus courantes, surtout pour les réseaux domestiques (comme le réseau Wi-Fi chez toi).

### Problèmes avec les Classes

Le système de classes semblait bien au début, mais il avait des problèmes :

- **Rigidité** : Les classes étaient fixes, donc si tu avais besoin d’un réseau un peu plus grand que ce qu’une classe C pouvait offrir (254 appareils), mais pas assez grand pour une classe B (65 000 appareils), tu te retrouvais avec un gros gaspillage d'adresses.
- **Pénurie d'adresses** : Avec l'explosion d'Internet, les adresses IP ont commencé à manquer, car beaucoup d'entre elles étaient gaspillées par ce système rigide.

