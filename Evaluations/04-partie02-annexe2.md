#  Bonus TP1 : Configuration de DNS avec Zones Directes et Inversées

- Toutes les étapes sont  couvertes de manière chronologique et détaillée avec les commandes **PowerShell**.
- Toutes les étapes nécessaires à la configuration de DNS avec zones directes et inversées sont couvertes dans ce plan détaillé.
- Je vous présente quelques points de vérification supplémentaires pour être sûr que tout est bien pris en compte :

1. **Promouvoir DC en tant que contrôleur de domaine** : Cette étape est bien réalisée.
2. **Installation et configuration de DNS sur DC et DS** : Tous les éléments clés de la configuration DNS (zones directes et inversées, enregistrement PTR) sont présents.
3. **Test de connectivité et de résolution DNS** : Des tests de connectivité et de résolution sont prévus avec **ping** et **nslookup**.
4. **Redirecteur DNS** : La configuration du redirecteur pour les requêtes externes (vers 8.8.8.8) est incluse.

- Toutes les étapes importantes pour atteindre les objectifs du TP sont bien présentes ici .



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

# Étapes détaillées et chronologiques

### **Étape 1 : Installation du rôle AD DS et DNS sur DC (Machine 1)**

1. Connectez-vous à **DC**.
2. Installez les rôles **Active Directory Domain Services** (AD DS) et **DNS** avec la commande PowerShell suivante :

```powershell
Install-WindowsFeature -Name AD-Domain-Services, DNS -IncludeManagementTools
```

3. Une fois l’installation terminée, vous devez promouvoir **DC** en tant que contrôleur de domaine (Domain Controller) pour le domaine `test.local`.

4. Utilisez cette commande PowerShell pour promouvoir le serveur en tant que contrôleur de domaine et créer un nouveau domaine **test.local** :

```powershell
Install-ADDSForest -DomainName "test.local" -DomainNetBiosName "test" -SafeModeAdministratorPassword (ConvertTo-SecureString "MotDePasseSecurise123" -AsPlainText -Force) -InstallDNS
```


- Ici la création de **test.local** a bien été réalisée sous la forme de la commande suivante :

```powershell
Install-ADDSForest -DomainName "test.local" -DomainNetBiosName "test" -SafeModeAdministratorPassword (ConvertTo-SecureString "MotDePasseSecurise123" -AsPlainText -Force) -InstallDNS
```

- Cette commande promeut **DC** en tant que contrôleur de domaine et crée le domaine **test.local** en une seule étape.
- Donc, la création du domaine **test.local** a été bien prise en compte dans la commande.
- La commande suivante installe **Active Directory Domain Services** et crée un nouveau domaine **test.local** avec son DNS associé. 



5. Le serveur redémarre automatiquement après la promotion. Connectez-vous à nouveau à **DC** après le redémarrage.

---

# **Étape 2 : Installation de DNS sur DS (Machine 2)**

1. Connectez-vous à **DS**.
2. Installez le rôle **DNS** sur **DS** avec la commande suivante :

```powershell
Install-WindowsFeature -Name DNS -IncludeManagementTools
```

3. Activez la gestion DNS sur **DS** avec cette commande pour vous assurer que la console DNS est prête :

```powershell
Install-WindowsFeature -Name RSAT-DNS-Server
```

---

# **Étape 3 : Configuration de base réseau (sur DC et DS)**

#### Sur **DC** (Machine 1) :
1. Utilisez la commande suivante pour vérifier la configuration réseau de **DC** et noter son adresse IP.

```powershell
ipconfig /all
```

2. Résultat attendu pour **DC** (adresse IP : **192.168.100.10**).

#### Sur **DS** (Machine 2) :
1. Faites de même sur **DS** pour vérifier sa configuration réseau.

```powershell
ipconfig /all
```

2. Résultat attendu pour **DS** (adresse IP : **192.168.100.11**).

---

# **Étape 4 : Test de connectivité réseau (ping depuis DC et DS)**

#### Sur **DC** (Machine 1) :
1. Testez la connectivité en pingant **8.8.8.8** :

```powershell
ping 8.8.8.8
```

2. Ensuite, testez la connectivité avec **DS** en pingant **192.168.100.11** :

```powershell
ping 192.168.100.11
```

#### Sur **DS** (Machine 2) :
1. Testez la connectivité avec **8.8.8.8** :

```powershell
ping 8.8.8.8
```

2. Ensuite, testez la connectivité avec **DC** en pingant **192.168.100.10** :

```powershell
ping 192.168.100.10
```

---

# **Étape 5 : Configuration DNS sur DC (Machine 1)**

1. Connectez-vous à **DC** et ouvrez la console DNS avec la commande :

```powershell
dnsmgmt.msc
```

#### **Créer la zone de recherche directe pour test.local** :
1. Ouvrez la console DNS et créez une nouvelle zone de recherche directe pour **test.local** :

```powershell
Add-DnsServerPrimaryZone -Name "test.local" -ReplicationScope "Domain"
```

#### **Créer la zone de recherche inversée pour le réseau 192.168.100.0/24** :
1. Créez une zone de recherche inversée pour le réseau **192.168.100.x** :

```powershell
Add-DnsServerPrimaryZone -NetworkId "192.168.100.0/24" -ZoneFile "192.168.100.x.dns"
```

