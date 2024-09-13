
![image](https://github.com/user-attachments/assets/97abfa1a-2ff4-44fc-8365-30bfcfbec5ce)

![image](https://github.com/user-attachments/assets/77a719ed-4bc6-498d-ab2f-14468ae28b67)

# 1. Fonctionnement d'une requête DNS
Lorsqu'un client envoie une requête vers un serveur DNS, trois informations importantes sont envoyées :
- **i. Domaine DNS** : Le client DNS envoie un nom de domaine complet (Fully Qualified Domain Name, FQDN) au serveur DNS. Cela permet de résoudre l'adresse IP du domaine en question.
- **ii. Type de requête** : Il s'agit du type de ressource demandée, comme par exemple un enregistrement de type "A", qui permet d'obtenir l'adresse IPv4 associée au nom de domaine.
- **iii. Classe de la requête** : En général, la classe est toujours "IN" pour les requêtes Internet.

# Exemple
Si un client cherche à résoudre l’adresse IPv4 du domaine `pci.contoso.local`, il envoie une requête DNS pour obtenir un enregistrement de type "A". Le serveur DNS va chercher cet enregistrement et, s’il le trouve, renvoie l’adresse IP associée au client.

# 2. Méthodes de résolution DNS
L'image ci-dessous illustre les méthodes de résolution DNS avec un schéma représentant un échange entre le client DNS, le serveur DNS et le cache du serveur DNS.
- **Q1** : Le client DNS envoie une requête au serveur.
- **Q2** : Si l’enregistrement est trouvé dans le cache du serveur DNS, il est renvoyé immédiatement au client (Réponse).
- **Q3** : Si l’enregistrement n’est pas dans le cache, le serveur DNS peut faire une requête à d’autres serveurs DNS pour obtenir l'enregistrement.

En résumé, une requête DNS passe par plusieurs étapes pour obtenir la résolution du nom de domaine, en s'appuyant sur les serveurs DNS locaux et parfois sur des serveurs externes si l'information n'est pas en cache.

---
# Annexe : 
---


### Qu'est-ce qu'une requête DNS ?
DNS, ou Domain Name System, est comme un **annuaire téléphonique d'Internet**. Imagine que tu veux appeler quelqu'un, mais tu ne connais pas son numéro de téléphone. Tu as juste son nom. Le DNS fonctionne exactement de la même manière, sauf qu’au lieu de chercher un numéro de téléphone, tu cherches une **adresse IP** (qui est l'équivalent du "numéro de téléphone" pour les ordinateurs).

Quand tu tapes une adresse web dans ton navigateur (comme `www.google.com`), ton ordinateur a besoin de savoir où se trouve ce site sur Internet, c’est-à-dire son **adresse IP**. La requête DNS est une demande envoyée à un serveur DNS pour qu'il te dise quelle est l'adresse IP associée à ce nom de domaine.

### Les 3 informations envoyées dans une requête DNS
Voici ce qui se passe concrètement quand tu envoies une requête DNS :

1. **Le nom de domaine** : C'est l'adresse que tu tapes dans ton navigateur, comme `www.example.com`. Ton ordinateur envoie cette adresse au serveur DNS pour qu'il puisse trouver l'IP correspondante.
   
2. **Le type de requête** : C'est ce que tu cherches exactement. Par exemple, si tu veux l'adresse IP (la carte d'identité de l'ordinateur), ton ordinateur envoie une requête de type "A". Il demande en gros : "Donne-moi l’adresse IPv4 de ce site."

3. **La classe** : En général, on utilise la classe "IN", qui signifie Internet. C'est juste pour dire que la demande concerne un site sur Internet, pas autre chose.

### Exemple 
Imaginons que tu veuilles aller chez ton ami, mais tu ne connais pas son adresse. Tu demandes donc à quelqu'un : "Où habite mon ami ?". Cette personne te répond en cherchant dans un annuaire ou en demandant à d'autres personnes. C’est exactement ce que fait le DNS.

Par exemple :
- Tu veux visiter un site appelé `pci.contoso.local`.
- Ton ordinateur ne connaît pas l'adresse de ce site. Il demande alors au serveur DNS : "Peux-tu me donner l’adresse IP de `pci.contoso.local` ?"
- Le serveur DNS cherche cette adresse dans ses données. S’il la trouve, il te la renvoie. Sinon, il demande à d’autres serveurs pour obtenir la réponse.

### Comment ça marche (Méthodes de résolution DNS) ?
Quand tu fais une recherche sur Internet, plusieurs étapes sont suivies pour que tu puisses trouver l’adresse du site :

1. **Ton ordinateur demande à un serveur DNS** : Imaginons que c'est comme demander à ton voisin s'il connaît l'adresse. 
   
2. **Le serveur DNS vérifie son cache** : Si le serveur a déjà cherché cette adresse avant, il l’a peut-être gardée en mémoire. S’il la trouve, il te la donne immédiatement (comme si ton voisin se souvenait de l’adresse sans avoir besoin de vérifier).

3. **Le serveur DNS demande à d'autres serveurs DNS** : Si le serveur DNS ne connaît pas l’adresse, il va demander à d'autres serveurs DNS dans le monde. C’est comme si ton voisin demandait à d’autres voisins pour trouver l’information.

Quand le serveur DNS obtient l'adresse IP, il te la renvoie. Ton ordinateur peut alors se connecter directement au site.

### Pourquoi c’est important ?
Sans DNS, tu devrais te souvenir des adresses IP (qui ressemblent à des chiffres compliqués comme `216.58.207.46`) pour chaque site que tu visites. Ça serait un cauchemar ! Heureusement, le DNS te permet de juste taper `www.google.com` et il fait tout le travail compliqué pour toi en arrière-plan.


