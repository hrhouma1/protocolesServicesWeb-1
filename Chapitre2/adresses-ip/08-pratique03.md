# Pratique animée par le professeur sur le cloud



---
# Exercice 01
----


Vous devez déterminer les adresses IP valides d'un réseau en fonction des informations suivantes :

- **Classe d'adresse IP :** C (192-223)
- **Bits du réseau :** \_\_\_\_\_ (remplir le nombre de bits du réseau)
- **Bits des hôtes :** \_\_\_\_\_ (remplir le nombre de bits des hôtes)
- **Masque de sous-réseau :** \_\_\_\_\_ (remplir le masque de sous-réseau)

### Calculs à effectuer

1. **Calcul du nombre de réseaux valides :**  
   Nombre de réseaux valides = \_\_\_\_\_

2. **Calcul du nombre d'hôtes valides dans chaque réseau :**  
   Nombre d'hôtes valides dans un réseau = \_\_\_\_\_

### Identification des adresses IP

En utilisant l'adresse réseau `192.0.0.0`, complétez les informations suivantes :

1. **Adresse réseau (Network IP) :**  
   \_\_\_\_\_

2. **Première adresse IP valide (1st Valid IP) :**  
   \_\_\_\_\_

3. **Dernière adresse IP valide (Last Valid IP) :**  
   \_\_\_\_\_

4. **Adresse de broadcast (Broadcast IP) :**  
   \_\_\_\_\_

---

Remplissez les champs manquants avec les valeurs calculées ou déterminées selon les étapes ci-dessus. Cet exercice vous permet de mieux comprendre la répartition des adresses IP dans un réseau de classe C.

----
# Correction de l'exercice 01:
----

### 1. **Déterminer la Classe de l'Adresse IP**
   - La classe C couvre les adresses IP allant de `192.0.0.0` à `223.255.255.255`.
   - Dans ce cas, l'adresse IP appartient à la classe C.

### 2. **Calculer les Bits du Réseau et des Hôtes**
   - Pour une classe C :
     - Nombre de bits pour le réseau : 24 (8 bits par octet, 3 octets pour le réseau)
     - Nombre de bits pour les hôtes : 8 (1 octet pour les hôtes)
  
### 3. **Déterminer le Masque de Sous-Réseau**
   - Masque par défaut pour une classe C : `255.255.255.0`

### 4. **Calculer le Nombre de Réseaux et d'Hôtes**
   - Nombre de réseaux valides : \(2^{\text{nombre de bits pour le réseau}}\) =  \(2^8 = 256\) (mais les réseaux 0 et 255 ne sont pas utilisés dans certains cas).
   - Nombre d'hôtes valides dans chaque réseau : \(2^{\text{nombre de bits pour les hôtes}} - 2\) (moins l'adresse réseau et l'adresse de broadcast) = \(2^8 - 2 = 254\).

### 5. **Identifier les Adresses IP**
   - **Adresse réseau** (Network IP) : La première adresse IP de la plage, où tous les bits d'hôtes sont à 0. Pour `192.0.0.0`, c'est `192.0.0.0`.
   - **Première adresse IP valide** (1st Valid IP) : L'adresse suivante, soit `192.0.0.1`.
   - **Dernière adresse IP valide** (Last Valid IP) : L'avant-dernière adresse IP avant l'adresse de broadcast. Pour `192.0.0.0`, c'est `192.0.0.254`.
   - **Adresse de broadcast** (Broadcast IP) : L'adresse où tous les bits d'hôtes sont à 1. Pour `192.0.0.0`, c'est `192.0.0.255`.

### Résumé des adresses :
- **Adresse réseau** : `192.0.0.0`
- **Première adresse IP valide** : `192.0.0.1`
- **Dernière adresse IP valide** : `192.0.0.254`
- **Adresse de broadcast** : `192.0.0.255`

➡️ Cet exercice démontre la détermination des adresses IP valides dans un sous-réseau de classe C.

---
---
---
---
# Exercice 2  : Scénario de Création et Analyse de Sous-Réseaux
---

**Contexte :**
Vous êtes l'administrateur réseau d'une entreprise et vous devez organiser un réseau IP de classe C, `192.168.1.0/24`, en plusieurs sous-réseaux pour optimiser l'infrastructure. Le réseau principal sera divisé en deux sous-réseaux : un sous-réseau public pour les services externes et un sous-réseau privé pour les équipements internes.

### Objectif :

1. **Analyser le réseau principal `192.168.1.0/24`** :
   - Identifier et calculer les informations essentielles pour le réseau principal avant la subdivision.

2. **Créer et analyser un sous-réseau public `192.168.1.0/25`** :
   - Effectuer les calculs et identifier les adresses IP valides, l'adresse réseau, l'adresse de broadcast, etc.

