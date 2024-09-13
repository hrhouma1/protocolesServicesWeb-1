# Histoire de la tâche 2 : 

Vous avez réussi à configurer votre ordinateur conformément aux normes de **TechFuture-PC**. Impressionnés par votre rapidité et votre efficacité, vos superviseurs vous confient une nouvelle mission : installer et configurer une base de données pour stocker des informations essentielles sur les clients de l'entreprise.

---

## 📝 Tâche 2 : Installation et Configuration de la Base de Données MySQL 📝

### Objectif :
Installer le système de gestion de base de données **MySQL**, le sécuriser, et créer une nouvelle base de données pour les clients.

---

## Préparation

### 1. Mise à jour des paquets et installation de MySQL :
Avant d'installer MySQL, assurez-vous que votre système est à jour. Cela permet d'éviter les conflits de version et de s'assurer que tous les paquets nécessaires sont présents.

Exécutez les commandes suivantes pour mettre à jour le système et installer MySQL :
```bash
sudo apt update
sudo apt install mysql-server
```

**Explication :**
- `sudo apt update` : Cette commande met à jour la liste des paquets disponibles sur les dépôts.
- `sudo apt install mysql-server` : Installe le serveur MySQL et ses dépendances.

---

### Sécurisation

### 2. Sécurisation de MySQL :
Après l'installation de MySQL, il est essentiel de sécuriser votre installation pour limiter les risques de failles de sécurité. MySQL propose un script interactif appelé **`mysql_secure_installation`** qui vous guide à travers plusieurs étapes importantes :
- Définir un mot de passe fort pour l'utilisateur `root`.
- Supprimer les utilisateurs anonymes.
- Désactiver l'accès à distance pour l'utilisateur `root`.
- Supprimer la base de données de test.

Lancez le script avec la commande suivante :
```bash
sudo mysql_secure_installation
```

**Ce que vous devez faire** :
1. Lorsque le script vous demande si vous voulez définir un mot de passe `root`, choisissez un mot de passe fort et suivez les recommandations (combinaison de lettres, chiffres et caractères spéciaux).
2. Acceptez les options proposées pour améliorer la sécurité, comme la suppression des utilisateurs anonymes et la base de données de test.

---

## Connexion et Configuration

### 3. Connexion à MySQL :
Une fois MySQL sécurisé, vous pouvez vous connecter à la base de données en tant qu'utilisateur `root`. Cette étape est cruciale pour gérer la base de données et créer de nouveaux utilisateurs ou bases de données.

Pour vous connecter, utilisez la commande suivante :
```bash
sudo mysql -u root -p
```
- `-u root` : Indique que vous vous connectez avec l'utilisateur `root`.
- `-p` : Demande un mot de passe, que vous avez défini lors de l'étape de sécurisation.

---

### 4. Création d'une nouvelle base de données :
Maintenant que vous êtes connecté à MySQL, il est temps de créer une base de données dédiée aux informations clients. La commande SQL suivante vous permet de créer la base de données `clients_techfuture` :
```sql
CREATE DATABASE clients_techfuture;
```

**Explication :**
- `CREATE DATABASE clients_techfuture;` : Cette commande crée une nouvelle base de données appelée **clients_techfuture**. Elle sera utilisée pour stocker les informations essentielles sur les clients.

---

### 5. Quittez MySQL :
Une fois la base de données créée, quittez MySQL pour revenir à votre session normale dans le terminal. Utilisez la commande suivante :
```bash
exit;
```

---

## Vérification

### 6. Vérification de la création de la base de données :
Pour vous assurer que la base de données a bien été créée, utilisez la commande suivante, qui vous permet de lister toutes les bases de données présentes sur votre serveur MySQL :
```bash
sudo mysql -u root -p -e "SHOW DATABASES;"
```

Recherchez **clients_techfuture** dans la liste. Si elle est présente, cela signifie que la base de données a bien été créée avec succès.

**Explication :**
- `-e "SHOW DATABASES;"` : Exécute la commande SQL directement depuis le terminal sans avoir besoin d'entrer dans l'interface MySQL.

---

### Question :
- Pourquoi est-il important de sécuriser une installation MySQL juste après l'avoir installée ?

---

En conclusion, vous avez correctement installé MySQL, sécurisé l'installation, créé une base de données dédiée aux informations clients, et vérifié la bonne création de cette base. Cette tâche vous permet de maîtriser les bases de la gestion d'un système de base de données sous Linux.

---

**La prochaine section portera sur la Tâche 3 : Installation et Configuration du Service DNS.**
