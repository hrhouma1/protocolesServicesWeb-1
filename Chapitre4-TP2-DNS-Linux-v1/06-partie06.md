# Histoire de la tâche 6 : 

Après avoir configuré les zones DNS, il est maintenant temps de définir des **enregistrements DNS** pour les domaines **techprog-XXX.xyz** et **datadyn-XXX.xyz**.

---

## 📝 Tâche 6 : Création d'Enregistrements DNS pour TechProg et DataDyn 📝

### Objectif :
Créer des **enregistrements DNS de type A** pour les domaines **techprog-XXX.xyz** et **datadyn-XXX.xyz**, facilitant ainsi la résolution des noms de domaine en adresses IP.

---

### 1. Création de fichiers de zone pour **techprog-XXX.xyz** :
Créez le fichier pour la zone **techprog-XXX.xyz** et attribuez-lui les bons droits :
```bash
sudo touch /var/lib/bind/techprog-XXX.xyz.db
sudo chown bind:bind /var/lib/bind/techprog-XXX.xyz.db
sudo chmod 644 /var/lib/bind/techprog-XXX.xyz.db
```

Ensuite, ouvrez le fichier et ajoutez la configuration suivante :
```bash
sudo nano /var/lib/bind/techprog-XXX.xyz.db
```

Ajoutez ces enregistrements DNS dans le fichier :
```bash
$TTL 3600
techprog-XXX.xyz.   IN SOA  sboumelit-techfuture-pc. admin.techprog-XXX.xyz. (
        2023100701   ; Serial: YYYYMMDDnn
        10800        ; Refresh: 3 hours
        1800         ; Retry: 30 minutes
        1814400      ; Expire: 3 weeks
        3600         ; Minimum TTL: 1 hour
)
techprog-XXX.xyz.   IN NS   sboumelit-techfuture-pc.
www.techprog-XXX.xyz.   IN A   ???.???.???.11
```

---

### 2. Création d'enregistrements supplémentaires :
Ouvrez le fichier **techprog-XXX.xyz.db** à nouveau pour ajouter des sous-domaines :
```bash
sudo nano /var/lib/bind/techprog-XXX.xyz.db
```

Ajoutez les lignes suivantes :
```bash
ftp.techprog-XXX.xyz.      IN A  ???.???.???.12
mail.techprog-XXX.xyz.     IN A  ???.???.???.13
fichiers.techprog-XXX.xyz. IN A  ???.???.???.14
```

---

### 3. Configuration des enregistrements DNS pour **datadyn-XXX.xyz** :
De la même manière, ouvrez le fichier de zone pour **datadyn-XXX.xyz** :
```bash
sudo nano /var/lib/bind/datadyn-XXX.xyz.db
```

Ajoutez les enregistrements suivants :
```bash
bombas.datadyn-XXX.xyz. IN A 192.168.100.111
trampas.datadyn-XXX.xyz. IN A 192.168.100.112
```

---

### 4. Redémarrage du service DNS :
Après avoir ajouté les enregistrements DNS, redémarrez le service DNS pour appliquer les modifications :
```bash
sudo systemctl restart named.service
```

---

### 5. Vérification des enregistrements DNS :
Vérifiez si les enregistrements DNS sont correctement résolus en utilisant la commande `ping` :
```bash
ping www.techprog-XXX.xyz
```

Répétez l'opération pour les autres sous-domaines :
```bash
ping ftp.techprog-XXX.xyz
ping mail.techprog-XXX.xyz
```

Pour un test plus approfondi, utilisez la commande `dig` :
```bash
dig www.techprog-XXX.xyz
```

---

Avec cette configuration, vous avez mis en place les zones DNS pour **TechProg** et **DataDyn**, créé les enregistrements DNS, et vérifié la résolution correcte des noms de domaine. Vous êtes désormais prêt à gérer ces domaines efficacement.
