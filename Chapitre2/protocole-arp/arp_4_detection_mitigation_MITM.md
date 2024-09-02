### Détection et Mitigation d'une Attaque Man-in-the-Middle (MITM) : Explications Détaillées et Vulgarisation

Pour détecter et stopper une attaque Man-in-the-Middle (MITM), nous allons utiliser Wireshark pour l'analyse réseau et appliquer des techniques de prévention et de récupération. Nous allons détailler chaque étape avec des analogies de la vie quotidienne pour mieux comprendre.

#### Étape 1 : Installation de Wireshark

Wireshark est comme une loupe pour voir ce qui se passe dans le réseau. Voici comment l'installer :

1. **Ouvrez un terminal sur chaque machine (Victim1, Victim2, et Attacker)**.
2. **Installez Wireshark** avec la commande suivante :
   ```sh
   sudo apt update
   sudo apt install wireshark -y
   sudo usermod -aG wireshark $USER
   ```

3. **Redémarrez votre machine ou déconnectez-vous et reconnectez-vous pour appliquer les changements de groupe**.

#### Étape 2 : Capture de Paquets avec Wireshark

Pensez à Wireshark comme à une caméra de sécurité qui enregistre tout ce qui se passe sur le réseau.

1. **Lancez Wireshark** :
   ```sh
   wireshark
   ```

2. **Sélectionnez l'interface réseau appropriée** (par exemple, `enp0s3` ou `enp0s8`).

3. **Cliquez sur le bouton "Start Capturing Packets"** pour commencer à capturer le trafic réseau.

4. **Regardez les paquets ARP** :
   - Dans le filtre de Wireshark, tapez `arp` et appuyez sur Enter.

#### Étape 3 : Analyser les Paquets

Imaginez que vous avez une pile de lettres (paquets) et vous cherchez des lettres bizarres (paquets suspects).

1. **Vérifiez les paquets ARP**. Filtrez les paquets ARP en utilisant le filtre suivant dans Wireshark :
   ```plaintext
   arp
   ```

2. **Recherchez des réponses ARP non sollicitées** :
   - Filtrez pour voir les réponses ARP uniquement :
     ```plaintext
     arp.opcode == 2
     ```

3. **Si vous voyez des réponses ARP sans requêtes correspondantes**, cela peut indiquer un empoisonnement ARP.

#### Étape 4 : Identifier l'Attaquant

1. **Utilisez `arp-scan` pour voir toutes les adresses MAC sur le réseau** :
   - Installez `arp-scan` :
     ```sh
     sudo apt install arp-scan -y
     ```
   - Exécutez `arp-scan` pour scanner le réseau :
     ```sh
     sudo arp-scan --interface=enp0s8 --localnet
     ```

2. **Comparez les résultats avec les adresses MAC connues** des machines victimes pour identifier toute adresse MAC suspecte.

#### Étape 5 : Rétablir les Tables ARP

Imaginez que vous réparez une boîte aux lettres (table ARP) pour que les lettres aillent au bon endroit.

1. **Ajouter des entrées ARP statiques** pour rétablir les associations correctes :
   - Sur **Victim1** (192.168.1.20) :
     ```sh
     sudo arp -s 192.168.1.30 <MAC_ADDRESS_VICTIM2>
     ```
   - Sur **Victim2** (192.168.1.30) :
     ```sh
     sudo arp -s 192.168.1.20 <MAC_ADDRESS_VICTIM1>
     ```

#### Étape 6 : Prévenir les Attaques Futures

1. **Configurer des entrées ARP statiques dans `/etc/ethers`** :
   - Éditez le fichier `/etc/ethers` :
     ```sh
     sudo nano /etc/ethers
     ```
   - Ajoutez les lignes suivantes :
     ```plaintext
     192.168.1.20 <MAC_ADDRESS_VICTIM1>
     192.168.1.30 <MAC_ADDRESS_VICTIM2>
     ```

2. **Utiliser ARPWatch pour surveiller les modifications des tables ARP** :
   - Installez ARPWatch :
     ```sh
     sudo apt install arpwatch -y
     ```
   - Lancez ARPWatch :
     ```sh
     sudo arpwatch -i enp0s8
     ```

#### Étape 7 : Configurer le Routage pour Bloquer les Attaques

1. **Configurer votre routeur ou pare-feu pour surveiller et bloquer les attaques ARP**. Consultez la documentation de votre routeur pour des instructions spécifiques.

### Résumé des Commandes et Actions

#### Machine Attaquant (Attacker)

```sh
# Activer le transfert IP
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Empoisonner les tables ARP
sudo arpspoof -i enp0s8 -t 192.168.1.20 192.168.1.30
sudo arpspoof -i enp0s8 -t 192.168.1.30 192.168.1.20

# Rediriger le trafic HTTP
sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080

# Lancer mitmproxy
sudo mitmproxy -p 8080

# Réinitialiser les règles iptables
sudo iptables -t nat -F
```

#### Machines Victimes (Victim1 et Victim2)

```sh
# Ajouter des entrées ARP statiques
sudo arp -s 192.168.1.30 <MAC_ADDRESS_VICTIM2>  # Sur Victim1
sudo arp -s 192.168.1.20 <MAC_ADDRESS_VICTIM1>  # Sur Victim2
```

#### Installation de Wireshark et ARPWatch

```sh
# Installer Wireshark
sudo apt update
sudo apt install wireshark -y
sudo usermod -aG wireshark $USER
wireshark

# Installer et lancer ARPWatch
sudo apt install arpwatch -y
sudo arpwatch -i enp0s8
```

En suivant ces étapes, vous pouvez détecter, stopper et prévenir une attaque Man-in-the-Middle, tout en utilisant des outils de surveillance et de configuration réseau pour assurer la sécurité de vos communications.
