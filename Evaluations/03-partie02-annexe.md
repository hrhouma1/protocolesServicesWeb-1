# Annexe : Guide détaillé pour la réalisation du TP1

# **1. Configuration de base des machines (20%)**

**Étape 1 :**  
- Sur **DC** et **DS**, ouvrez une invite de commande (cmd).
- Tapez la commande suivante pour afficher la configuration réseau :

  ```
  ipconfig /all
  ```

**Exemple de sortie :**

```
Configuration IP de Windows

   Nom de l’hôte . . . . . . . . . . : DC
   Adresse IPv4. . . . . . . . . . . : 192.168.100.10
   Masque de sous-réseau. . . . . . : 255.255.255.0
   Passerelle par défaut . . . . . . : 192.168.100.1
```

**Étape 2 :**  
- Pour tester la connectivité réseau, tapez la commande suivante sur **DC** et **DS** :

  ```
  ping 8.8.8.8
  ```

**Exemple de sortie :**

```
Envoi d’une requête 'ping' sur 8.8.8.8 avec 32 octets de données :
Réponse de 8.8.8.8 : octets=32 temps=15 ms TTL=118
```

---

# **2. Installation et configuration DNS sur DC (40%)**

**Étape 1 : Installation du rôle DNS sur DC**

1. Allez dans le **Gestionnaire de serveur** sur **DC**.
2. Cliquez sur **Ajouter des rôles et des fonctionnalités**.
3. Sélectionnez **Rôle DNS** et suivez les instructions pour installer le service.

**Capture attendue :** Une capture d'écran de l'interface indiquant que le rôle DNS est installé avec succès.

---

**Étape 2 : Configuration de la zone de recherche directe**

1. Ouvrez la console **DNS**.
2. Faites un clic droit sur **Zones de recherche directe** > **Nouvelle zone**.
3. Suivez les étapes et entrez **test.local** comme nom de zone.

**Capture attendue :** La zone **test.local** visible dans la console DNS.

---

**Étape 3 : Configuration de la zone de recherche inversée**

1. Toujours dans la console DNS, faites un clic droit sur **Zones de recherche inversée** > **Nouvelle zone**.
2. Suivez les étapes et entrez **192.168.100.0/24** pour la zone inversée.

**Capture attendue :** La zone inversée correspondant à **192.168.100.x**.

---

**Étape 4 : Ajout d'un enregistrement PTR**

1. Allez dans la zone de recherche inversée que vous venez de créer.
2. Faites un clic droit > **Nouvel enregistrement PTR**.
3. Entrez **192.168.100.10** pour **DC**.

**Capture attendue :** L'enregistrement PTR pour **DC** dans la zone de recherche inversée.

---

# **3. Tests DNS et résolutions (40%)**

**Étape 1 : Test avec nslookup**

1. Ouvrez une invite de commande sur **DC** et tapez :

   ```
   nslookup test.local
   ```

**Résultat attendu :**

```
Serveur :  dc.test.local
Address :  192.168.100.10

Nom :    test.local
Address :  192.168.100.10
```

2. Faites la même chose sur **DS** pour vérifier la résolution.

**Capture attendue :** La sortie de **nslookup** montrant la résolution du domaine.

---

**Étape 2 : Test de la résolution inversée**

1. Depuis **DS**, tapez la commande suivante pour vérifier la résolution inversée :

   ```
   nslookup 192.168.100.10
   ```

**Résultat attendu :**

```
Serveur :  dc.test.local
Address :  192.168.100.10

Nom :    dc.test.local
Address :  192.168.100.10
```

**Capture attendue :** La sortie montrant que l'adresse IP a été résolue en **dc.test.local**.

---

**Étape 3 : Configuration du redirecteur DNS**

1. Dans la console DNS de **DC**, faites un clic droit sur **DC** > **Propriétés**.
2. Allez dans l'onglet **Redirecteurs**.
3. Ajoutez l'adresse **8.8.8.8** en tant que redirecteur.

**Capture attendue :** La capture montrant que le redirecteur vers **8.8.8.8** est configuré.

---

## Explications supplémentaires :

### Qu'est-ce que DNS ?
Le **DNS** (Domain Name System) est un service qui permet de convertir des noms de domaine (comme **www.example.com**) en adresses IP que les ordinateurs peuvent comprendre (comme **192.168.1.1**).

