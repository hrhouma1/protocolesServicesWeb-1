# Histoire de la tâche 7 :

Vous avez brillamment configuré les enregistrements DNS pour les domaines **techprog-XXX.xyz** et **datadyn-XXX.xyz**. Toutefois, pour assurer une gestion encore plus efficace du réseau et renforcer la sécurité, il vous est demandé de configurer des **zones inversées**, qui permettent de traduire des adresses IP en noms de domaine. C'est comme chercher le nom d'une personne en ayant uniquement son numéro de téléphone.

---

## 📝 Tâche 7 : Configurer des Zones Inversées pour la Traduction des Adresses IP en Noms de Domaine 📝

### Objectif :
Mettre en place des **zones inversées** pour permettre la traduction des adresses IP en noms de domaine, ce qui est utile pour la gestion du réseau et les audits de sécurité.

---

## Étapes de la procédure

### 1. Préparation de la Zone Inversée :
Imaginons que vous travaillez avec le réseau **172.16.1.0**. Vous allez configurer une zone inversée pour ce réseau.

#### a. Ouvrir le fichier de configuration DNS :
Modifiez le fichier **named.conf.local** pour ajouter une nouvelle zone inversée.
```bash
sudo nano /etc/bind/named.conf.local
```

#### b. Ajoutez la configuration de la zone inversée :
À la fin du fichier, copiez et collez la configuration suivante :
```bash
zone "1.16.172.in-addr.arpa" {
    type master;
    file "/var/lib/bind/1.16.172.rev";
};
```

Cela configure une zone inversée pour les adresses **172.16.1.X**.

Enregistrez les modifications (CTRL + O) et quittez l'éditeur (CTRL + X).

---

### 2. Préparation du fichier de zone inversée :
Créez un fichier pour stocker les informations de la zone inversée, puis donnez-lui les permissions correctes.

#### a. Créez et définissez les permissions du fichier :
```bash
sudo touch /var/lib/bind/1.16.172.rev
sudo chown root:bind /var/lib/bind/1.16.172.rev
sudo chmod 644 /var/lib/bind/1.16.172.rev
```

#### b. Modifiez le fichier de zone inversée :
Ouvrez le fichier pour ajouter les enregistrements PTR nécessaires.
```bash
sudo nano /var/lib/bind/1.16.172.rev
```

Ajoutez le contenu suivant pour configurer la traduction d'une adresse IP vers un nom de domaine :
```bash
$TTL 3600
1.16.172.in-addr.arpa. IN SOA hostname. email.domain.com. (
    2023100701 ; Serial
    10800      ; Refresh
    1800       ; Retry
    1814400    ; Expire
    3600       ; Minimum TTL
)
1.16.172.in-addr.arpa. IN NS hostname.
11.1.16.172.in-addr.arpa. IN PTR www.techprog-XXX.xyz.
```

**Explication :**
- **PTR** : Un enregistrement PTR (Pointer Record) associe une adresse IP à un nom de domaine. Ici, l'adresse **172.16.1.11** est associée à **www.techprog-XXX.xyz**.
- Remplacez **hostname** par le nom de votre serveur et **email.domain.com** par votre e-mail (avec un point à la place du @).

Enregistrez les modifications (CTRL + O) et quittez l'éditeur (CTRL + X).

---

### 3. Comprendre les Zones Inversées :
Si vous voulez approfondir la notion de zones inversées, consultez cette ressource :  
[Introduction aux DNS et enregistrements PTR](https://www.digitalocean.com/community/tutorials/an-introduction-to-dns-terminology-components-and-concepts#record-types)

---

### 4. Vérification de la configuration :
Avant de redémarrer le service DNS, assurez-vous que la configuration est correcte en utilisant la commande suivante :
```bash
named-checkconfig
```

Si aucune erreur n'est détectée, redémarrez le service DNS :
```bash
sudo systemctl restart named.service
```

---

### 5. Test de la zone inversée :
Pour vérifier que la zone inversée fonctionne correctement, utilisez l'outil **dig** pour traduire une adresse IP en nom de domaine :
```bash
dig -x 172.16.1.11 +short
```

Si tout est bien configuré, la commande devrait retourner **www.techprog-XXX.xyz**.

---

## Conclusion :
Vous avez maintenant mis en place une zone inversée pour traduire les adresses IP en noms de domaine, ce qui est particulièrement utile pour les audits réseau et les résolutions inversées. Répétez ce processus pour chaque réseau ou plage d'adresses IP que vous souhaitez gérer.

---

## RÉFÉRENCES
- **Ellingwood, J.** (2020, 04 novembre). [Introduction à la terminologie, aux composants et aux concepts du DNS](https://www.digitalocean.com/community/tutorials/an-introduction-to-dns-terminology-components-and-concepts)
- **Rauniyar, K. et Jain, R.** (2020, 18 mai). [Commande Dig dans Linux avec des exemples](https://www.geeksforgeeks.org/dig-command-in-linux-with-examples/)

---

Avec cette dernière tâche, vous maîtrisez désormais la configuration avancée des DNS, y compris la gestion des zones inversées et la résolution des noms de domaine à partir des adresses IP.
