# 01 - Introduction :
Le CIDR (Classless Inter-Domain Routing) et le système de classes (classe A, B, C, etc.) sont deux méthodes différentes pour attribuer et gérer les adresses IP sur un réseau.

# 02- Système de Classes (Classe A, B, C)

Historiquement, les adresses IP étaient divisées en classes prédéfinies, selon les premiers bits de l'adresse IP :

- **Classe A** :
  - Plage d'adresses : `0.0.0.0` à `127.255.255.255`
  - Masque par défaut : `255.0.0.0` ou `/8`
  - Nombre d'hôtes possibles : ~16 millions par réseau
  - Utilisation : Grands réseaux, souvent pour les grandes organisations ou institutions.

- **Classe B** :
  - Plage d'adresses : `128.0.0.0` à `191.255.255.255`
  - Masque par défaut : `255.255.0.0` ou `/16`
  - Nombre d'hôtes possibles : ~65 000 par réseau
  - Utilisation : Moyennes organisations ou entreprises.

- **Classe C** :
  - Plage d'adresses : `192.0.0.0` à `223.255.255.255`
  - Masque par défaut : `255.255.255.0` ou `/24`
  - Nombre d'hôtes possibles : 254 par réseau
  - Utilisation : Petits réseaux, comme les réseaux locaux (LAN) résidentiels ou de petites entreprises.

### Limitations du Système de Classes
- **Inflexibilité** : Les classes ont des tailles fixes, ce qui conduit souvent à un gaspillage d'adresses IP. Par exemple, si une entreprise a besoin de 300 adresses IP, une classe C serait trop petite, et une classe B serait largement sous-utilisée.
- **Pénurie d'adresses IP** : Avec la croissance d'Internet, le système de classes a rapidement montré ses limites, menaçant de créer une pénurie d'adresses IP disponibles.

# 03 - CIDR (Classless Inter-Domain Routing)

Le CIDR a été introduit pour résoudre ces limitations en offrant une méthode plus flexible et efficace d'attribution des adresses IP. Plutôt que de s'appuyer sur des classes fixes, le CIDR permet de diviser les réseaux en sous-réseaux de tailles variées selon les besoins.

## 03-1 -  Caractéristiques du CIDR :
- **Pas de classes fixes** : Les réseaux peuvent être de n'importe quelle taille, selon le nombre de bits utilisés dans le masque de sous-réseau. Par exemple, un réseau pourrait être `/12`, `/22`, ou même `/30`, selon les besoins spécifiques.
- **Utilisation efficace des adresses IP** : En permettant des tailles de sous-réseau personnalisées, le CIDR réduit le gaspillage d'adresses IP et prolonge la durée de vie de l'espace d'adresses IPv4.
- **Routage plus efficace** : Le CIDR permet de regrouper plusieurs sous-réseaux en une seule entrée de routage, simplifiant ainsi le routage et réduisant la taille des tables de routage sur Internet.

## 03-2 - Exemple Comparatif
- **Classe C** : Avec un masque de `/24`, vous avez 256 adresses, dont 254 utilisables pour des hôtes.
- **CIDR** : Si vous avez besoin de 100 adresses, vous pouvez utiliser un masque `/25` (128 adresses, dont 126 utilisables), ce qui est plus adapté à vos besoins, évitant ainsi de gaspiller 128 adresses.

# 04 - Conclusion

- **Système de classes** : Simple, mais rigide et inefficace pour la gestion fine des adresses IP.
- **CIDR** : Flexible et optimisé pour l'attribution d'adresses, mieux adapté aux besoins modernes des réseaux, notamment dans les environnements dynamiques comme le cloud computing.

- Le CIDR a remplacé le système de classes pour la plupart des utilisations modernes, permettant une gestion plus efficace et une conservation des adresses IP.
