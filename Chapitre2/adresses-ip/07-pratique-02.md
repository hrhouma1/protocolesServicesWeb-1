-----
# Exercice 1 : Identification des Classes et Plages d'Adresses IP Privées
-----

- **Objectif** : Identifier la classe et calculer la plage d'adresses IP pour les réseaux privés.
  
1. Pour le réseau privé `10.0.0.0/8`, identifiez la classe à laquelle il appartient et calculez le nombre total d'adresses IP disponibles dans cette plage.
2. Pour le réseau privé `172.16.0.0/12`, déterminez la plage d'adresses IP, le nombre d'adresses IP utilisables, et identifiez la classe associée à cette adresse IP privée.
3. Pour le réseau privé `192.168.1.0/24`, calculez la plage d'adresses IP et identifiez la classe de cette adresse.
-----
# Exercice 2 : Comparaison des Plages d'Adresses IP Privées
-----

- **Objectif** : Comparer différentes plages d'adresses IP privées en termes de taille de réseau et d'adresses IP disponibles.

1. Comparez les plages d'adresses IP des réseaux privés suivants : `10.0.0.0/8`, `172.16.0.0/12`, et `192.168.0.0/16`.
2. Pour chaque plage d'adresses IP, calculez le nombre total d'adresses IP disponibles et analysez les différences entre ces réseaux en termes de taille et de nombre d'adresses utilisables.

-----
# Exercice 3 - CIDR
-----

1. Calculer la plage d'adresses IP pour le réseau 192.168.10.0/23.
2. Déterminer le masque de sous-réseau et l'adresse de diffusion pour 172.16.50.0/22.
3. Trouver le nombre d'adresses IP utilisables dans le réseau 10.0.0.0/16.
4. Diviser le réseau 192.168.0.0/24 en 4 sous-réseaux égaux. Indiquer le CIDR et la plage d'adresses pour chaque sous-réseau.
5. Convertir le masque de sous-réseau 255.255.240.0 en notation CIDR.

## 3.1.Vérification des résultats

Pour vérifier vos calculs, utilisez la calculatrice CIDR d'iplocate :
https://www.iplocate.com/fr/tools/cidr-calculator
Cette calculatrice est recommandée pour sa fiabilité et sa complétude. Elle fournit des informations détaillées sur le masque wildcard, l'adresse de broadcast, le réseau CIDR et la plage des adresses IP utilisables.

## 3.2.Méthodologie de travail

1. Résoudre les exercices manuellement en appliquant les concepts théoriques.
2. Utiliser la calculatrice CIDR d'iplocate pour vérifier les résultats obtenus.
3. Comparer les réponses calculées manuellement avec celles fournies par l'outil.
4. Analyser les éventuelles différences et comprendre leur origine.



---
# Correction de l'exercice 01
----

### Exercice 1 : Identification des Classes et Plages d'Adresses IP Privées

#### Objectif :
L'objectif de cet exercice est de vous aider à identifier les classes d'adresses IP et à calculer les plages d'adresses IP pour différents réseaux privés. Les réseaux privés sont des blocs d'adresses IP réservées pour une utilisation au sein de réseaux locaux (LAN). Les trois réseaux privés communs que nous allons examiner sont 10.0.0.0/8, 172.16.0.0/12, et 192.168.1.0/24.

### 1. Réseau privé 10.0.0.0/8
#### a) Identification de la classe

- **Adresse IP** : 10.0.0.0
- **Masque de sous-réseau** : /8, soit 255.0.0.0

**Classe :**  
L'adresse 10.0.0.0 appartient à la **classe A**.  
En IPv4, les adresses IP de classe A vont de 1.0.0.0 à 126.0.0.0. La première octet de l'adresse IP (le plus à gauche) détermine la classe :
- **Classe A** : 1.0.0.0 à 126.255.255.255  
L'adresse IP 10.0.0.0 se situe dans cette plage, donc elle est de classe A.

#### b) Calcul de la plage d'adresses IP

- **Adresse de début :** 10.0.0.0  
  C'est l'adresse de réseau, donc c'est la première adresse du réseau.

- **Adresse de fin :** 10.255.255.255  
  Pour trouver l'adresse de fin, nous devons mettre tous les bits d'hôte à 1. Le masque de sous-réseau étant /8, cela signifie que les 24 bits suivants sont des bits d'hôte. Donc, l'adresse de diffusion pour ce réseau est 10.255.255.255.

