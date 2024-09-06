# Histoire de la tâche 3 : 

Après avoir brillamment installé et configuré la base de données, votre superviseur chez **TechFuture-PC** est très satisfait de vos compétences techniques. Votre prochain défi est de mettre en place un service DNS (Domain Name System), un élément clé pour la gestion du réseau de l'entreprise, car il permet la résolution des noms de domaine en adresses IP.

---

## 📝 Tâche 3 : Installation et Configuration du Service DNS sous Linux 📝

### Objectif :
Installer le service **DNS** et le configurer pour une utilisation de base. Cela inclut la mise en place du service DNS, la vérification de son bon fonctionnement, et la gestion des ports associés.

---

## Étapes de la procédure

### 1. Vérification de la capacité d'installation :
Avant d'installer un nouveau service, assurez-vous que votre système est à jour. Cela évitera les conflits et vous garantira d’avoir les dernières versions des paquets disponibles :
```bash
sudo apt update
```

---

### 2. Installation du service DNS :
Le service **BIND9** est utilisé comme serveur DNS dans cet exercice. Installez-le ainsi que les utilitaires associés avec la commande suivante :
```bash
sudo apt install -y bind9 bind9-utils
```

**Explication :**
- `bind9` : Le serveur DNS.
- `bind9-utils` : Des utilitaires supplémentaires pour tester et gérer le DNS.

---

### 3. Vérification du statut du service après installation :
Vérifiez que le service **DNS** fonctionne correctement après son installation. Le service peut être nommé `named.service` ou `bind9.service`, selon la distribution :
```bash
sudo systemctl status named.service
```

**Explication :**
Cette commande permet de vérifier si le service est actif et en cours d'exécution.

---

### 4. Démarrage et activation du service (si nécessaire) :
Si le service DNS n'est pas démarré automatiquement, démarrez-le manuellement et assurez-vous qu'il s'exécutera automatiquement au démarrage du système :
```bash
sudo systemctl start named.service
sudo systemctl enable named.service
```

---

### 5. Vérifiez les ports utilisés par le service DNS :
Le DNS utilise généralement le port **53** pour ses communications. Vous pouvez vérifier quels ports sont utilisés par le service DNS avec la commande suivante :
```bash
ss -ltn
```
Pour des détails supplémentaires sur les processus actifs :
```bash
sudo ss -ltnp
```

---

### 6. Configuration du pare-feu :
Pour que le service DNS puisse répondre aux requêtes, vous devez ouvrir le port **53** sur le pare-feu, à la fois pour les connexions **TCP** et **UDP** :
```bash
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
```

**Explication :**
Le protocole DNS utilise à la fois **TCP** et **UDP** pour différents types de requêtes. Il est donc important d’autoriser les deux.

---

### 7. Validation de la configuration du pare-feu :
Vérifiez que les règles du pare-feu sont correctement appliquées et que le port **53** est bien ouvert :
```bash
sudo ufw status
```

---

### 8. Test du service DNS :
Pour vous assurer que le service DNS est opérationnel, utilisez la commande `dig` pour interroger le serveur DNS local :
```bash
dig @localhost
```

**Explication :**
- `@localhost` : Indique que la requête est envoyée au serveur DNS local.

---

## Questions :
1. Pourquoi est-il crucial d'autoriser le trafic sur les ports spécifiques utilisés par le service DNS dans le pare-feu ?
2. Comment le DNS facilite-t-il la navigation sur le web pour les utilisateurs finaux ?

---

## Suite de l’histoire de la tâche 3 : 

Votre superviseur vous informe qu'une partie du réseau interne utilise le port **5353** pour les requêtes DNS, en raison d'une ancienne configuration. Votre mission est de configurer le service DNS pour qu'il écoute également sur ce port, en utilisant la technique du **port forwarding** (redirection de port).

---

### 9. Configuration de la redirection de port :
Pour rediriger le trafic entrant sur le port **5353** vers le port **53** utilisé par le DNS, configurez les règles de **port forwarding** à l'aide de `iptables` :
```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 5353 -j REDIRECT --to-port 53
sudo iptables -t nat -A PREROUTING -p udp --dport 5353 -j REDIRECT --to-port 53
```

---

### 10. Sauvegardez les règles iptables :
Afin que ces règles persistent après un redémarrage, sauvegardez-les dans un fichier de configuration :
```bash
sudo iptables-save > /etc/iptables.rules
```

---

### 11. Configuration du pare-feu :
Ouvrez également le port **5353** sur le pare-feu pour permettre le trafic entrant à travers ce port :
```bash
sudo ufw allow 5353/tcp
sudo ufw allow 5353/udp
```

---

### 12. Validation de la configuration du pare-feu :
Vérifiez que les ports **5353** et **53** sont bien autorisés par le pare-feu :
```bash
sudo ufw status
```

---

### 13. Test du service DNS avec le port redirigé :
Pour vérifier que le service DNS fonctionne correctement avec la redirection du port **5353**, utilisez à nouveau la commande `dig` en spécifiant le port **5353** :
```bash
dig @localhost -p 5353
```

---

## Questions :
1. Quel est l'intérêt de rediriger le trafic d'un port vers un autre ?
2. Dans quelles autres situations le **port forwarding** pourrait-il être utile ?
3. Comment garantir que les règles de **port forwarding** persistent après le redémarrage du système ?

---

Cette section vous a permis de mettre en place et configurer un serveur DNS fonctionnel avec la gestion des ports standards ainsi que la redirection de ports. Vous avez également appris à configurer le pare-feu et à tester le bon fonctionnement du service.

---

**La prochaine section portera sur des configurations avancées du service DNS.**
