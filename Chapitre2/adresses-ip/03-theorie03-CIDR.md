# 01 - Introduction au CIDR (Classless Inter-Domain Routing)

Pour résoudre ces problèmes, une nouvelle méthode a été inventée : **CIDR**.

# 02 - Comment fonctionne le CIDR ?

Le CIDR ne se soucie pas des classes fixes. Il permet de diviser un réseau en plus petits morceaux de manière flexible, selon les besoins exacts.

Au lieu de dire "C'est une classe A, B, ou C", avec le CIDR, tu dis "C'est un réseau qui a besoin de X adresses". Tu utilises un **masque de sous-réseau** pour indiquer la taille du réseau.

- **Adresse de base** : C’est l’adresse IP de départ pour le réseau, par exemple `192.168.1.0`.
- **Masque de sous-réseau** : C’est le nombre après la barre oblique `/`. Par exemple, `/24` signifie que les 24 premiers bits de l'adresse sont utilisés pour identifier le réseau, laissant les 8 derniers bits pour identifier les appareils.

# 03 - Exemple de comparaison entre classes et CIDR

#### Classe C traditionnelle :
- **Adresse** : `192.168.1.0/24`
- **Nombre d'appareils** : 254 appareils
- **Utilisation** : Parfait pour un petit bureau ou un réseau domestique.

#### Avec CIDR :
Imaginons que tu as un bureau plus petit et que tu n’as besoin que de 30 adresses IP pour tes appareils. Avec CIDR, tu peux dire :

- **Adresse** : `192.168.1.0/27`
- **Nombre d'appareils** : 32 adresses IP (30 pour les appareils, 1 pour l'adresse réseau, et 1 pour la diffusion).

Avec CIDR, tu utilises exactement ce dont tu as besoin sans gaspiller d'adresses IP. Le masque de sous-réseau `/27` te donne un réseau plus petit que `/24`, mais suffisamment grand pour ton besoin.

# 04 - Pourquoi CIDR est-il important ?

- **Économie d'adresses IP** : Comme les adresses IP sont limitées, CIDR aide à les utiliser de manière plus efficace.
- **Flexibilité** : Les réseaux peuvent être créés avec une taille adaptée aux besoins réels, plutôt que de forcer à utiliser une taille de classe qui pourrait être trop grande ou trop petite.

# 05 - Conclusion

- **Ancien système (Classes A, B, C)** : Simple à comprendre mais rigide et peu flexible.
- **Nouveau système (CIDR)** : Plus flexible, permet d’économiser les adresses IP et de créer des réseaux de tailles adaptées aux besoins spécifiques.

Le CIDR est largement utilisé aujourd'hui parce qu'il est beaucoup plus efficace pour gérer l'espace d'adressage IP, surtout avec la croissance exponentielle d'Internet et des appareils connectés.
