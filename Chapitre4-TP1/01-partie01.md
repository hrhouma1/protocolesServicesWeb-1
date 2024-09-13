
# Histoire de la tâche 1 : 
Vous venez de commencer un stage chez **TechFuture-PC**, une entreprise de pointe dans le domaine de la technologie. Le premier jour, on vous remet un ordinateur flambant neuf pour travailler. Cependant, pour des raisons de sécurité et de traçabilité, l'entreprise a une politique stricte : chaque stagiaire doit configurer son ordinateur avec un nom d'utilisateur basé sur son numéro d'étudiant, soit "eNumEtudiant". Vous êtes enthousiaste à l'idée de commencer, mais avant cela, il est essentiel de configurer votre machine selon les normes de l'entreprise.

### 📝 Tâche 1 : Configuration de Nom d'Hôte sous Linux 📝

#### Objectif :
Configurer le nom d'hôte basé sur votre prénom et nom de famille et assurer que vous travaillez sous l'utilisateur "eNumEtudiant".

---

## Préparation

### 1. Création de l'utilisateur "eNumEtudiant" et désactivation de l'accès root :
Créez un nouvel utilisateur "eNumEtudiant" et désactivez l'accès direct à l'utilisateur `root` pour renforcer la sécurité :
```bash
sudo adduser eNumEtudiant
sudo passwd -l root
```

### 2. Ajouter "eNumEtudiant" à la liste des sudoers :
Pour permettre à l'utilisateur d'exécuter des commandes avec des privilèges administratifs, ajoutez-le au fichier `sudoers` :
```bash
echo "eNumEtudiant ALL=(ALL:ALL) ALL" | sudo tee -a /etc/sudoers
```

### 3. Connexion avec l'utilisateur "eNumEtudiant" :
Connectez-vous en tant que cet utilisateur :
```bash
su – eNumEtudiant
```

#### Question :
Quelle est la différence entre les commandes suivantes ?
- `su – eNumEtudiant`
- `su eNumEtudiant`

---

## Configuration

### 4. Définition du nom d'hôte basé sur votre prénom et nom :
Modifiez le nom d'hôte de la machine en fonction de votre prénom et nom de famille, suivi de "-techfuture-pc". Par exemple :
- **John Smith** deviendra `jsmith-techfuture-pc`
- **Emily Davis** deviendra `edavis-techfuture-pc`

Exécutez la commande suivante pour définir le nom d'hôte :
```bash
sudo hostnamectl set-hostname "votre_nom_d'équipe"
```

---

### 5. Éditer le fichier `/etc/hosts` avec `nano` :
Modifiez le fichier `/etc/hosts` pour remplacer l'ancien nom d'hôte par le nouveau. Si la ligne contient "127.0.1.1 ancien-nom", remplacez "ancien-nom" par "votre_nom_d'équipe" :
```bash
sudo nano /etc/hosts
```

### Vérification

### 6. Assurez-vous que le nom d'hôte est correctement configuré :
Vérifiez que la modification du nom d'hôte a bien été effectuée :
```bash
hostname
```

### 7. Déconnectez-vous et reconnectez-vous :
Déconnectez-vous de la session utilisateur et reconnectez-vous pour que les changements prennent effet.

### 8. Mise à jour des paquets :
Avant de commencer d'autres activités, assurez-vous que votre système est à jour :
```bash
sudo apt update && sudo apt upgrade -y
```

---

### Question Bonus :
- Quels sont les avantages de désactiver l'accès direct à l'utilisateur root ?

---

### Références rapides :
- [Commande hostname sur Linux - GeeksforGeeks](https://www.geeksforgeeks.org/hostname-command-in-linux-with-examples/)
- [Exemples de commande hostname - TecMint](https://www.tecmint.com/hostname-command-examples-for-linux/)

---

Fin de l'histoire de la tâche 1.

---

La prochaine section abordera la **Tâche 2 : Installation et Configuration de MySQL**.