**Plage d'adresses :**  
10.0.0.0 à 10.255.255.255

#### c) Nombre total d'adresses IP disponibles

- **Nombre de bits d'hôte :** 32 - 8 = 24 bits
- **Nombre total d'adresses :** 2^24 = 16 777 216 adresses

### 2. Réseau privé 172.16.0.0/12
#### a) Identification de la classe

- **Adresse IP** : 172.16.0.0
- **Masque de sous-réseau** : /12, soit 255.240.0.0

**Classe :**  
L'adresse 172.16.0.0 appartient à la **classe B**.  
Les adresses IP de classe B vont de 128.0.0.0 à 191.255.255.255. Le premier octet de l'adresse IP se situe entre 128 et 191.

- **Classe B** : 128.0.0.0 à 191.255.255.255  
L'adresse IP 172.16.0.0 se situe dans cette plage, donc elle est de classe B.

#### b) Calcul de la plage d'adresses IP

- **Adresse de début :** 172.16.0.0  
  C'est l'adresse de réseau, donc c'est la première adresse du réseau.

- **Adresse de fin :** 172.31.255.255  
  Pour trouver l'adresse de fin, nous devons mettre tous les bits d'hôte à 1. Le masque de sous-réseau étant /12, cela signifie que les 20 bits suivants sont des bits d'hôte. Donc, l'adresse de diffusion pour ce réseau est 172.31.255.255.

**Plage d'adresses :**  
172.16.0.0 à 172.31.255.255

#### c) Nombre total d'adresses IP utilisables

- **Nombre de bits d'hôte :** 32 - 12 = 20 bits
- **Nombre total d'adresses :** 2^20 = 1 048 576 adresses

Cependant, pour calculer les adresses **utilisables** dans un réseau, il faut soustraire 2 (une pour l'adresse de réseau et une pour l'adresse de diffusion) :

- **Adresses utilisables :** 1 048 574 adresses

### 3. Réseau privé 192.168.1.0/24
#### a) Identification de la classe

- **Adresse IP** : 192.168.1.0
- **Masque de sous-réseau** : /24, soit 255.255.255.0

**Classe :**  
L'adresse 192.168.1.0 appartient à la **classe C**.  
Les adresses IP de classe C vont de 192.0.0.0 à 223.255.255.255. Le premier octet de l'adresse IP se situe entre 192 et 223.

- **Classe C** : 192.0.0.0 à 223.255.255.255  
L'adresse IP 192.168.1.0 se situe dans cette plage, donc elle est de classe C.

#### b) Calcul de la plage d'adresses IP

- **Adresse de début :** 192.168.1.0  
  C'est l'adresse de réseau, donc c'est la première adresse du réseau.

- **Adresse de fin :** 192.168.1.255  
  Pour trouver l'adresse de fin, nous devons mettre tous les bits d'hôte à 1. Le masque de sous-réseau étant /24, cela signifie que les 8 bits suivants sont des bits d'hôte. Donc, l'adresse de diffusion pour ce réseau est 192.168.1.255.

**Plage d'adresses :**  
192.168.1.0 à 192.168.1.255

#### c) Nombre total d'adresses IP utilisables

- **Nombre de bits d'hôte :** 32 - 24 = 8 bits
- **Nombre total d'adresses :** 2^8 = 256 adresses

Pour calculer les adresses **utilisables** dans un réseau de classe C :

- **Adresses utilisables :** 256 - 2 = 254 adresses (on soustrait 2 pour l'adresse de réseau et l'adresse de diffusion).

### Conclusion

- **Réseau 10.0.0.0/8 :** Classe A, Plage : 10.0.0.0 à 10.255.255.255, Nombre d'adresses : 16 777 216
- **Réseau 172.16.0.0/12 :** Classe B, Plage : 172.16.0.0 à 172.31.255.255, Nombre d'adresses : 1 048 576, Adresses utilisables : 1 048 574
- **Réseau 192.168.1.0/24 :** Classe C, Plage : 192.168.1.0 à 192.168.1.255, Nombre d'adresses : 256, Adresses utilisables : 254

Ces calculs vous aident à comprendre comment les adresses IP sont organisées dans différentes classes et comment les plages d'adresses sont déterminées pour chaque sous-réseau, ce qui est essentiel pour la gestion efficace des réseaux privés.


---
# Correction de l'exercice 02
----

### Exercice 2 : Comparaison des Plages d'Adresses IP Privées

