# README - Étude de Cas : Configuration IP d'un Routeur Virtuel sous VMware

## Contexte

Dans le cadre de ce projet, un routeur virtuel sous VMware a été configuré pour relier deux sous-réseaux distincts (192.168.100.0/24 et 192.168.200.0/24). Le routeur virtuel doit être capable de communiquer avec les autres machines présentes sur ces sous-réseaux et jouer le rôle de relais DHCP entre les deux segments réseau. Cependant, des difficultés techniques ont été rencontrées lors de la configuration de l'adresse IPv4 fixe sur la machine virtuelle (VM) jouant le rôle de routeur. La VM ne parvient pas à conserver l'adresse IP fixe spécifiée et adopte systématiquement une adresse d’autoconfiguration de type APIPA (Automatic Private IP Addressing).

Ce document fournit une analyse détaillée du problème ainsi qu'un guide de dépannage exhaustif pour résoudre ce type de dysfonctionnement dans un environnement de virtualisation VMware.

## Analyse du problème

Lorsque la configuration réseau d'une machine virtuelle bascule vers une adresse APIPA (par exemple, une adresse dans la plage 169.254.x.x), cela signifie qu'elle ne parvient pas à obtenir une adresse IP valide de manière statique ou dynamique (via DHCP). Dans le cas d'une adresse IP statique, ce comportement est anormal et résulte généralement d'erreurs dans la configuration réseau, telles que :

1. **Paramètres réseau inadaptés dans VMware** : Le mode réseau de la machine virtuelle pourrait ne pas être configuré correctement pour permettre la communication avec d’autres machines sur le réseau.
2. **Configuration manuelle incorrecte de l'adresse IP** : L'adresse IP fixe spécifiée pourrait ne pas être conforme au plan d'adressage réseau, ou le masque de sous-réseau, la passerelle et le DNS pourraient être mal configurés.
3. **Interférences du service DHCP** : Si le service DHCP est activé sur plusieurs machines ou s'il y a des erreurs de permissions dans la configuration du service DHCP, cela peut causer des conflits d'adressage IP.
4. **Problèmes dans la pile TCP/IP de Windows** : Des anomalies dans la pile réseau Windows de la VM peuvent également empêcher la machine d'utiliser correctement une adresse IP statique.

## Guide de Dépannage (Troubleshooting)

### 1. Vérification des paramètres réseau dans VMware

   - **Mode Bridge** : Il est recommandé de configurer l'interface réseau de la machine virtuelle en mode "Bridge" dans VMware. Ce mode permet à la VM de se comporter comme un périphérique réseau indépendant sur le même réseau que les autres machines physiques et virtuelles. D’autres modes, tels que "NAT" ou "Host-Only", peuvent restreindre la connectivité réseau de la VM et empêcher une configuration IP correcte.
   - **Validation des changements** : Après avoir appliqué le mode Bridge, redémarrer la VM pour s'assurer que la configuration réseau est bien prise en compte.

### 2. Configuration manuelle de l’adresse IP

   Pour configurer correctement une adresse IP statique sur la VM, suivre ces étapes :

   - Ouvrir les **Paramètres réseau** dans Windows (Panneau de configuration > Centre Réseau et partage > Modifier les paramètres de la carte).
   - Clic droit sur l’interface réseau active > **Propriétés** > sélectionner **Protocole Internet version 4 (TCP/IPv4)** > **Propriétés**.
   - Sélectionner "Utiliser l’adresse IP suivante" et renseigner les informations comme suit :
     - **Adresse IP** : Choisir une adresse appropriée pour le sous-réseau (ex. 192.168.100.254 pour un sous-réseau 192.168.100.0/24).
     - **Masque de sous-réseau** : 255.255.255.0 pour un /24.
     - **Passerelle par défaut** : Configurer la passerelle en fonction du plan réseau.
     - **Serveur DNS préféré** : Utiliser un serveur DNS local ou un DNS public comme 8.8.8.8, si requis.
   - Appliquer les paramètres, puis désactiver et réactiver la connexion réseau pour s'assurer que les modifications sont prises en compte.

### 3. Désactivation de l’autoconfiguration d'adresse IP (APIPA)

   Lorsque le routeur obtient une adresse dans la plage 169.254.x.x, cela signifie que l'APIPA est activé. Pour désactiver cette fonctionnalité :

   - Ouvrir l’éditeur de registre (`regedit`).
   - Accéder à la clé suivante : 
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
     ```
   - Créer une nouvelle valeur DWORD nommée `IPAutoconfigurationEnabled` et définir sa valeur à `0` pour désactiver APIPA.
   - Redémarrer la machine après avoir appliqué cette modification pour que les changements soient effectifs.

### 4. Vérification du service DHCP

   Si le service DHCP refuse de démarrer ou indique une erreur de permissions, des droits administratifs supplémentaires peuvent être nécessaires pour effectuer des modifications sur la VM.

   - **Vérification des permissions** : S'assurer que l'utilisateur dispose des droits administratifs sur la machine.
   - **Redémarrage du service DHCP** :
     - Ouvrir `services.msc`, rechercher le service DHCP et vérifier son état.
     - Si le service ne fonctionne pas, essayer de le redémarrer manuellement et vérifier les journaux d'événements Windows (`eventvwr.msc`) pour toute erreur spécifique liée au service DHCP.

### 5. Réinitialisation de la pile TCP/IP

   Si les étapes précédentes ne résolvent pas le problème, une réinitialisation de la pile TCP/IP peut aider à restaurer les paramètres réseau par défaut de la VM :

   - Ouvrir une invite de commande en tant qu’administrateur.
   - Exécuter les commandes suivantes :
     ```
     netsh int ip reset
     netsh winsock reset
     ```
   - Redémarrer la machine pour appliquer ces modifications.

### 6. Vérification des fichiers Hosts et des paramètres DNS

   Dans certains cas, des configurations incorrectes dans le fichier `hosts` ou des erreurs dans la configuration DNS peuvent empêcher l’utilisation correcte d’une adresse IP statique.

   - **Fichier Hosts** : Ouvrir le fichier `hosts` situé dans `C:\Windows\System32\drivers\etc\hosts` et vérifier qu'il n'y a pas d’entrées qui pourraient interférer avec la résolution de noms pour l’adresse statique.
   - **Serveur DNS** : S'assurer que les paramètres DNS sont correctement configurés. Utiliser un DNS local ou un serveur public (par ex. 8.8.8.8) pour tester si le problème persiste.

### 7. Désactivation temporaire du service DHCP sur le routeur

   Si plusieurs serveurs DHCP sont actifs sur le réseau, cela peut causer des conflits d’adressage IP. S'assurer que le service DHCP n'est activé que sur les machines qui en ont réellement besoin, avant de configurer manuellement l'adresse IP du routeur.

## Conclusion

La configuration d'une adresse IP fixe sur une machine virtuelle nécessite une bonne compréhension des paramètres réseau et de la topologie du réseau. Ce guide de dépannage fournit des étapes détaillées pour diagnostiquer et résoudre les problèmes liés à l’attribution d’une adresse IP statique dans un environnement VMware.

Dans le cas où toutes ces étapes auraient été suivies et que le problème persiste, il pourrait être judicieux de vérifier si l’image de la machine virtuelle est corrompue ou si une réinstallation du système pourrait résoudre d’éventuels problèmes sous-jacents.

Ce document a pour objectif de guider les utilisateurs dans la résolution de ce type de problème de manière autonome et de renforcer leur compréhension des configurations réseau de base dans un environnement virtualisé.
