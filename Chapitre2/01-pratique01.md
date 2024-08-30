---
# Explication des Paquets ICMP dans Wireshark
---

**Contexte :**
Vous avez effectué un ping vers l'adresse IP 8.8.8.8 et capturé les paquets ICMP échangés avec Wireshark. Vous avez mentionné que votre adresse physique est `00-93-37-E3-86-C2` et vous êtes au cégep. Vous vous demandez si le routeur est la destination de ces paquets.

**Analyse des paquets ICMP :**

1. **Adresse IP source et destination :**
   - **Source :** 10.90.140.11 (votre adresse IP locale).
   - **Destination :** 8.8.8.8 (Google Public DNS).

2. **Protocole :** 
   - **ICMP (Internet Control Message Protocol)** est utilisé pour envoyer des messages de diagnostic et de contrôle dans un réseau IP. Le ping utilise ICMP pour tester la connectivité.

3. **Adresse Physique (MAC) :**
   - **Source MAC:** `00-93-37-E3-86-C2` (votre adresse physique).
   - **Destination MAC:** `80:24:8f:98:b2:7f` (l'adresse MAC du routeur ou d'un autre équipement réseau intermédiaire).

4. **Analyse des messages ICMP :**
   - **ICMP Echo (ping) Request :** Vous envoyez une requête ping à l'adresse 8.8.8.8 pour vérifier la connectivité.
   - **ICMP Echo Reply :** Vous recevez une réponse de 8.8.8.8, confirmant que la connexion est réussie.

**Est-ce que le routeur est la destination ?**
Non, le routeur n'est pas la destination finale de ces paquets ICMP. Le routeur est simplement un intermédiaire qui transfère les paquets entre votre machine (source) et l'adresse 8.8.8.8 (destination). L'adresse physique `80:24:8f:98:b2:7f` est probablement celle de votre routeur, qui route les paquets vers l'extérieur du réseau local.

### Tableau Récapitulatif des Paquets ICMP

| **No.** | **Heure (s)** | **Source IP** | **Destination IP** | **Protocole** | **Type**            | **Longueur (octets)** | **Infos**                      |
|---------|---------------|---------------|--------------------|---------------|---------------------|------------------------|---------------------------------|
| 1786    | 19.375981      | 10.90.140.11  | 8.8.8.8            | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=1                |
| 1787    | 19.379116      | 8.8.8.8       | 10.90.140.11       | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=1                |
| 1819    | 20.396209      | 10.90.140.11  | 8.8.8.8            | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=2                |
| 1820    | 20.399701      | 8.8.8.8       | 10.90.140.11       | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=2                |

**Résumé des captures :**
- **Ping Statistiques :**
  - **Paquets envoyés :** 4
  - **Paquets reçus :** 4
  - **Perte de paquets :** 0%
  - **Temps de réponse moyen :** 2 ms

Ces informations vous confirment que votre réseau est bien configuré pour acheminer les paquets vers et depuis l'extérieur, avec un temps de latence très faible, ce qui est idéal pour des tests de connectivité réseau comme celui-ci.

---
# Annexe 1 - Explication Exhaustive des Paquets ICMP dans Wireshark pour Débutants
---

#### Contexte
Vous avez utilisé la commande `ping` pour tester la connectivité entre votre ordinateur et l'adresse IP 8.8.8.8, qui est l'une des adresses du service Google Public DNS. Vous avez capturé les paquets échangés entre votre ordinateur et le réseau avec Wireshark, un outil permettant d'analyser les données qui circulent dans un réseau.

#### Détails des Tableaux ICMP dans Wireshark

**1. Adresse IP Source et Destination :**
   - **Source IP :** C'est l'adresse IP de l'appareil qui envoie le paquet. Dans ce cas, votre ordinateur utilise l'adresse IP `10.90.140.11` pour communiquer.
   - **Destination IP :** C'est l'adresse IP de l'appareil qui reçoit le paquet. Ici, c'est l'adresse IP `8.8.8.8`, le serveur DNS de Google, qui reçoit les requêtes de ping.

**2. Adresse MAC (ou adresse physique) :**
   - **Source MAC :** Il s'agit de l'adresse MAC de votre carte réseau. Dans votre capture, l'adresse MAC source est `00-93-37-E3-86-C2`, ce qui correspond à l'interface réseau Wi-Fi de votre ordinateur.
   - **Destination MAC :** L'adresse MAC de destination est `80:24:8f:98:b2:7f`, qui correspond au routeur ou à un autre équipement intermédiaire dans votre réseau local. Le routeur utilise cette adresse pour faire suivre les paquets vers l'extérieur du réseau local.

**3. Protocole :**
   - **ICMP (Internet Control Message Protocol) :** Ce protocole est utilisé pour envoyer des messages de diagnostic ou de contrôle. Dans ce cas, il est utilisé par la commande `ping` pour tester la connectivité entre votre ordinateur et un autre appareil sur le réseau.