#### Objectif :
Cet exercice vise à comparer différentes plages d'adresses IP privées en termes de taille de réseau et de nombre total d'adresses IP disponibles. Les réseaux privés en question sont 10.0.0.0/8, 172.16.0.0/12, et 192.168.0.0/16. Cette comparaison permet de mieux comprendre les capacités et les limitations de chaque plage d'adresses IP dans un réseau privé.

### Introduction aux Plages d'Adresses IP Privées
Les adresses IP privées sont utilisées pour l'identification des appareils au sein d'un réseau local (LAN) et ne sont pas routables sur Internet. Les trois plages d'adresses IP privées couramment utilisées sont :

1. **10.0.0.0/8** - Réseau de classe A
2. **172.16.0.0/12** - Réseau de classe B
3. **192.168.0.0/16** - Réseau de classe C

Chaque plage est définie par une adresse réseau et un masque de sous-réseau qui détermine la partie "réseau" et la partie "hôte" de l'adresse IP.

### 1. Réseau 10.0.0.0/8

#### a) Identification de la Plage d'Adresses IP

- **Adresse de réseau :** 10.0.0.0
- **Masque de sous-réseau :** /8, ce qui correspond à 255.0.0.0

Le masque de sous-réseau /8 signifie que les 8 premiers bits de l'adresse IP sont réservés pour identifier le réseau, tandis que les 24 bits restants sont utilisés pour identifier les hôtes au sein de ce réseau. 

#### b) Calcul du Nombre Total d'Adresses IP

- **Bits d'hôte disponibles :** 32 - 8 = 24 bits
- **Nombre total d'adresses IP :** 2^24 = 16 777 216 adresses

**Analyse :**  
Le réseau 10.0.0.0/8 offre un grand espace d'adressage avec **16 777 216** adresses IP, ce qui le rend idéal pour de très grands réseaux privés ou des réseaux d'entreprises.

### 2. Réseau 172.16.0.0/12

#### a) Identification de la Plage d'Adresses IP

- **Adresse de réseau :** 172.16.0.0
- **Masque de sous-réseau :** /12, ce qui correspond à 255.240.0.0

Le masque de sous-réseau /12 signifie que les 12 premiers bits sont utilisés pour identifier le réseau, et les 20 bits restants sont utilisés pour les hôtes.

#### b) Calcul du Nombre Total d'Adresses IP

- **Bits d'hôte disponibles :** 32 - 12 = 20 bits
- **Nombre total d'adresses IP :** 2^20 = 1 048 576 adresses

**Analyse :**  
Le réseau 172.16.0.0/12 offre **1 048 576** adresses IP, ce qui est beaucoup moins que le réseau 10.0.0.0/8, mais suffisant pour des réseaux de taille moyenne à grande.

### 3. Réseau 192.168.0.0/16

#### a) Identification de la Plage d'Adresses IP

- **Adresse de réseau :** 192.168.0.0
- **Masque de sous-réseau :** /16, ce qui correspond à 255.255.0.0

Le masque de sous-réseau /16 signifie que les 16 premiers bits identifient le réseau, et les 16 bits restants sont disponibles pour les hôtes.

#### b) Calcul du Nombre Total d'Adresses IP

- **Bits d'hôte disponibles :** 32 - 16 = 16 bits
- **Nombre total d'adresses IP :** 2^16 = 65 536 adresses

**Analyse :**  
Le réseau 192.168.0.0/16 offre **65 536** adresses IP, ce qui est adapté pour des réseaux de petite à moyenne taille.

### 4. Comparaison des Trois Réseaux

#### a) Taille des Réseaux

- **Réseau 10.0.0.0/8 :** Le plus grand des trois réseaux, capable de supporter des millions d'hôtes.
- **Réseau 172.16.0.0/12 :** Un réseau de taille intermédiaire, capable de supporter plus d'un million d'hôtes.
- **Réseau 192.168.0.0/16 :** Le plus petit des trois, avec une capacité maximale de 65 536 hôtes.

#### b) Comparaison en Nombre d'Adresses IP Disponibles

| Réseau            | Nombre Total d'Adresses IP | Adresses IP Utilisables (Max.) |
|-------------------|---------------------------|--------------------------------|
| 10.0.0.0/8        | 16 777 216                 | 16 777 214 (sans adresse réseau et de diffusion)|
| 172.16.0.0/12     | 1 048 576                  | 1 048 574 (sans adresse réseau et de diffusion)|
| 192.168.0.0/16    | 65 536                     | 65 534 (sans adresse réseau et de diffusion) |

