# 01 - Introduction aux adresses IP

Avant de parler de CIDR et de classes, commençons par les bases :

- **Adresse IP** : C'est comme une adresse postale, mais pour les ordinateurs sur un réseau. Chaque appareil connecté à un réseau a une adresse IP, ce qui permet de l'identifier et de communiquer avec d'autres appareils.

Une adresse IP se présente sous la forme de quatre nombres séparés par des points, comme `192.168.1.1`. Chaque nombre peut aller de 0 à 255.

# 02 - Le système des classes d'adresses IP (Classe A, B, C)

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

# 03 - Problèmes avec les Classes

Le système de classes semblait bien au début, mais il avait des problèmes :

- **Rigidité** : Les classes étaient fixes, donc si tu avais besoin d’un réseau un peu plus grand que ce qu’une classe C pouvait offrir (254 appareils), mais pas assez grand pour une classe B (65 000 appareils), tu te retrouvais avec un gros gaspillage d'adresses.
- **Pénurie d'adresses** : Avec l'explosion d'Internet, les adresses IP ont commencé à manquer, car beaucoup d'entre elles étaient gaspillées par ce système rigide.

# ANNEXE 01 - Calcul des appareils connectables avec les classes d'adresses IP

Quand je dis qu'une classe A permet de connecter environ 16 millions d'appareils, je vais expliquer comment on arrive à ce chiffre.

#### 1. **Classe A**
- **Plage d'adresses** : `0.0.0.0` à `127.255.255.255`
- **Masque de sous-réseau par défaut** : `/8` ou `255.0.0.0`
- **Explication** :
  - Le masque de sous-réseau `/8` signifie que les 8 premiers bits de l'adresse IP sont utilisés pour identifier le réseau.
  - Une adresse IP est composée de 32 bits (chaque groupe de chiffres entre les points représente 8 bits, donc 4 x 8 = 32 bits).
  - Si 8 bits sont utilisés pour identifier le réseau, il reste 24 bits pour identifier les appareils (hôtes) dans ce réseau.
  - **Nombre possible d'hôtes** : 2^24 = 16 777 216.
    - **Pourquoi 2^24 ?** : Parce qu'il y a 24 bits disponibles pour les hôtes, chaque bit peut être soit 0 soit 1, donc il y a 2 possibilités par bit. Pour 24 bits, cela donne 2^24 combinaisons possibles.
  - **Pourquoi pas exactement 16 777 216 appareils ?** : En réalité, on ne peut pas utiliser toutes ces adresses parce que :
    - L'adresse avec tous les bits à 0 est réservée pour identifier le réseau lui-même (l'adresse de réseau).
    - L'adresse avec tous les bits à 1 est réservée pour la diffusion (l'adresse de broadcast).
    - **Nombre d'adresses utilisables** : 16 777 216 - 2 = 16 777 214 adresses pour des appareils.

Donc, avec une classe A, tu peux connecter environ 16 millions d'appareils.

#### Pourquoi les classes A sont-elles rares ?
- **Grande taille du réseau** : Chaque réseau de classe A est énorme, avec la possibilité de connecter plus de 16 millions d'appareils. Ces réseaux étaient destinés aux très grandes organisations, comme les gouvernements, les grandes entreprises multinationales, ou les institutions académiques géantes.
- **Gaspillage potentiel** : Si une organisation n'utilise qu'une fraction de ces adresses, une énorme quantité d'adresses IP est gaspillée. Par exemple, si une entreprise utilise seulement 10 000 adresses dans un réseau de classe A, les 16 millions moins 10 000 adresses restantes sont perdues.
- **Attribution limitée** : Initialement, il n'y avait qu'un nombre limité de réseaux de classe A disponibles (128 au total, car les adresses commencent par un bit à 0, donc 2^7 = 128 réseaux possibles). Une fois attribuées, ces adresses n'étaient plus disponibles pour d'autres.

