# TP1 : Configuration de DNS avec Zones Directes et Inversées

# **Objectifs du TP :**
- Comprendre les concepts de base du DNS.
- Configurer une zone de recherche directe et une zone de recherche inversée.
- Vérifier les enregistrements DNS à l'aide de commandes simples.
  
# **Schéma de réseau :**
```
                      +----------------------+
                      | domaine: test.local  |
                      | 192.168.100.0/24     |
                      +----------------------+
                               |
               ---------------------------------
               |                               |
        .-------------                     .------------- 
        |     DC      | (.10)                |     DS      | (.11)
        '-------------'                     '-------------'
                               |
                          [Internet]
```

---

# **Instructions :**
- Vous devez remettre un fichier `.docx` contenant vos réponses avec captures d'écran et commandes exécutées.
- Indiquez le numéro de la question avant chaque capture d'écran et réponse.

---

# **1. Configuration de base des machines (20%)**

##**Étape 1 :**  
- **DC** : Configurer comme le contrôleur de domaine pour le domaine `test.local`.
- **DS** : Servir comme serveur secondaire dans le domaine `test.local`.

**À faire :**
- Lancez la commande **ipconfig /all** sur **DC** et **DS** pour afficher les configurations réseau.

**À remettre :**
- Capture d'écran des résultats de la commande **ipconfig /all** sur **DC** et **DS**.
  
##**Étape 2 :**  
- Testez la connectivité Internet sur **DC** et **DS**.

**À faire :**
- Pinguez l'adresse IP publique `8.8.8.8` depuis **DC** et **DS** pour tester la connectivité réseau.

**À remettre :**
- Capture d'écran de la commande **ping 8.8.8.8** sur **DC** et **DS**.

---

# **2. Installation et configuration DNS sur DC (40%)**

##**Étape 1 : Installation du rôle DNS (10%)**  
- Installez le rôle **DNS** sur **DC** si ce n'est pas déjà fait.

**À faire :**
- Utilisez l'interface graphique pour ajouter le rôle DNS à **DC**.

**À remettre :**
- Capture d'écran de la confirmation que le rôle DNS est installé sur **DC**.

---

##**Étape 2 : Configuration de la zone de recherche directe (10%)**  
- Créez une zone de recherche directe pour le domaine `test.local` sur **DC**.

**À faire :**
- Utilisez la console DNS sur **DC** pour créer une zone de recherche directe avec le nom de domaine `test.local`.

**À remettre :**
- Capture d'écran de la console DNS montrant la zone de recherche directe `test.local`.

---

##**Étape 3 : Configuration de la zone de recherche inversée (10%)**  
- Créez une zone de recherche inversée pour le réseau `192.168.100.x` sur **DC**.

**À faire :**
- Dans la console DNS, ajoutez une nouvelle zone de recherche inversée pour le sous-réseau `192.168.100.0/24`.

**À remettre :**
- Capture d'écran montrant la zone de recherche inversée pour `192.168.100.x`.

---

##**Étape 4 : Ajout d'un enregistrement PTR (10%)**  
- Ajoutez un enregistrement **PTR** pour **DC** dans la zone de recherche inversée.

**À faire :**
- Créez un enregistrement **PTR** correspondant à l'adresse IP de **DC** (`192.168.100.10`) dans la zone inversée.

**À remettre :**
- Capture d'écran montrant l'enregistrement **PTR** dans la console DNS.

---

# **3. Tests DNS et résolutions (40%)**

##**Étape 1 : Test avec nslookup (20%)**  
- Testez votre configuration DNS avec la commande **nslookup**.

**À faire :**
- Utilisez la commande **nslookup test.local** depuis **DC** et **DS** pour vérifier la résolution du domaine.
  
**À remettre :**
- Capture d'écran de la commande **nslookup test.local** depuis **DC** et **DS**.
- Expliquez brièvement le résultat.

---

##**Étape 2 : Test de la résolution inversée (10%)**  
- Testez la résolution de l'adresse IP de **DC**.

**À faire :**
- Utilisez la commande **nslookup 192.168.100.10** depuis **DS** pour vérifier que la résolution inverse fonctionne correctement.

**À remettre :**
- Capture d'écran de **nslookup 192.168.100.10** avec une explication du résultat.

---

##**Étape 3 : Configuration du redirecteur DNS (10%)**  
- Configurez un redirecteur pour les requêtes externes.

**À faire :**
- Dans la console DNS de **DC**, configurez un redirecteur pointant vers `8.8.8.8` pour les résolutions externes.

**À remettre :**
- Capture d'écran montrant que le redirecteur vers `8.8.8.8` est bien configuré.