3. **Créer et analyser un sous-réseau privé `192.168.1.128/25`** :
   - Effectuer les mêmes calculs pour ce sous-réseau.

# Partie 1 : Réseau Principal `192.168.1.0/24`

#### Informations à compléter :

1. **Classe d'adresse IP :** \_\_\_\_\_
2. **Masque de sous-réseau :** \_\_\_\_\_
3. **Bits du réseau :** \_\_\_\_\_
4. **Bits des hôtes :** \_\_\_\_\_
5. **Nombre d'hôtes valides :** \_\_\_\_\_

#### Calculs à effectuer :

1. **Adresse réseau (Network IP) :** \_\_\_\_\_
2. **Adresse de broadcast (Broadcast IP) :** \_\_\_\_\_

# Partie 2 : Sous-Réseau Public `192.168.1.0/25`

#### Informations à compléter :

1. **Classe d'adresse IP :** \_\_\_\_\_
2. **Masque de sous-réseau :** \_\_\_\_\_
3. **Bits du réseau :** \_\_\_\_\_
4. **Bits des hôtes :** \_\_\_\_\_
5. **Nombre d'hôtes valides :** \_\_\_\_\_

#### Calculs à effectuer :

1. **Adresse réseau (Network IP) :** \_\_\_\_\_
2. **Adresse de broadcast (Broadcast IP) :** \_\_\_\_\_

#### Identification des adresses IP :

1. **Première adresse IP valide (1st Valid IP) :** \_\_\_\_\_
2. **Dernière adresse IP valide (Last Valid IP) :** \_\_\_\_\_

# Partie 3 : Sous-Réseau Privé `192.168.1.128/25`

#### Informations à compléter :

1. **Classe d'adresse IP :** \_\_\_\_\_
2. **Masque de sous-réseau :** \_\_\_\_\_
3. **Bits du réseau :** \_\_\_\_\_
4. **Bits des hôtes :** \_\_\_\_\_
5. **Nombre d'hôtes valides :** \_\_\_\_\_

# Calculs à effectuer :

1. **Adresse réseau (Network IP) :** \_\_\_\_\_
2. **Adresse de broadcast (Broadcast IP) :** \_\_\_\_\_

#### Identification des adresses IP :

1. **Première adresse IP valide (1st Valid IP) :** \_\_\_\_\_
2. **Dernière adresse IP valide (Last Valid IP) :** \_\_\_\_\_

### Questions supplémentaires (Pour chaque sous-réseau) :

1. **Quelle est l'adresse du prochain sous-réseau immédiatement après celui-ci ?**  
   \_\_\_\_\_

2. **Quelle est l'adresse de réseau du sous-réseau immédiatement précédent ?**  
   \_\_\_\_\_

---

**Instructions :** Pour chaque partie, remplissez les champs manquants en suivant les calculs et les méthodes apprises en cours. Cet exercice vous permettra de mieux comprendre comment subdiviser un réseau principal en sous-réseaux plus petits, en identifiant les adresses réseau, les adresses IP valides, et les adresses de broadcast pour chaque sous-réseau.



---
---
---

# Correction EXERCICE 02
----

### Correction Détaillée : Scénario de Création et Analyse de Sous-Réseaux

---

**Contexte :**
Vous devez organiser un réseau IP de classe C, `192.168.1.0/24`, en plusieurs sous-réseaux pour optimiser l'infrastructure. Le réseau principal sera divisé en deux sous-réseaux : un sous-réseau public pour les services externes et un sous-réseau privé pour les équipements internes.

### Partie 1 : Réseau Principal `192.168.1.0/24`

#### Informations Complétées :

1. **Classe d'adresse IP :** Classe C  
   **Justification :** Les adresses IP de classe C se situent entre `192.0.0.0` et `223.255.255.255`. L'adresse `192.168.1.0` fait partie de cette plage.
   
2. **Masque de sous-réseau :** 255.255.255.0  
   **Justification :** Pour un réseau de classe C par défaut, le masque de sous-réseau est de 24 bits (3 octets de 255), ce qui correspond à `11111111.11111111.11111111.00000000` en notation binaire.

3. **Bits du réseau :** 24  
   **Justification :** Un réseau de classe C utilise 24 bits pour identifier le réseau.

