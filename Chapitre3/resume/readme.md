---
# 1 - NetBIOS
---

**NetBIOS (Network Basic Input/Output System)** est un protocole de communication qui permet aux applications sur des ordinateurs différents d’un réseau local (LAN) de communiquer entre elles. Il a été développé dans les années 1980 et est utilisé principalement dans les réseaux **IPv4** pour la **résolution des noms d’hôte** et la **partage de fichiers et d’imprimantes** sur un réseau local.

----
# 2 - Explication détaillée 
----

# **Les principales fonctions de NetBIOS :**

1. **Résolution de noms d’hôte dans un réseau IPv4 :**
   - **NetBIOS** permet de traduire un **nom d’hôte** (comme `Serv1`) en **adresse IP** (comme `192.168.0.1`) dans un réseau **IPv4** local. Cela facilite la communication entre les machines en utilisant des noms plutôt que des adresses IP, qui sont plus difficiles à retenir.

2. **Services de partage :**
   - **NetBIOS** est utilisé pour permettre à des ordinateurs de partager des fichiers et des imprimantes dans un réseau local. Grâce à NetBIOS, un utilisateur peut se connecter à des ressources partagées sur un autre ordinateur en utilisant des noms comme `\\NomDeMachine` (par exemple `\\Serv1`).

3. **Découverte des ordinateurs dans le réseau :**
   - Avec **NetBIOS**, les ordinateurs sur un réseau peuvent se "voir" et se connecter les uns aux autres. Cela fonctionne même s'il n'y a pas de serveur **DNS** dans le réseau, car NetBIOS gère la résolution des noms localement.

---
# 3 - Comment fonctionne NetBIOS
---

- **Résolution des noms d’hôte :**
   Lorsqu'un ordinateur souhaite se connecter à un autre sur le réseau en utilisant son nom (par exemple `Serv1`), **NetBIOS** traduit ce nom en adresse IP. Cela est particulièrement utile dans les réseaux où il n'y a pas de serveur **DNS** pour gérer la résolution des noms.
  
- **Diffusion sur le réseau :**
   **NetBIOS** utilise un mécanisme appelé **broadcasting** pour annoncer la présence d'une machine sur le réseau et pour découvrir les autres machines. Cela signifie qu'il envoie des messages à tous les ordinateurs du réseau pour savoir quelles machines sont disponibles.

---
# 4 - Cas d'utilisation typique de NetBIOS 
---

Imaginez un petit bureau avec plusieurs ordinateurs connectés à un réseau local, mais sans serveur **DNS**. Grâce à **NetBIOS**, ces ordinateurs peuvent se trouver facilement et se connecter les uns aux autres pour partager des fichiers ou des imprimantes. Par exemple, vous pouvez taper `\\ServeurDuBureau` dans l'Explorateur de fichiers pour accéder aux fichiers d'un autre ordinateur, et c'est **NetBIOS** qui permet cette connexion en résolvant le nom `ServeurDuBureau` en une adresse IP.

---
# 5 - Limites de NetBIOS 
---

- **IPv4 uniquement :** **NetBIOS** ne fonctionne que dans les réseaux **IPv4**. Il n'est pas utilisé dans les réseaux **IPv6**, où d'autres protocoles comme **LLMNR** (Link-Local Multicast Name Resolution) et **mDNS** (Multicast DNS) sont utilisés pour la résolution des noms d'hôte.

- **Broadcasting inefficace dans les grands réseaux :** **NetBIOS** utilise un mécanisme de **broadcast** pour découvrir les machines, ce qui devient moins efficace dans les grands réseaux où de nombreuses machines sont connectées, car cela génère beaucoup de trafic inutile.

---
# 6 - Conclusion 
---

**NetBIOS** est une technologie ancienne mais toujours utile dans les réseaux **IPv4** locaux pour résoudre les noms d'ordinateurs et faciliter la communication entre les machines. Dans les réseaux modernes **IPv6**, **NetBIOS** est remplacé par des technologies plus récentes comme **LLMNR** et **mDNS**, mais il reste important de comprendre **NetBIOS** pour bien travailler avec les réseaux **IPv4**, qui sont encore largement utilisés.
