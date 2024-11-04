# FAQ Technique # 1: Configuration d'une IP Statique sur une VM de Routeur sous VMware

Cette FAQ aborde en détail les étapes et solutions pour configurer correctement une adresse IP statique sur une machine virtuelle (VM) agissant comme routeur dans un environnement VMware. Ce document vise à clarifier les étapes essentielles et les solutions de dépannage pour surmonter les problèmes de configuration, notamment lorsque la VM obtient une adresse IP en autoconfiguration (APIPA).

---

### 1. Pourquoi ma VM de routeur prend-elle une adresse APIPA (169.254.x.x) au lieu de l’adresse IP statique configurée ?

Lorsque votre VM adopte une adresse IP de type APIPA (169.254.x.x), cela indique qu'elle ne parvient pas à obtenir une adresse IP valide. Ce comportement se produit généralement lorsque :
- La configuration manuelle de l'IP statique n'a pas été appliquée correctement.
- Il y a un conflit réseau empêchant la VM de communiquer correctement.
- Le mode réseau de la VM n’est pas adapté pour une connexion directe au réseau local.

---

### 2. Est-ce que désactiver les serveurs DHCP dans le réseau résout le problème d'adresse APIPA ?

Non, désactiver les serveurs DHCP ne résout pas le problème d’attribution d’une adresse IP statique. Lorsqu'une IP statique est configurée manuellement, la machine ne dépend pas du DHCP pour obtenir son adresse IP. Si la machine continue de prendre une adresse APIPA, cela signifie que la configuration de l’IP statique n’est pas correcte ou que des paramètres réseau empêchent son application.

---

### 3. Comment vérifier si le mode réseau de la VM est correctement configuré ?

1. **Accédez aux paramètres de la VM dans VMware** :
   - Ouvrez VMware, sélectionnez votre VM, puis allez dans **Settings**.
   - Sous **Network Adapter**, vérifiez que le mode est configuré sur **Bridge** (ponté).

2. **Pourquoi utiliser le mode "Bridge" ?**
   - Le mode Bridge permet à la VM de fonctionner comme un périphérique réseau indépendant, sur le même réseau que les autres machines. Ce mode est essentiel pour qu'une machine avec une IP statique puisse communiquer directement avec les autres périphériques du réseau. Les modes "NAT" ou "Host-Only" sont plus restrictifs et peuvent causer des interférences, forçant la machine à adopter une adresse APIPA.

---

### 4. Comment configurer manuellement une adresse IP statique dans Windows ?

1. **Ouvrez les paramètres réseau de la VM sous Windows** :
   - Allez dans **Panneau de configuration** > **Centre Réseau et partage** > **Modifier les paramètres de la carte**.
   
2. **Configurer les propriétés TCP/IP** :
   - Faites un clic droit sur l’interface réseau active et sélectionnez **Propriétés**.
   - Sélectionnez **Protocole Internet version 4 (TCP/IPv4)** et cliquez sur **Propriétés**.

3. **Entrer les informations IP statiques** :
   - Adresse IP : Par exemple, 192.168.100.254 (ou toute adresse dans la plage du sous-réseau configuré).
   - Masque de sous-réseau : 255.255.255.0 pour un /24.
   - Passerelle par défaut : Adresse de la passerelle pour le réseau.
   - Serveur DNS préféré : 192.168.100.10 ou une autre adresse DNS appropriée.

4. **Appliquer et vérifier** :
   - Après avoir entré les informations, désactivez et réactivez l’interface réseau, ou utilisez `ipconfig /renew` pour appliquer les changements.

---

### 5. Dois-je désactiver le service DHCP Client sur la VM de routeur ?

Oui, désactiver le service **DHCP Client** peut aider si vous configurez une IP statique, car ce service peut parfois interférer avec les paramètres de configuration manuelle.

1. **Désactiver le service DHCP Client** :
   - Ouvrez la console des services en tapant `services.msc` dans la barre de recherche Windows.
   - Localisez le service **DHCP Client** et définissez-le sur **Désactivé**.

2. **Pourquoi cette étape est-elle nécessaire ?**
   - Une machine configurée avec une adresse IP statique n’a pas besoin de ce service pour fonctionner. En le désactivant, vous éliminez une potentielle cause d’interférence qui pourrait forcer la machine à adopter une adresse APIPA.

---