**Analyse :**  
- **Réseau 10.0.0.0/8** est de loin le plus grand, offrant la possibilité d'adresser un nombre immense de machines, ce qui le rend particulièrement adapté pour des réseaux d'entreprise très vastes.
- **Réseau 172.16.0.0/12** est une option viable pour des réseaux de taille moyenne, bien qu'il offre un nombre d'adresses IP beaucoup plus réduit comparé au réseau de classe A.
- **Réseau 192.168.0.0/16** est le plus restreint, destiné à des réseaux plus petits, typiquement utilisés dans des environnements résidentiels ou des petites entreprises.

### Conclusion
- Le réseau 10.0.0.0/8 est idéal pour des infrastructures réseau très étendues grâce à son énorme espace d'adressage.
- Le réseau 172.16.0.0/12 convient à des réseaux moyennement grands, offrant une balance entre une large capacité d'adressage et une gestion relativement simplifiée.
- Le réseau 192.168.0.0/16, bien que le plus limité en termes de taille, reste largement suffisant pour la majorité des petits réseaux domestiques ou d'entreprises avec une configuration simple.

Cette comparaison vous permet de comprendre non seulement la différence en termes de taille entre ces réseaux, mais aussi leur application pratique en fonction des besoins de l'infrastructure réseau. En connaissant le nombre d'adresses IP disponibles dans chaque plage, vous pouvez mieux choisir la plage qui correspond aux besoins de votre réseau spécifique.


---
# Correction de l'exercice 03
----


### Exercice 3 - CIDR : Correction Exhaustive avec Détails et Comparaison en Tables

Cet exercice explore les concepts de CIDR en calculant les plages d'adresses IP, les masques de sous-réseau, en divisant des réseaux en sous-réseaux plus petits, et en incluant des notations binaires et hexadécimales pour une compréhension approfondie. Nous allons structurer la correction en utilisant des tables pour chaque étape.

### 1. Calcul de la Plage d'Adresses IP pour le Réseau 192.168.10.0/23

#### Explication :

- **CIDR :** `/23` signifie que les 23 premiers bits sont réservés pour l'adresse du réseau, et les 9 bits restants sont utilisés pour les hôtes dans ce réseau.
- **Plage d'adresses :** Pour déterminer la plage, nous fixons les 23 premiers bits et faisons varier les 9 derniers bits pour couvrir toutes les adresses possibles dans ce réseau.

#### Table Comparative :

| Paramètre              | Décimal               | Binaire                                      | Hexadécimal       |
|------------------------|-----------------------|----------------------------------------------|-------------------|
| Adresse de réseau      | 192.168.10.0          | 11000000.10101000.00001010.00000000          | C0.A8.0A.00       |
| Masque de sous-réseau  | 255.255.254.0 (/23)   | 11111111.11111111.11111110.00000000          | FF.FF.FE.00       |
| Adresse de début       | 192.168.10.0          | 11000000.10101000.00001010.00000000          | C0.A8.0A.00       |
| Adresse de fin         | 192.168.11.255        | 11000000.10101000.00001011.11111111          | C0.A8.0B.FF       |
| Nombre total d'adresses| 512                   |                                              |                   |
| Adresses utilisables   | 510                   |                                              |                   |

**Détails :**  
- **Nombre d'adresses IP utilisables :** 510 (512 adresses totales moins 2 : l'adresse de réseau et l'adresse de diffusion).
- **Adresse de diffusion :** La dernière adresse IP dans la plage est l'adresse de diffusion (192.168.11.255).

### 2. Détermination du Masque de Sous-Réseau et de l'Adresse de Diffusion pour 172.16.50.0/22

#### Explication :

- **CIDR :** `/22` signifie que les 22 premiers bits sont réservés pour l'adresse réseau, et les 10 bits restants sont pour les hôtes.
- **Plage d'adresses :** En faisant varier les 10 derniers bits, nous trouvons la plage des adresses IP disponibles.

#### Table Comparative :