### Classe B
- **Plage d'adresses** : `128.0.0.0` à `191.255.255.255`
- **Masque de sous-réseau par défaut** : `/16` ou `255.255.0.0`
- **Explication** :
  - Le masque `/16` signifie que les 16 premiers bits sont utilisés pour le réseau, et les 16 bits restants pour les hôtes.
  - **Nombre possible d'hôtes** : 2^16 = 65 536.
  - **Nombre d'adresses utilisables** : 65 536 - 2 = 65 534.

### Classe C
- **Plage d'adresses** : `192.0.0.0` à `223.255.255.255`
- **Masque de sous-réseau par défaut** : `/24` ou `255.255.255.0`
- **Explication** :
  - Le masque `/24` signifie que les 24 premiers bits sont utilisés pour le réseau, et les 8 bits restants pour les hôtes.
  - **Nombre possible d'hôtes** : 2^8 = 256.
  - **Nombre d'adresses utilisables** : 256 - 2 = 254.

### Conclusion sur les classes et la transition vers le CIDR

Avec ces détails, on voit pourquoi le système de classes avait des limitations importantes, surtout avec les réseaux de classe A. Ces réseaux étaient surdimensionnés pour beaucoup d'applications, ce qui conduisait à un gaspillage massif d'adresses IP. Le CIDR a été introduit pour permettre une utilisation beaucoup plus flexible et efficace de l'espace d'adressage, en éliminant ces classes fixes et en permettant de créer des réseaux de la taille exacte dont on a besoin. 

Cela a prolongé la durée de vie de l'espace d'adresses IPv4, tout en permettant une meilleure gestion des réseaux, particulièrement importants avec l'augmentation exponentielle du nombre d'appareils connectés à Internet.


# ANNEXE 02 - Table comparative détaillée des classes d'adresses IP

- Je vous présente dans cet annexe des informations nécessaires pour bien comprendre les différences entre les classes A, B, C, D, et E.

| **Caractéristiques**         | **Classe A**                            | **Classe B**                            | **Classe C**                            | **Classe D**                            | **Classe E**                            |
|------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|
| **Plage d'adresses**         | `0.0.0.0` à `127.255.255.255`           | `128.0.0.0` à `191.255.255.255`         | `192.0.0.0` à `223.255.255.255`         | `224.0.0.0` à `239.255.255.255`         | `240.0.0.0` à `255.255.255.255`         |
| **Masque de sous-réseau par défaut** | `/8` (255.0.0.0)                       | `/16` (255.255.0.0)                     | `/24` (255.255.255.0)                   | N/A                                     | N/A                                     |
| **Nombre de bits réseau**    | 8 bits                                  | 16 bits                                 | 24 bits                                 | Pas utilisé pour des réseaux            | Pas utilisé pour des réseaux            |
| **Nombre de bits hôtes**     | 24 bits                                 | 16 bits                                 | 8 bits                                  | Pas utilisé pour des hôtes              | Pas utilisé pour des hôtes              |
| **Nombre maximum de réseaux**| 128 (2^7, le premier bit est fixé)       | 16 384 (2^14, les deux premiers bits sont fixés) | 2 097 152 (2^21, les trois premiers bits sont fixés) | Pas utilisé pour des réseaux            | Pas utilisé pour des réseaux            |
| **Nombre d'hôtes par réseau**| 16 777 214 (`2^24 - 2`)                 | 65 534 (`2^16 - 2`)                     | 254 (`2^8 - 2`)                         | Pas applicable                          | Pas applicable                          |
| **Utilisation**              | Grands réseaux, gouvernement, grandes entreprises | Réseaux de taille moyenne, universités, entreprises de taille moyenne | Petits réseaux, réseaux locaux, résidences | Diffusion multidiffusion (multicast)    | Recherche expérimentale                |
| **Exemple d'adresse**        | `10.0.0.1`                              | `172.16.0.1`                            | `192.168.1.1`                           | `224.0.0.1` (adresse multicast)         | N/A                                     |
| **Particularités**           | - Grand gaspillage si sous-utilisé<br>- Nombre très limité de réseaux disponibles | - Compromis entre taille et nombre de réseaux<br>- Utile pour les grandes institutions | - Convient bien aux petits réseaux<br>- Très utilisé pour les réseaux domestiques | - Utilisé pour envoyer des paquets à plusieurs destinataires simultanément (multicast) | - Réservé pour des utilisations futures ou des expérimentations                |
| **Disponibilité**            | Rarement disponible aujourd'hui         | Modérément disponible                   | Très largement disponible               | Utilisation spécifique                  | Non utilisé publiquement                |

