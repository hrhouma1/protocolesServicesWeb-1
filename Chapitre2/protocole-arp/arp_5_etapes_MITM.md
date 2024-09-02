

# Étapes
1. Installation des Machines Virtuelles :
   - Commencez par télécharger et installer VirtualBox depuis [ce lien](https://www.virtualbox.org/).
   - Téléchargez l'image ISO d'Ubuntu 22.04 LTS à partir de [ce lien](https://ubuntu.com/download/desktop).

2. Configuration des Machines Virtuelles :
   - Créez trois machines virtuelles dans VirtualBox : une pour l'attaquant et deux pour les victimes.
   - Pour chaque machine, configurez les paramètres suivants :
     - Type: Linux
     - Version: Ubuntu (64-bit)
     - Mémoire vive: 2048 MB
     - Disque dur: Créez un nouveau disque virtuel VDI (20 Go et dynamiquement alloué).

3. Installation d'Ubuntu :
   - Installez Ubuntu 22.04 sur chaque machine virtuelle en utilisant l'image ISO téléchargée. Suivez les instructions à l'écran pour compléter l'installation.

4. Configuration du Réseau :
   - Configurez deux adaptateurs réseau pour chaque machine :
     - Adaptateur 1: Bridge Adapter (connexion à Internet via DHCP).
     - Adaptateur 2: NAT Network (nommé `natnet`, adresse réseau `10.0.2.0/24`).

5. Configuration des Adresses IP Statiques :
   - Sur chaque machine, configurez les adresses IP statiques pour l'adaptateur NAT Network (enp0s8) :
     - Attacker: 10.0.2.10
     - Victim 1: 10.0.2.20
     - Victim 2: 10.0.2.30
   - Utilisez `netplan` pour appliquer ces configurations.

6. Vérification de la connectivité :
   - Assurez-vous que toutes les machines peuvent se pinger entre elles via l'interface enp0s8.

7. Installation des Outils pour l'Attaque :
   - Sur la machine de l'attaquant, installez les outils nécessaires comme `dsniff`, `ettercap-text-only`, `iptables`, et `mitmproxy`.

8. Début de l'Empoisonnement ARP :
   - Exécutez `arpspoof` pour commencer à empoisonner les tables ARP des victimes et rediriger leur trafic vers la machine attaquante.

Prenez le temps de configurer correctement chaque étape et vérifiez la connectivité à chaque point pour vous assurer que tout fonctionne comme prévu. 

Bonne continuation,