| Paramètre              | Décimal               | Binaire                                      | Hexadécimal       |
|------------------------|-----------------------|----------------------------------------------|-------------------|
| Adresse de réseau      | 172.16.50.0           | 10101100.00010000.00110010.00000000          | AC.10.32.00       |
| Masque de sous-réseau  | 255.255.252.0 (/22)   | 11111111.11111111.11111100.00000000          | FF.FF.FC.00       |
| Adresse de début       | 172.16.50.0           | 10101100.00010000.00110010.00000000          | AC.10.32.00       |
| Adresse de fin         | 172.16.53.255         | 10101100.00010000.00110101.11111111          | AC.10.35.FF       |
| Nombre total d'adresses| 1024                  |                                              |                   |
| Adresses utilisables   | 1022                  |                                              |                   |

**Détails :**  
- **Nombre d'adresses IP utilisables :** 1022 (1024 adresses totales moins 2).
- **Adresse de diffusion :** 172.16.53.255 (la dernière adresse IP dans la plage).

### 3. Trouver le Nombre d'Adresses IP Utilisables dans le Réseau 10.0.0.0/16

#### Explication :

- **CIDR :** `/16` signifie que les 16 premiers bits sont réservés pour l'adresse réseau, et les 16 bits restants sont pour les hôtes.
- **Plage d'adresses :** Les 16 bits pour les hôtes permettent de couvrir un large éventail d'adresses IP.

#### Table Comparative :

| Paramètre              | Décimal               | Binaire                                      | Hexadécimal       |
|------------------------|-----------------------|----------------------------------------------|-------------------|
| Adresse de réseau      | 10.0.0.0              | 00001010.00000000.00000000.00000000          | 0A.00.00.00       |
| Masque de sous-réseau  | 255.255.0.0 (/16)     | 11111111.11111111.00000000.00000000          | FF.FF.00.00       |
| Adresse de début       | 10.0.0.0              | 00001010.00000000.00000000.00000000          | 0A.00.00.00       |
| Adresse de fin         | 10.0.255.255          | 00001010.00000000.11111111.11111111          | 0A.00.FF.FF       |
| Nombre total d'adresses| 65 536                |                                              |                   |
| Adresses utilisables   | 65 534                |                                              |                   |

**Détails :**  
- **Nombre d'adresses IP utilisables :** 65 534 (65 536 adresses totales moins 2).
- **Adresse de diffusion :** 10.0.255.255.

### 4. Division du Réseau 192.168.0.0/24 en 4 Sous-Réseaux Égaux

#### Explication :

- **CIDR initial :** `/24` signifie que les 24 premiers bits sont réservés pour l'adresse réseau, laissant 8 bits pour les hôtes.
- **Nouveau CIDR :** En divisant en 4 sous-réseaux égaux, nous utilisons `/26`, ce qui laisse 6 bits pour les hôtes dans chaque sous-réseau.

#### Table Comparative pour les Sous-Réseaux :

| Sous-réseau | CIDR             | Plage d'adresses        | Adresse de réseau (Binaire)               | Adresse de diffusion (Binaire)           | Hexadécimal       |
|-------------|------------------|-------------------------|-------------------------------------------|------------------------------------------|-------------------|
| 1           | 192.168.0.0/26   | 192.168.0.0 - 192.168.0.63 | 11000000.10101000.00000000.00000000       | 11000000.10101000.00000000.00111111    | C0.A8.00.00 - C0.A8.00.3F |
| 2           | 192.168.0.64/26  | 192.168.0.64 - 192.168.0.127 | 11000000.10101000.00000000.01000000   | 11000000.10101000.00000000.01111111      | C0.A8.00.40 - C0.A8.00.7F |
| 3           | 192.168.0.128/26 | 192.168.0.128 - 192.168.0.191 | 11000000.10101000.00000000.10000000  | 11000000.10101000.00000000.10111111      | C0.A8.00.80 - C0.A8.00.BF |
| 4           | 192.168.0.192/26 | 192.168.0.192 - 192.168.0.255 | 11000000.10101000.00000000.11000000  | 11000000.10101000.00000000.11111111      | C0.A8.00.C0 - C0.A8.00.FF |

**Détails :**  
- **Nombre d'adresses IP par sous-réseau :** 64 adresses par sous-réseau, avec 62 adresses utilisables (64 - 2).
- **CIDR pour chaque sous-réseau :** `/26`.

### 5. Conversion du Masque de Sous-Réseau 255.255.240.0 en Notation CIDR

#### Explication :

- **Masque décimal :** 255.255.240.0
- **Conversion en binaire :** Le masque de sous-réseau en notation binaire est utilisé pour déterminer le CIDR.