### Explications supplémentaires :

1. **Plage d'adresses** :
   - Chaque classe a une plage d'adresses spécifique basée sur les premiers bits de l'adresse IP.
   - **Classe A** : Les adresses commencent par un bit à 0, donc les adresses vont de `0.0.0.0` à `127.255.255.255`.
   - **Classe B** : Les adresses commencent par deux bits à `10`, donc de `128.0.0.0` à `191.255.255.255`.
   - **Classe C** : Les adresses commencent par trois bits à `110`, donc de `192.0.0.0` à `223.255.255.255`.
   - **Classe D** : Utilisée pour le multicast, commence par `1110`.
   - **Classe E** : Réservée à des fins futures, commence par `1111`.

2. **Masque de sous-réseau** :
   - Le masque de sous-réseau détermine quelle partie de l'adresse IP est utilisée pour identifier le réseau et quelle partie est utilisée pour identifier les hôtes.
   - **Classe A** : 8 bits pour le réseau, 24 bits pour les hôtes.
   - **Classe B** : 16 bits pour le réseau, 16 bits pour les hôtes.
   - **Classe C** : 24 bits pour le réseau, 8 bits pour les hôtes.

3. **Nombre d'hôtes par réseau** :
   - Cela se calcule en fonction du nombre de bits restants après avoir assigné des bits au réseau.
   - **Classe A** : 24 bits pour les hôtes => 2^24 - 2 = 16 777 214 hôtes.
   - **Classe B** : 16 bits pour les hôtes => 2^16 - 2 = 65 534 hôtes.
   - **Classe C** : 8 bits pour les hôtes => 2^8 - 2 = 254 hôtes.

4. **Utilisation spécifique** :
   - **Classe A** : Destinée aux grandes organisations. Par exemple, le réseau `10.0.0.0/8` est souvent utilisé pour les grands réseaux internes.
   - **Classe B** : Typiquement utilisé par les institutions de taille moyenne, telles que les universités.
   - **Classe C** : Très commun pour les réseaux domestiques ou de petites entreprises.
   - **Classe D** : Utilisé pour envoyer des données en multicast, où une seule adresse est utilisée pour envoyer à plusieurs destinataires.
   - **Classe E** : Réservée à des fins futures ou expérimentales, pas utilisée dans la pratique.

5. **Pourquoi certaines classes sont-elles rares ou limitées ?** :
   - **Classe A** : Très peu de réseaux disponibles (seulement 128), ce qui signifie que ces réseaux sont rares et ont été attribués il y a longtemps à de grandes organisations. La rareté provient du fait que chaque réseau de classe A peut gérer un nombre énorme d'appareils, mais ces adresses sont désormais rarement disponibles pour de nouvelles organisations.
   - **Classe B** : Offrait un bon compromis entre taille et nombre d'hôtes, mais est devenue moins commune car le besoin de flexibilité a favorisé l'adoption du CIDR.
   - **Classe C** : La plus flexible pour les petits réseaux, c'est pourquoi elle est très répandue.

Cette table fournit une vue d'ensemble complète qui aide les apprenants à comprendre non seulement les différences entre les classes d'adresses IP, mais aussi pourquoi certaines classes sont plus courantes ou rares que d'autres, en fonction de leur utilité et des limitations historiques.