2. Résultat attendu : La zone inversée est visible dans la console DNS de **DC**.

---

# **Étape 6 : Ajout d'un enregistrement PTR (Machine 1)**

1. Ajoutez un enregistrement **PTR** pour **DC** (192.168.100.10) dans la zone de recherche inversée sur **DC** :

```powershell
Add-DnsServerResourceRecordPtr -ZoneName "100.168.192.in-addr.arpa" -Name "10" -PtrDomainName "dc.test.local"
```

2. Résultat attendu : L'enregistrement **PTR** pour **DC** apparaît dans la console DNS sous la zone inversée.

---

# **Étape 7 : Configuration DNS secondaire sur DS (Machine 2)**

1. Connectez-vous à **DS** et ouvrez la console DNS :

```powershell
dnsmgmt.msc
```

#### **Configurer une zone DNS secondaire sur DS pour test.local** :
1. Créez une zone secondaire pour le domaine **test.local** sur **DS** :

```powershell
Add-DnsServerSecondaryZone -Name "test.local" -ZoneFile "test.local.dns" -MasterServers 192.168.100.10
```

2. Résultat attendu : La zone **test.local** est visible en tant que zone secondaire dans la console DNS de **DS**.

---

# **Étape 8 : Test de la résolution DNS (sur DC et DS)**

#### **Test de la résolution directe (Machine 1 et Machine 2)**

1. Depuis **DC**, exécutez la commande **nslookup** pour vérifier la résolution de **test.local** :

```powershell
nslookup test.local
```

2. Faites de même depuis **DS** :

```powershell
nslookup test.local
```

3. Résultat attendu : Les deux machines devraient résoudre **test.local** vers l’adresse IP de **DC** (192.168.100.10).

#### **Test de la résolution inversée (Machine 2)**

1. Depuis **DS**, exécutez la commande **nslookup** pour vérifier la résolution inversée de l’adresse **192.168.100.10** :

```powershell
nslookup 192.168.100.10
```

2. Résultat attendu : La résolution inversée doit retourner **dc.test.local**.

---

# **Étape 9 : Configuration du redirecteur DNS (Machine 1)**

1. Connectez-vous à **DC** et ouvrez la console DNS.
2. Configurez un redirecteur DNS vers **8.8.8.8** avec la commande suivante :

```powershell
Set-DnsServerForwarder -IPAddress "8.8.8.8"
```

3. Résultat attendu : Le redirecteur vers **8.8.8.8** est configuré correctement et visible dans la console DNS.

---

### Résumé final des étapes

- **Machine 1 (DC)** : Installation d'AD DS, DNS, création des zones de recherche directe et inversée, configuration du PTR, configuration du redirecteur DNS.
- **Machine 2 (DS)** : Installation de DNS, création de la zone secondaire, test de la résolution DNS.
- **Tests** : Ping, nslookup pour tester la connectivité et la résolution des noms et adresses IP.

----------------------
# Annexe 1:
----------------------

## Question:

- Est-ce que j'ai besoin d'installer Active Directory sur la machine 2, ou est-ce que ce n'est pas nécessaire ?

## Réponse :

- Non, il n'est pas nécessaire d'installer **Active Directory** sur la machine **DS** (Machine 2). 

Voici pourquoi :
- **DC** (Domain Controller) est le serveur principal qui gère le domaine **test.local**. C'est ce serveur qui héberge **Active Directory** et assure l'authentification, la gestion des utilisateurs et des objets du domaine.
- **DS** est configuré ici en tant que serveur DNS secondaire. Il n'a pas besoin d'**Active Directory** car il ne joue pas le rôle de contrôleur de domaine. Son rôle est uniquement de répliquer et de servir de sauvegarde pour les informations DNS, ce qui suffit pour les tâches DNS dans ce TP.

Donc, pour ce TP, **Active Directory** est seulement nécessaire sur **DC**, et **DS** se concentre sur la gestion secondaire des enregistrements DNS sans nécessiter **Active Directory**.





----------------------
# Annexe 2:
----------------------

### Question
- Lorsque j'exécute la commande **ipconfig /all** sur une machine qui fait partie du domaine **test.local**, est-ce que cette commande affichera les adresses IP des autres machines dans le domaine, ou seulement celle de la machine sur laquelle elle est exécutée ?


### Réponse :


- Non, lorsque tu utilises la commande **ipconfig /all** sur chaque machine, elle affichera uniquement la configuration réseau locale de la machine où la commande est exécutée. Chaque machine affichera **sa propre** adresse IP, son masque de sous-réseau, sa passerelle par défaut, et d'autres informations réseau, mais elle ne montrera pas directement les adresses IP des autres machines du domaine.

Même si les deux machines font partie du même domaine **test.local**, elles ont chacune leur propre configuration réseau. Pour afficher les adresses IP des autres machines dans le même domaine, tu devras utiliser des commandes comme **ping** ou **nslookup** depuis l'une des machines pour interroger l'autre.

En résumé :
- **ipconfig /all** te montre uniquement la configuration réseau de la machine sur laquelle la commande est exécutée.
- Si tu veux tester la connectivité entre deux machines (par exemple **DC** et **DS**), tu peux utiliser **ping** pour vérifier l'adresse IP de l'autre machine.




