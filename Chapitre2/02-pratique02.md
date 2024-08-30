# Travaux Pratiques : Introduction à Wireshark sous Linux

## Objectifs

- **Partie 1** : Installer et vérifier la topologie Mininet.
- **Partie 2** : Capturer et analyser les données ICMP avec Wireshark.

## Contexte / Scénario

Dans ce lab, vous utiliserez un script sur la machine virtuelle CyberOps pour configurer une topologie Mininet. Cette topologie vous permettra d’accéder à quatre hôtes, un commutateur et un routeur, ce qui vous permettra de simuler différents services et protocoles réseau sans avoir besoin de configurer un réseau physique. Vous utiliserez la commande `ping` entre deux hôtes et capturerez les requêtes ICMP avec Wireshark.

Wireshark est un analyseur de protocoles (ou analyseur de paquets) utilisé pour dépanner les réseaux, effectuer des analyses, développer des logiciels et des protocoles, et s'informer. Il capture chaque unité de données de protocole (PDU) et permet de décoder et analyser leur contenu conformément aux spécifications RFC ou autres appropriées.

## Ressources Requises

- **Machine virtuelle CyberOps VM**

## Instructions

### Partie 1 : Installer et Vérifier la Topologie Mininet

1. **Démarrez et connectez-vous au poste de travail CyberOps** :
   - Nom d'utilisateur : `analyst`
   - Mot de passe : `cyberops`

2. **Exécutez le script Python pour installer la topologie Mininet** :
   ```bash
   sudo ~/lab.support.files/scripts/cyberops_topo.py
   ```
   - Entrez le mot de passe lorsque vous y êtes invité.

3. **Enregistrez les adresses IP et MAC pour les hôtes H1 et H2** :
   - Ouvrez des fenêtres terminales pour H1 et H2 :
     ```bash
     mininet> xterm H1
     mininet> xterm H2
     ```
   - Dans le terminal de H1, exécutez la commande suivante :
     ```bash
     ip address
     ```
   - Notez les adresses IPv4 et MAC.
   - Répétez l’opération pour H2.

### Partie 2 : Capturer et Analyser les Données ICMP avec Wireshark

1. **Lancez Wireshark sur H1** :
   ```bash
   wireshark-gtk &
   ```
   - Ignorez les messages d'avertissement.
   - Sous l'en-tête "Capture", sélectionnez l'interface `H1-eth0`.
   - Cliquez sur "Start" pour commencer à capturer le trafic.

2. **Envoyez des requêtes ping de H1 à H2** :
   ```bash
   ping -c 5 10.0.0.12
   ```
   - L’option `-c 5` spécifie que cinq requêtes ping seront envoyées.

3. **Arrêtez la capture dans Wireshark** :
   - Cliquez sur "Stop" pour arrêter la capture.
   - Appliquez un filtre pour n'afficher que le trafic ICMP :
     ```text
     icmp
     ```
   - Cliquez sur "Apply".

4. **Analysez les données capturées** :
   - Dans Wireshark, examinez les trames de requête ICMP capturées.
   - Vérifiez que l’adresse MAC source correspond à l’interface de H1 et que l’adresse MAC de destination correspond à celle de H2.

### Questions

1. L'adresse MAC source correspond-elle à l'interface de H1 ?
2. L'adresse MAC de destination dans Wireshark correspond-elle à celle de H2 ?

### Analyse Avancée : Capturer des Données depuis un Réseau Distant

1. **Ouvrez des fenêtres terminales pour H4 et R1** :
   ```bash
   mininet> xterm H4
   mininet> xterm R1
   ```

2. **Vérifiez les adresses IP et MAC pour H4 et R1** :
   ```bash
   ip address
   ```
   - Notez les adresses.

3. **Démarrez une nouvelle capture Wireshark sur H1** :
   - Sélectionnez "Capture > Start" ou appuyez sur `Ctrl-E`.
   - Choisissez "Continue without Saving" pour commencer une nouvelle capture.

4. **Envoyez une requête ping de H1 à H4** :
   ```bash
   ping -c 5 172.16.0.40
   ```
   - La requête ping devrait réussir.

5. **Examinez les données capturées** :
   - Analysez les adresses IP et MAC des trames capturées.
   - Notez que l'adresse MAC correspond à l'interface R1.

### Nettoyage

1. **Arrêtez Mininet** :
   ```bash
   mininet> quit
   ```
   - Nettoyez tous les processus utilisés par Mininet :
   ```bash
   sudo mn -c
   ```

### Annexe

Toutes les commandes nécessaires pour ce lab sont répertoriées ci-dessous :

- **Installer la topologie Mininet** :
  ```bash
  sudo ~/lab.support.files/scripts/cyberops_topo.py
  ```

- **Vérifier les adresses IP et MAC** :
  ```bash
  ip address
  ```

- **Lancer Wireshark** :
  ```bash
  wireshark-gtk &
  ```

- **Filtrer le trafic ICMP dans Wireshark** :
  ```text
  icmp
  ```

- **Envoyer des requêtes ping** :
  ```bash
  ping -c 5 [IP_Destination]
  ```

- **Nettoyer Mininet** :
  ```bash
  sudo mn -c
  ```

Note : Si vous ne trouvez pas le script d’installation, assurez-vous que le chemin est correct ou consultez votre instructeur pour obtenir de l'aide.