**4. Type de Message ICMP :**
   - **Echo (ping) Request :** Votre ordinateur envoie une requête ICMP de type "Echo Request" à l'adresse IP 8.8.8.8. Cette requête demande au destinataire (8.8.8.8) de répondre pour vérifier que la communication est possible.
   - **Echo (ping) Reply :** Le serveur DNS de Google (8.8.8.8) répond avec un message ICMP de type "Echo Reply", indiquant que la requête a été reçue et que la communication est possible.

**5. Longueur (en octets) :**
   - Chaque paquet ICMP dans votre capture a une longueur de 74 octets. Cela inclut l'en-tête IP (20 octets), l'en-tête ICMP (8 octets) et les données (32 octets), plus un petit nombre d'octets supplémentaires pour l'encapsulation Ethernet.

**6. Informations Supplémentaires :**
   - **ID de Paquet (id) :** Chaque requête et réponse ICMP contient un identifiant unique (`id=0x0001` dans votre capture) pour que les paquets correspondants puissent être appariés.
   - **Numéro de Séquence (seq) :** Ce champ (`seq=1`, `seq=2`, etc.) permet de savoir dans quel ordre les paquets ont été envoyés et reçus.

### Tableau Récapitulatif Détails ICMP

| **No.** | **Heure (s)** | **Source IP**    | **Destination IP** | **Source MAC**           | **Destination MAC**      | **Protocole** | **Type**            | **Longueur (octets)** | **Infos**                      |
|---------|---------------|------------------|--------------------|--------------------------|--------------------------|---------------|---------------------|------------------------|---------------------------------|
| 1786    | 19.375981      | 10.90.140.11     | 8.8.8.8            | 00-93-37-E3-86-C2         | 80:24:8f:98:b2:7f         | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=1                |
| 1787    | 19.379116      | 8.8.8.8          | 10.90.140.11       | 80:24:8f:98:b2:7f         | 00-93-37-E3-86-C2         | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=1                |
| 1819    | 20.396209      | 10.90.140.11     | 8.8.8.8            | 00-93-37-E3-86-C2         | 80:24:8f:98:b2:7f         | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=2                |
| 1820    | 20.399701      | 8.8.8.8          | 10.90.140.11       | 80:24:8f:98:b2:7f         | 00-93-37-E3-86-C2         | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=2                |
| 1835    | 21.421384      | 10.90.140.11     | 8.8.8.8            | 00-93-37-E3-86-C2         | 80:24:8f:98:b2:7f         | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=3                |
| 1836    | 21.423240      | 8.8.8.8          | 10.90.140.11       | 80:24:8f:98:b2:7f         | 00-93-37-E3-86-C2         | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=3                |
| 1837    | 22.434479      | 10.90.140.11     | 8.8.8.8            | 00-93-37-E3-86-C2         | 80:24:8f:98:b2:7f         | ICMP          | Echo (ping) Request | 74                     | id=0x0001, seq=4                |
| 1838    | 22.438142      | 8.8.8.8          | 10.90.140.11       | 80:24:8f:98:b2:7f         | 00-93-37-E3-86-C2         | ICMP          | Echo (ping) Reply   | 74                     | id=0x0001, seq=4                |

### Points Clés pour Débutants

- **Ping et ICMP :** Le ping est un outil qui envoie des requêtes ICMP pour vérifier si une autre machine est joignable sur le réseau.
- **Adresse IP :** C'est l'identifiant numérique de votre appareil sur le réseau. L'adresse IP locale commence souvent par 10.x.x.x, tandis que l'adresse 8.8.8.8 est une adresse publique appartenant à Google.
- **Adresse MAC :** C'est l'identifiant matériel unique de votre carte réseau. Contrairement aux adresses IP, les adresses MAC ne changent pas lorsque vous vous déplacez dans différents réseaux.
- **Paquets ICMP :** Ils sont utilisés pour diagnostiquer les problèmes de connectivité. Lors d'un ping, deux types de paquets ICMP sont échangés : une requête et une réponse.

En comprenant ces concepts, vous pouvez analyser les paquets dans Wireshark et vérifier la connectivité entre votre machine et d'autres appareils sur le réseau.
![image4](https://github.com/user-attachments/assets/343591c1-835b-41a9-ba4e-4c2d1d70883c)
![image3](https://github.com/user-attachments/assets/ad6adef0-8f0e-4b83-9f1a-ba7d745eeab8)
![image2](https://github.com/user-attachments/assets/4db821a3-bbb2-4886-b4d8-88772affcffd)
![image](https://github.com/user-attachments/assets/a4826054-aee8-4f18-bb42-63109f925f66)