#### Table Comparative :

| Masque de sous-réseau  | Décimal               | Binaire                                      | Hexadécimal       | CIDR |
|------------------------|-----------------------|----------------------------------------------|-------------------|------|
| Masque initial         | 255.255.240.0         | 11111111.11111111.11110000.00000000          | FF.FF.F0.00       | /20  |

**Détails :**  
- **Conversion en CIDR :** Le masque 255.255.240.0 correspond à un CIDR de `/20` car il a 20 bits à 1.

### 3.1. Vérification des Résultats avec la Calculatrice CIDR

Pour vérifier vos calculs, vous pouvez utiliser la calculatrice CIDR d'iplocate. Cela permet de s'assurer que vos calculs manuels sont corrects et de voir les détails supplémentaires comme le masque wildcard et l'adresse de diffusion.

- **Lien de la calculatrice :** [Calculatrice CIDR](https://www.iplocate.com/fr/tools/cidr-calculator)

### 3.2. Méthodologie de Travail

1. **Calcul manuel :** Effectuez les calculs en utilisant les concepts théoriques de CIDR, en vérifiant les étapes avec des notations binaires et hexadécimales.
   
2. **Vérification :** Utilisez la calculatrice CIDR pour comparer les résultats manuels avec ceux générés automatiquement.
   
3. **Analyse :** En cas de différences, examinez les calculs manuels pour comprendre et corriger les erreurs, renforçant ainsi votre compréhension.


---
# calculatrice CIDR - https://www.iplocate.com/fr/tools/cidr-calculator
---
- https://www.iplocate.com/fr/tools/cidr-calculator

  
1. **Adresse IP :**  
   Saisissez votre adresse IP ou celle qui vous est donnée, par exemple : `172.70.110.92`.

2. **Masque du réseau CIDR :**  
   Sélectionnez `128.0.0.0` dans le menu déroulant. Ce masque correspond à un CIDR avec un seul bit de masque activé, ce qui est une configuration très large.

3. **Bits du masque :**  
   Choisissez `1` dans la liste des bits du masque. Cela signifie que seul le premier bit est utilisé pour déterminer le réseau.

4. **Masque wildcard :**  
   Vous verrez automatiquement le masque wildcard correspondant apparaître, soit `127.255.255.255`. Ce masque représente les bits disponibles pour les adresses d'hôtes.

5. **Nombre maximal de sous-réseaux :**  
   Le nombre de sous-réseaux possible sera affiché comme `2147483648`. Vous n'avez rien à saisir ici, mais assurez-vous de noter ce nombre.

6. **Nombre maximal d’adresses :**  
   Le nombre total d'adresses IP disponibles dans ce réseau sera calculé automatiquement : `2147483646`. Cela représente toutes les adresses possibles moins celles réservées pour le réseau et la diffusion.

7. **Réseau CIDR (Route) :**  
   Le réseau de base associé sera `128.0.0.0`. Ce champ est également généré automatiquement.

8. **Réseau : Notation CIDR :**  
   La notation CIDR résultante, `128.0.0.0/1`, sera affichée. Cela signifie que le masque couvre la moitié des adresses IP possibles sur IPv4.

9. **Plage d'adresses CIDR :**  
   La plage d'adresses correspondante, allant de `128.0.0.0` à `255.255.255.255`, sera affichée. Prenez note de cette plage car elle est importante pour comprendre l'étendue du réseau couvert par ce CIDR.

### Ce que les Étudiants Doivent Faire :

- **Entrez les valeurs demandées.**
- **Notez les résultats générés par la calculatrice.**
- **Comparez ces résultats avec les concepts théoriques appris en classe.**
- **Réfléchissez à l'ampleur du réseau couvert par un CIDR aussi large et pourquoi de telles configurations pourraient être utilisées.**

### Objectif de l'Exercice :

Cet exercice vise à vous familiariser avec l'utilisation d'une calculatrice CIDR pour comprendre les concepts de masques de sous-réseau, de plages d'adresses IP, et de l'importance des notations CIDR dans la gestion des réseaux.


----
### Conclusion
----

Cette correction exhaustive, avec une comparaison détaillée à l'aide de tables, permet de mieux comprendre les concepts de CIDR, la gestion des adresses IP, et l'importance de l'utilisation de notations binaires et hexadécimales dans l'analyse des réseaux. Ces compétences sont essentielles pour la conception et la gestion des réseaux IP.
