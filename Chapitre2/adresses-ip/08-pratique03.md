# Pratique animée par le professeur sur le cloud



---
# Annexe : Énoncé de l'exercice
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
# Correction:
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

Cet exercice démontre la détermination des adresses IP valides dans un sous-réseau de classe C. Si tu as d'autres questions ou besoin de plus d'explications, n'hésite pas à demander !