### Qu'est-ce qu'une zone de recherche inversée ?
Une **zone de recherche inversée** est une zone DNS qui permet de convertir une adresse IP en nom de domaine (l'inverse de la résolution classique). Cela permet de vérifier à qui appartient une adresse IP.

# Annexe :


#### Table avec les commandes courantes que nous pouvons tester avec **nslookup** sur Windows +  quelques commandes avec **dig** si tu utilises un environnement Windows avec un outil comme WSL (Windows Subsystem for Linux) ou un autre moyen de faire fonctionner des commandes Linux.

---

### Table des commandes **nslookup** :

```
+--------------------------------------+------------------------------------------+
| Commande                             | Description                              |
+--------------------------------------+------------------------------------------+
| nslookup <nom_de_domaine>            | Résout un nom de domaine en adresse IP   |
| nslookup <adresse_IP>                | Résout une adresse IP en nom de domaine  |
| nslookup -type=A <nom_de_domaine>    | Recherche spécifiquement les enregistrements de type A (IPv4)  |
| nslookup -type=AAAA <nom_de_domaine> | Recherche les enregistrements IPv6       |
| nslookup -type=PTR <adresse_IP>      | Recherche inversée (IP vers nom)         |
| nslookup -type=MX <nom_de_domaine>   | Recherche les enregistrements MX (serveur de mail) |
| nslookup -type=NS <nom_de_domaine>   | Recherche les serveurs de noms           |
| nslookup -type=SOA <nom_de_domaine>  | Recherche les enregistrements SOA (Start of Authority) |
| nslookup -type=TXT <nom_de_domaine>  | Recherche les enregistrements TXT        |
| nslookup -debug <nom_de_domaine>     | Affiche des détails supplémentaires pour déboguer  |
| nslookup <nom_de_domaine> <serveur_DNS> | Interroge un serveur DNS spécifique   |
+--------------------------------------+------------------------------------------+
```

---

### Commandes **dig** (nécessite un environnement Linux/WSL) :

Sur Windows, **dig** n'est pas disponible par défaut. Cependant, si tu utilises un environnement **WSL** (Windows Subsystem for Linux), tu peux installer **dig** via un paquet comme **bind9-dnsutils**. Voici quelques commandes :

```
+--------------------------------------+------------------------------------------+
| Commande                             | Description                              |
+--------------------------------------+------------------------------------------+
| dig <nom_de_domaine>                 | Résout un nom de domaine en adresse IP   |
| dig <nom_de_domaine> @<serveur_DNS>  | Résout un nom via un serveur DNS spécifique |
| dig -x <adresse_IP>                  | Résolution inversée (IP vers nom de domaine) |
| dig <nom_de_domaine> +trace          | Trace la résolution DNS de manière détaillée |
| dig <nom_de_domaine> ANY             | Retourne tous les types d'enregistrements |
| dig <nom_de_domaine> A               | Retourne uniquement les enregistrements de type A |
| dig <nom_de_domaine> MX              | Retourne les enregistrements MX (mail)   |
| dig <nom_de_domaine> NS              | Retourne les serveurs DNS pour le domaine |
| dig <nom_de_domaine> SOA             | Retourne les enregistrements SOA (Start of Authority) |
| dig +short <nom_de_domaine>          | Donne une sortie simplifiée avec seulement l'IP |
+--------------------------------------+------------------------------------------+
```

### **Puis-je utiliser dig sur Windows ?**

Par défaut, **dig** n'est pas disponible dans les configurations Windows natives. Cependant, tu peux utiliser **nslookup** qui est déjà intégré à Windows pour toutes les tâches DNS de base. Si tu souhaites utiliser **dig**, voici des solutions :

1. **WSL (Windows Subsystem for Linux)** : Active WSL et installe une distribution Linux comme Ubuntu. Tu pourras ensuite installer **dig** via la commande :

   ```
   sudo apt-get install dnsutils
   ```

2. **Installer BIND pour Windows** : Tu peux télécharger les outils BIND (Berkeley Internet Name Domain) qui incluent **dig** pour Windows.

3. **Utiliser des outils en ligne** : Certains services en ligne te permettent d'exécuter des commandes **dig** sans avoir besoin de l'installer localement.

Pour un environnement pédagogique simple et avec des machines Windows, **nslookup** devrait être suffisant pour tous les besoins DNS de base.