### 6. Faut-il redémarrer la VM après avoir appliqué ces modifications ?

Oui, il est fortement recommandé de redémarrer la VM après avoir appliqué les modifications de configuration réseau. Un redémarrage permet de s'assurer que tous les paramètres sont pris en compte et supprime les configurations temporaires qui pourraient persister.

---

### 7. Comment vérifier que l’adresse IP statique est correctement appliquée après redémarrage ?

1. **Utiliser la commande `ipconfig /all`** :
   - Après le redémarrage, ouvrez une invite de commande et tapez `ipconfig /all`.
   - Vérifiez que l'adresse IP affichée correspond bien à l’adresse statique configurée et non à une adresse APIPA.

2. **Diagnostiquer si l'adresse est incorrecte** :
   - Si l’adresse reste en 169.254.x.x, cela signifie qu’il reste un problème dans la configuration réseau de la VM ou de l'environnement VMware.

---

### 8. Que faire si la VM continue de prendre une adresse APIPA malgré toutes les étapes ci-dessus ?

Si le problème persiste malgré la vérification et l'application de toutes les étapes précédentes, il pourrait y avoir d'autres facteurs en jeu :

1. **Désactiver l'autoconfiguration d'adresse IP (APIPA) dans le registre** :
   - Ouvrez l'éditeur de registre (`regedit`).
   - Allez dans la clé :
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
     ```
   - Créez une nouvelle valeur DWORD nommée `IPAutoconfigurationEnabled` et définissez-la à `0`.
   - Redémarrez la machine pour appliquer la modification.

2. **Réinitialisation de la pile TCP/IP** :
   - Parfois, une réinitialisation de la pile réseau peut résoudre les problèmes d’APIPA persistants.
   - Ouvrez une invite de commande en tant qu’administrateur et tapez les commandes suivantes :
     ```
     netsh int ip reset
     netsh winsock reset
     ```
   - Redémarrez la machine après ces commandes.

---

### 9. Est-il possible que des permissions administratives bloquent le service DHCP sur la VM de routeur ?

Oui, un manque de permissions administratives peut empêcher la VM de démarrer correctement le service DHCP. 

1. **Vérifiez les permissions** :
   - Assurez-vous que le compte utilisateur possède les droits administratifs complets.
   
2. **Redémarrer le service DHCP** :
   - Ouvrez `services.msc`, recherchez le service **DHCP Client** et tentez de le démarrer manuellement.
   - Si une erreur de permissions persiste, consultez les journaux d’événements Windows (`eventvwr.msc`) pour obtenir plus de détails.

---

### 10. Dois-je désactiver le service DHCP sur toutes les autres machines du réseau ?

Oui, si vous avez plusieurs serveurs DHCP actifs, cela peut entraîner des conflits. Assurez-vous que le DHCP n'est activé que sur les machines qui en ont besoin. Lorsque l’adresse IP est configurée manuellement, cette étape garantit l’absence d'interférence de serveurs DHCP multiples.

---

### 11. Est-il nécessaire de vérifier le fichier `hosts` et les paramètres DNS ?

Oui, des entrées incorrectes dans le fichier `hosts` ou des erreurs de configuration DNS peuvent empêcher l’attribution correcte d’une IP statique.

1. **Vérifier le fichier `hosts`** :
   - Le fichier `hosts` est situé dans `C:\Windows\System32\drivers\etc\hosts`.
   - Assurez-vous qu’il ne contient pas d’entrées pouvant interférer avec la résolution de noms pour votre configuration IP.

2. **Vérifiez les paramètres DNS** :
   - Assurez-vous que les paramètres DNS sont configurés avec un DNS local approprié ou un DNS public, comme 8.8.8.8, si applicable.

---

### Conclusion

Configurer une adresse IP statique sur une machine virtuelle dans VMware requiert une compréhension précise des paramètres réseau. Cette FAQ couvre toutes les étapes nécessaires pour diagnostiquer et résoudre le problème d’attribution d’adresse IP, y compris les vérifications de base, les ajustements dans VMware, et les modifications avancées du registre et de la pile TCP/IP. 

Si, après avoir suivi chaque étape, le problème persiste, envisagez de vérifier si votre image VM est corrompue ou de réinstaller le système. Pour des questions supplémentaires ou de l’assistance, veuillez fournir des captures d’écran détaillées de chaque étape afin de faciliter un diagnostic plus approfondi.