4. **Bits des hôtes :** 8  
   **Justification :** Il reste 8 bits pour les hôtes (les 8 derniers bits de l'adresse IP).

5. **Nombre d'hôtes valides :** 254  
   **Justification :** Le nombre d'hôtes est calculé par la formule `2^n - 2`, où `n` est le nombre de bits pour les hôtes. Ici, `2^8 - 2 = 254`. On soustrait 2 pour exclure l'adresse réseau et l'adresse de broadcast.

#### Calculs effectués :

1. **Adresse réseau (Network IP) :** 192.168.1.0  
   **Justification :** L'adresse réseau est la première adresse du réseau où tous les bits des hôtes sont à 0 (`192.168.1.00000000`).

2. **Adresse de broadcast (Broadcast IP) :** 192.168.1.255  
   **Justification :** L'adresse de broadcast est la dernière adresse du réseau où tous les bits des hôtes sont à 1 (`192.168.1.11111111`).

---

### Partie 2 : Sous-Réseau Public `192.168.1.0/25`

#### Informations Complétées :

1. **Classe d'adresse IP :** Classe C  
   **Justification :** Même adresse de classe C, car l'adresse principale `192.168.1.0` est de classe C.

2. **Masque de sous-réseau :** 255.255.255.128  
   **Justification :** Avec un préfixe `/25`, le masque de sous-réseau utilise 25 bits pour le réseau et 7 bits pour les hôtes : `11111111.11111111.11111111.10000000` en binaire.

3. **Bits du réseau :** 25  
   **Justification :** Un préfixe `/25` signifie que 25 bits sont utilisés pour le réseau.

4. **Bits des hôtes :** 7  
   **Justification :** Il reste 7 bits pour les hôtes.

5. **Nombre d'hôtes valides :** 126  
   **Justification :** Le nombre d'hôtes est `2^7 - 2 = 126`.

#### Calculs effectués :

1. **Adresse réseau (Network IP) :** 192.168.1.0  
   **Justification :** Pour un sous-réseau `/25`, l'adresse réseau est `192.168.1.00000000`.

2. **Adresse de broadcast (Broadcast IP) :** 192.168.1.127  
   **Justification :** L'adresse de broadcast est `192.168.1.01111111`, soit `192.168.1.127`.

#### Identification des adresses IP :

1. **Première adresse IP valide (1st Valid IP) :** 192.168.1.1  
   **Justification :** La première adresse IP valide est juste après l'adresse réseau : `192.168.1.00000001`.

2. **Dernière adresse IP valide (Last Valid IP) :** 192.168.1.126  
   **Justification :** La dernière adresse IP valide est juste avant l'adresse de broadcast : `192.168.1.01111110`.

---

### Partie 3 : Sous-Réseau Privé `192.168.1.128/25`

#### Informations Complétées :

1. **Classe d'adresse IP :** Classe C  
   **Justification :** L'adresse de base `192.168.1.128` appartient à la classe C.

2. **Masque de sous-réseau :** 255.255.255.128  
   **Justification :** Même masque `/25` que pour le sous-réseau public, car il est également divisé en `/25`.

3. **Bits du réseau :** 25  
   **Justification :** 25 bits pour le réseau comme précédemment.

4. **Bits des hôtes :** 7  
   **Justification :** 7 bits pour les hôtes.

5. **Nombre d'hôtes valides :** 126  
   **Justification :** Calculé de la même manière : `2^7 - 2 = 126`.

#### Calculs effectués :

1. **Adresse réseau (Network IP) :** 192.168.1.128  
   **Justification :** L'adresse réseau est `192.168.1.10000000` en binaire.

2. **Adresse de broadcast (Broadcast IP) :** 192.168.1.255  
   **Justification :** L'adresse de broadcast est `192.168.1.11111111`, soit `192.168.1.255`.

#### Identification des adresses IP :

1. **Première adresse IP valide (1st Valid IP) :** 192.168.1.129  
   **Justification :** La première adresse IP valide est juste après l'adresse réseau : `192.168.1.10000001`.

2. **Dernière adresse IP valide (Last Valid IP) :** 192.168.1.254  
   **Justification :** La dernière adresse IP valide est juste avant l'adresse de broadcast : `192.168.1.11111110`.

---

### Questions supplémentaires (Pour chaque sous-réseau) :

1. **Quelle est l'adresse du prochain sous-réseau immédiatement après celui-ci ?**

   - **Sous-réseau public `192.168.1.0/25` :**  
     `192.168.1.128`  
     **Justification :** Le prochain sous-réseau commence à `192.168.1.128`, soit l'adresse réseau du sous-réseau privé.

   - **Sous-réseau privé `192.168.1.128/25` :**  
     Il n'y a pas de sous-réseau supplémentaire directement après celui-ci dans le cadre du réseau `192.168.1.0/24`.

2. **Quelle est l'adresse de réseau du sous-réseau immédiatement précédent ?**

   - **Sous-réseau privé `192.168.1.128/25` :**  
     `192.168.1.0`  
     **Justification :** L'adresse de réseau du sous-réseau public précédent est `192.168.1.0`.


---
---
---
---

# Exercice 3
----
## Animée par le professeur en classe 
