# 02-Pratique 1 (correction complété)

# Attaque Man-in-the-Middle (MITM) : Tutoriel Complet 🛡️💻

Bienvenue sur le repository du projet MITM où nous vous guidons à travers la configuration et l'exécution d'une attaque MITM utilisant Ubuntu 22.04 sur VirtualBox. Ce projet est conçu à des fins éducatives et doit être utilisé dans un environnement contrôlé et éthique.

## Prérequis 📋

Avant de commencer, assurez-vous de disposer de :
- **VirtualBox** - [Téléchargez ici](https://www.virtualbox.org/)
- **Ubuntu 22.04 LTS** - [Téléchargez ici](https://ubuntu.com/download/desktop)
- **Trois machines virtuelles** configurées selon les instructions ci-dessous.

## Installation et Configuration 🛠️

### Étape 1: Préparation des machines virtuelles

1. **Téléchargez et installez VirtualBox.**
2. **Téléchargez l'image ISO d'Ubuntu 22.04 LTS.**
3. **Créez trois nouvelles machines virtuelles** (une pour l'attaquant, deux pour les victimes).

### Étape 2: Configuration du réseau

- **Configurer deux adaptateurs réseau pour chaque VM** :
  - **Adaptateur 1**: Bridge Adapter
  - **Adaptateur 2**: NAT Network (`natnet` avec l'adresse `10.0.2.0/24`)

### Étape 3: Configuration des adresses IP statiques

- **Configurer les IPs statiques** via `netplan` sur chaque VM pour permettre la communication interne.

## Exécution de l'Attaque 🔥

### Étape 1: Empoisonnement ARP

- Utilisez `arpspoof` pour tromper les machines victimes et rediriger leur trafic vers la machine attaquante.

### Étape 2: Intercepter le trafic

- Configurez `iptables` pour rediriger le trafic HTTP vers `mitmproxy` où vous pouvez visualiser et modifier les requêtes/réponses.

## Utilisation de `mitmproxy`

- **Lancer `mitmproxy`** sur la machine attaquante pour commencer à intercepter le trafic.

## Sécurité et Éthique 🔐

- **Attention** : Ce projet est destiné à des fins d'apprentissage sous supervision et dans un environnement légalement contrôlé. N'utilisez pas ces techniques sans autorisation explicite des parties impliquées.

## Contributeurs et Remerciements 🌟

- Merci à tous ceux qui ont contribué à ce projet. Vos insights et expertises sont inestimables pour faire de ce projet une ressource d'apprentissage réussie.
