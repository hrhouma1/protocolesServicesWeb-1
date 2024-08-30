# Travaux Pratiques : Introduction à Wireshark sous Linux

## Objectifs

- **Partie 1** : Installer et vérifier la topologie Mininet.
- **Partie 2** : Capturer et analyser les données ICMP avec Wireshark.

## Contexte / Scénario

Dans ce lab, vous allez configurer une topologie Mininet sur la machine virtuelle CyberOps, ce qui vous permettra de simuler un réseau de plusieurs hôtes et routeurs. Vous utiliserez la commande `ping` pour tester la connectivité entre deux hôtes et capturerez les paquets ICMP avec Wireshark, un outil puissant pour l'analyse des réseaux.

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
   - **Explication** : Cette commande exécute un script Python avec des privilèges super-utilisateur (via `sudo`). Ce script configure une topologie réseau virtuelle dans Mininet, un émulateur de réseau qui crée un environnement de test de réseau complet sur une seule machine.

3. **Enregistrez les adresses IP et MAC pour les hôtes H1 et H2** :
   - Ouvrez des fenêtres terminales pour H1 et H2 :
     ```bash
     mininet> xterm H1
     mininet> xterm H2
     ```
   - **Explication** : La commande `xterm` est utilisée pour ouvrir une nouvelle fenêtre de terminal. Dans ce cas, `xterm H1` ouvre un terminal pour l'hôte H1 dans Mininet, vous permettant d'interagir avec cet hôte comme s'il s'agissait d'une machine séparée. Chaque hôte dans Mininet a sa propre configuration réseau (adresse IP et MAC), que vous pouvez vérifier et manipuler via cette fenêtre.

   - Dans le terminal de H1, exécutez la commande suivante :
     ```bash
     ip address
     ```
   - **Explication** : La commande `ip address` affiche toutes les interfaces réseau disponibles sur l'hôte ainsi que leurs adresses IP et MAC. Cela vous permet de vérifier la configuration réseau de l'hôte H1.

   - Répétez l’opération pour H2.

### Partie 2 : Capturer et Analyser les Données ICMP avec Wireshark

1. **Lancez Wireshark sur H1** :
   ```bash
   wireshark-gtk &
   ```
   - **Explication** : La commande `wireshark-gtk &` lance Wireshark en arrière-plan. Le `&` à la fin de la commande permet de libérer le terminal pour d'autres commandes tout en laissant Wireshark en cours d'exécution. `wireshark-gtk` est la version de Wireshark utilisant l'interface graphique GTK. Cela vous permet de capturer et d'analyser des paquets réseau de manière interactive.

   - Ignorez les messages d'avertissement. Ces messages sont généralement liés à l'environnement graphique et n'affectent pas la capture des paquets.
   - Sous l'en-tête "Capture", sélectionnez l'interface `H1-eth0`.
   - Cliquez sur "Start" pour commencer à capturer le trafic.

2. **Envoyez des requêtes ping de H1 à H2** :
   ```bash
   ping -c 5 10.0.0.12
   ```
   - **Explication** : La commande `ping` est utilisée pour tester la connectivité réseau entre deux hôtes. L'option `-c 5` spécifie que cinq paquets ICMP seront envoyés à l'adresse IP `10.0.0.12`, qui est l'adresse de l'hôte H2. Cette commande permet de vérifier que H1 peut atteindre H2 sur le réseau Mininet.

3. **Arrêtez la capture dans Wireshark** :
   - Cliquez sur "Stop" pour arrêter la capture.
   - Appliquez un filtre pour n'afficher que le trafic ICMP :
     ```text
     icmp
     ```
   - **Explication** : Le filtre `icmp` dans Wireshark permet de n'afficher que les paquets ICMP (Internet Control Message Protocol). Les paquets ICMP sont utilisés par la commande `ping` pour envoyer des requêtes et recevoir des réponses, ce qui est utile pour tester la connectivité réseau.

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
   - **Explication** : Comme précédemment, ces commandes ouvrent des fenêtres de terminal pour les hôtes H4 et le routeur R1, vous permettant de vérifier et manipuler leurs configurations réseau.

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
   - **Explication** : Cette commande envoie cinq requêtes ping à l'hôte H4, qui est un serveur distant simulé sur le réseau. Le but est de capturer et d'analyser les différences dans les paquets ICMP lorsque la requête traverse un routeur (R1 dans ce cas).

5. **Examinez les données capturées** :
   - Analysez les adresses IP et MAC des trames capturées.
   - Notez que l'adresse MAC correspond à l'interface R1.

### Nettoyage

1. **Arrêtez Mininet** :
   ```bash
   mininet> quit
   ```
   - **Explication** : La commande `quit` dans Mininet arrête l'émulateur et ferme toutes les sessions ouvertes pour les hôtes et autres équipements virtuels.

   - Nettoyez tous les processus utilisés par Mininet :
   ```bash
   sudo mn -c
   ```
   - **Explication** : La commande `mn -c` nettoie tous les processus et configurations résiduelles laissées par Mininet, y compris les interfaces virtuelles, les tunnels, et autres éléments réseau créés par Mininet. Elle garantit que le système est réinitialisé pour une utilisation future.

### Annexe

Toutes les commandes nécessaires pour ce lab sont répertoriées ci-dessous avec leurs explications :

- **Installer la topologie Mininet** :
  ```bash
  sudo ~/lab.support.files/scripts/cyberops_topo.py
  ```

- **Vérifier les adresses IP et MAC** :
  ```bash
  ip address
  ```

- **Ouvrir un terminal pour un hôte spécifique dans Mininet** :
  ```bash
  xterm H1
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

Note : Si vous ne trouvez pas le script d’installation, vérifiez que le chemin est correct ou demandez de l'aide à votre instructeur.
