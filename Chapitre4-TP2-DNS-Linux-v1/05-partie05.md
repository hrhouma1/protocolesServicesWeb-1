# Histoire de la tâche 5 : 

Votre expérience chez **TechFuture-PC** continue de s'enrichir, et vos responsabilités augmentent. Vous êtes désormais en charge de la gestion des **zones DNS** pour plusieurs partenaires de l'entreprise, tels que **TechProg** et **DataDyn**. La configuration DNS pour ces collaborations est cruciale pour garantir une communication fluide et fiable au sein du réseau.

---

## 📝 Tâche 5 : Configuration Avancée du DNS pour les Collaborations avec TechProg et DataDyn 📝

### Objectif :
Configurer les zones DNS pour les partenaires **TechProg** et **DataDyn** afin de garantir la résolution correcte des noms de domaine.

---

## Étapes de la procédure

### 1. Préparation du système et désactivation de la résolution de nom par défaut :
Avant de configurer le serveur DNS, désactivez le service de résolution de nom par défaut de votre système afin d'utiliser exclusivement **BIND9**.

```bash
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
sudo rm /etc/resolv.conf
sudo nano /etc/resolv.conf
```

Dans le fichier `/etc/resolv.conf`, ajoutez la ligne suivante :
```bash
nameserver 127.0.0.1
```

**Explication :** Cela permet de rediriger toutes les requêtes DNS vers le serveur DNS local.

Redémarrez la machine pour appliquer les changements :
```bash
sudo reboot
```

---

### 2. Configuration initiale du DNS :
Maintenant que le système est prêt, configurez le serveur **BIND9**. Ouvrez le fichier de configuration pour spécifier les options de base :
```bash
sudo nano /etc/bind/named.conf.options
```

Assurez-vous que la configuration est correcte en exécutant cette commande :
```bash
named-checkconf
```

Redémarrez ensuite le service DNS pour appliquer les modifications :
```bash
sudo systemctl restart bind9
```

---

### 3. Configuration des requêtes DNS :
Modifiez le fichier **named.conf.options** pour spécifier les réseaux autorisés à faire des requêtes vers votre serveur DNS.

```bash
sudo nano /etc/bind/named.conf.options
```

Ajoutez ou modifiez la section `acl` et `options` comme suit :
```bash
acl corpnets { 192.168.0.0/24; localhost; };

options {
    directory "/var/cache/bind";
    allow-query { corpnets; };
    dnssec-validation auto;
    listen-on-v6 { any; };
};
```

---

### 4. Création des zones pour **techprog-XXX.xyz** et **datadyn-XXX.xyz** :
Créez et configurez les zones DNS pour les deux domaines en modifiant le fichier `named.conf.local`.

```bash
sudo nano /etc/bind/named.conf.local
```

Ajoutez les sections suivantes :
```bash
zone "techprog-XXX.xyz" {
    type master;
    file "/var/lib/bind/techprog-XXX.xyz.db";
};

zone "datadyn-XXX.xyz" {
    type master;
    file "/var/lib/bind/datadyn-XXX.xyz.db";
};
```

Remplacez **XXX** par la première lettre de votre prénom et de chacun de vos noms de famille.

Ensuite, vérifiez la configuration des zones avec ces commandes :
```bash
named-checkzone techprog-XXX.xyz /etc/bind/db.techprog-XXX.xyz
named-checkzone datadyn-XXX.xyz /etc/bind/db.datadyn-XXX.xyz
```

---

### 5. Appliquez les modifications :
Redémarrez le service DNS pour appliquer toutes les configurations :
```bash
sudo systemctl restart named.service
```

---

### 6. Configuration du serveur pour utiliser sa propre adresse IP comme serveur DNS :
Modifiez le fichier `/etc/resolv.conf` pour que votre serveur utilise sa propre adresse IP comme serveur DNS.

```bash
sudo nano /etc/resolv.conf
```

Ajoutez les lignes suivantes :
```bash
nameserver ip_ubuntu_server
search techprog-XXX.xyz datadyn-XXX.xyz
```

---
