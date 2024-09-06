#  Vulgarisation

#### 1 - Qu’est-ce que la résolution de noms ?

Imagine que tu as un groupe d’amis et que tu veux leur envoyer un message. Mais au lieu d'utiliser leur nom, tu ne connais que leur numéro de téléphone. C’est un peu compliqué, n’est-ce pas ? La **résolution de noms** est un système qui fait correspondre les "noms" (comme le prénom de ton ami) aux "adresses" (comme son numéro de téléphone). Dans un réseau d’ordinateurs, c’est pareil : les ordinateurs communiquent entre eux en utilisant des "adresses IP", mais il est plus facile d’utiliser des noms (comme `google.com` au lieu de l’adresse IP de Google).

#### 1.1 - Les trois méthodes pour résoudre les noms dans les réseaux Windows

1. **DNS (Domain Name System)** : 
   - Imagine un énorme carnet d’adresses. Chaque fois que tu veux appeler quelqu'un (par exemple, quand tu tapes `google.com`), le DNS va chercher dans ce carnet d’adresses pour te donner le "numéro de téléphone" (l’adresse IP) de la personne ou du service que tu veux joindre.
   - **Problème** : DNS, c’est super pour les grandes entreprises, mais si tu fais une petite fête avec tes amis chez toi (un petit réseau local), tu n’as pas besoin de ce gros carnet d’adresses compliqué. C’est là que les autres systèmes entrent en jeu.

2. **LLMNR (Link Local Multicast Name Resolution)** : 
   - Imagine que tu es dans une petite fête et que tu veux savoir qui est là, mais il n’y a pas de liste d’invités. Au lieu de demander à tout le monde individuellement (comme dans le DNS), tu lances un message dans la pièce en disant : "Hey, qui est Pierre ?" Tous les Pierre présents dans la salle vont répondre. C’est exactement ce que fait LLMNR dans un petit réseau : il envoie un message et les ordinateurs concernés répondent.
  
3. **NetBIOS/WINS** :
   - C’est comme un vieux système de se présenter dans les fêtes. Au lieu d'utiliser des messages modernes, c’est plus ancien et moins utilisé de nos jours, mais ça fonctionne encore pour certains vieux réseaux.

#### 1.2 - LLMNR expliqué simplement

**LLMNR**, c’est un peu comme le fait de crier dans une pièce pour savoir qui est là. Il est utilisé dans les petits réseaux, comme dans une maison ou un petit bureau, où il n’y a pas de "livre d’adresses" (pas de serveur DNS). 


- **Messages en multicast** : 
   - Au lieu d'envoyer un message à tout le monde (broadcast), LLMNR envoie son message seulement aux personnes qui pourraient être intéressées. Un peu comme si, au lieu de crier dans toute la fête "Où est Pierre ?", tu disais cela uniquement à ceux qui se trouvent dans le salon.
   
- **IPv6** : 
   - C’est comme utiliser une technologie de téléphone moderne pour passer tes appels au lieu d’un vieux téléphone fixe. LLMNR utilise des adresses modernes (IPv6) si elles sont disponibles.

- **Découverte réseau** : 
   - LLMNR ne fonctionne que si tes ordinateurs sont prêts à découvrir et se faire découvrir dans le réseau. C’est comme si la porte de la salle était ouverte pour que tout le monde puisse se parler.

#### LLMNR en détails techniques :

LLMNR utilise un port spécial (5355/UDP), comme si tu disais à tes amis "Envoyez-moi vos réponses uniquement sur ce canal de discussion spécifique". Voici aussi les adresses qu’il utilise :

- En **IPv4** (version classique) : Imagine un canal spécifique où tu lances tes messages, et tous les ordinateurs concernés écoutent sur ce canal.
- En **IPv6** (version moderne) : Pareil, mais sur un canal encore plus sophistiqué.

#### Pourquoi utiliser LLMNR dans la vraie vie ?

Si tu es dans une petite fête avec tes amis (un petit réseau), tu n’as pas besoin de mettre en place un système ultra-complexe comme le DNS. LLMNR te permet simplement de demander "Qui est là ?" sans avoir besoin d’un grand carnet d’adresses. C’est rapide, facile, et parfait pour des situations simples.
