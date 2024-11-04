# 3. Concepts Clés des Services Web

---

## Table des matières
- [3.1 Méthodes HTTP](#methodes-http)
  - [3.1.1 GET](#get)
  - [3.1.2 POST](#post)
  - [3.1.3 PUT](#put)
  - [3.1.4 DELETE](#delete)
  - [3.1.5 Autres Méthodes HTTP (PATCH, HEAD, OPTIONS)](#autres-methodes)
- [3.2 Codes de Statut HTTP](#codes-status-http)
  - [3.2.1 Catégories de Codes de Statut](#categories-codes-status)
  - [3.2.2 Codes de Statut Courants](#codes-status-courants)
- [3.3 En-têtes HTTP](#entetes-http)
  - [3.3.1 En-têtes de Requête](#entetes-requete)
  - [3.3.2 En-têtes de Réponse](#entetes-reponse)
  - [3.3.3 En-têtes Généraux](#entetes-generaux)
  - [3.3.4 En-têtes Personnalisés](#entetes-personnalises)
- [Conclusion](#conclusion)

---

### <a name="methodes-http">3.1 Méthodes HTTP</a>

Les méthodes HTTP (ou verbes HTTP) définissent les actions possibles que le client peut demander au serveur de réaliser sur une ressource. Ces méthodes respectent les principes de l'architecture REST et permettent de manipuler les ressources de manière standardisée.

#### <a name="get">3.1.1 GET</a>

La méthode **GET** est utilisée pour récupérer des informations d'une ressource sans effectuer de modifications sur celle-ci. Elle est utilisée pour obtenir des données, comme afficher une page web ou récupérer des informations sur un utilisateur.

- **Caractéristiques** :
  - **Idempotente** : L’appel de cette méthode plusieurs fois n’a pas d’effet supplémentaire.
  - **Sans impact** : Ne modifie pas la ressource.
  - **Peut être mise en cache** : Les réponses GET peuvent être stockées pour un accès ultérieur.

- **Exemple** :
  - `GET /api/users/123` : Récupère les informations de l’utilisateur avec l'ID 123.

#### <a name="post">3.1.2 POST</a>

La méthode **POST** est utilisée pour envoyer des données au serveur afin de créer une nouvelle ressource. Contrairement à GET, elle a un effet sur le serveur et modifie l'état de la ressource.

- **Caractéristiques** :
  - **Non idempotente** : Appeler POST plusieurs fois peut entraîner la création de plusieurs ressources.
  - **Utilisée pour les formulaires** : Souvent utilisée pour envoyer des formulaires ou des données à traiter.
  
- **Exemple** :
  - `POST /api/users` avec un corps de requête contenant les informations de l'utilisateur : Crée un nouvel utilisateur.

#### <a name="put">3.1.3 PUT</a>

La méthode **PUT** est utilisée pour mettre à jour une ressource existante ou créer une ressource si elle n'existe pas. Elle envoie l’état complet de la ressource au serveur.

- **Caractéristiques** :
  - **Idempotente** : Appeler PUT plusieurs fois avec les mêmes données donne toujours le même résultat.
  - **Remplace** : Remplace l’ensemble des données de la ressource cible.

- **Exemple** :
  - `PUT /api/users/123` avec un corps de requête contenant des données mises à jour : Met à jour les informations de l’utilisateur avec l'ID 123.

#### <a name="delete">3.1.4 DELETE</a>

La méthode **DELETE** est utilisée pour supprimer une ressource. Elle indique au serveur qu’il doit supprimer la ressource spécifiée.

- **Caractéristiques** :
  - **Idempotente** : Si la ressource est déjà supprimée, appeler DELETE de nouveau n'aura pas d'effet supplémentaire.
  - **Action destructrice** : Supprime définitivement la ressource.

- **Exemple** :
  - `DELETE /api/users/123` : Supprime l’utilisateur avec l'ID 123.

#### <a name="autres-methodes">3.1.5 Autres Méthodes HTTP (PATCH, HEAD, OPTIONS)</a>

- **PATCH** : Met à jour partiellement une ressource. Contrairement à PUT, PATCH modifie seulement certains champs de la ressource.
- **HEAD** : Similaire à GET, mais ne retourne que les en-têtes de la réponse, sans le corps. Utilisé pour vérifier si une ressource existe.
- **OPTIONS** : Utilisée pour décrire les options de communication pour la ressource cible. Souvent utilisée pour les requêtes CORS (Cross-Origin Resource Sharing).

[Retour en haut](#)

---

### <a name="codes-status-http">3.2 Codes de Statut HTTP</a>

Les codes de statut HTTP sont des codes numériques que le serveur renvoie pour indiquer le résultat de la requête. Ils permettent de savoir si une requête a été traitée avec succès, si une erreur s'est produite, ou si des actions supplémentaires sont requises.

#### <a name="categories-codes-status">3.2.1 Catégories de Codes de Statut</a>

Les codes de statut HTTP sont divisés en plusieurs catégories :

- **1xx (Informational)** : Requêtes informatives. Indiquent que la requête est en cours de traitement.
- **2xx (Success)** : Succès. La requête a été reçue, comprise, et acceptée.
- **3xx (Redirection)** : Redirection. Indique que le client doit effectuer une autre action pour obtenir une réponse.
- **4xx (Client Error)** : Erreur du client. La requête contient des erreurs (ex. : mauvaise syntaxe, ressource inexistante).
- **5xx (Server Error)** : Erreur du serveur. Le serveur a rencontré une situation qu’il ne sait pas gérer.

#### <a name="codes-status-courants">3.2.2 Codes de Statut Courants</a>

Quelques codes de statut courants incluent :

- **200 OK** : La requête a réussi et la réponse contient le résultat demandé.
- **201 Created** : La requête a réussi et une nouvelle ressource a été créée.
- **204 No Content** : La requête a réussi, mais il n'y a pas de contenu à retourner.
- **400 Bad Request** : La requête est mal formée ou contient une erreur de syntaxe.
- **401 Unauthorized** : La requête nécessite une authentification.
- **403 Forbidden** : Le serveur comprend la requête mais refuse de l’exécuter.
- **404 Not Found** : La ressource demandée n’a pas été trouvée.
- **500 Internal Server Error** : Le serveur a rencontré une erreur imprévue.
- **503 Service Unavailable** : Le serveur est temporairement indisponible (surcharge ou maintenance).

[Retour en haut](#)

---

### <a name="entetes-http">3.3 En-têtes HTTP</a>

Les en-têtes HTTP fournissent des informations supplémentaires sur la requête ou la réponse. Ils peuvent contenir des informations sur le type de contenu, les paramètres de cache, les autorisations, et bien plus.

#### <a name="entetes-requete">3.3.1 En-têtes de Requête</a>

Les en-têtes de requête sont envoyés par le client pour fournir des informations supplémentaires sur la requête ou sur le client lui-même.

- **Host** : Indique le nom de domaine du serveur.
- **Authorization** : Contient les informations d’authentification (ex. : `Bearer token` pour les API sécurisées).
- **Content-Type** : Indique le type de contenu de la requête (ex. : `application/json`).
- **Accept** : Indique les formats de réponse acceptés par le client (ex. : `text/html`, `application/json`).

#### <a name="entetes-reponse">3.3.2 En-têtes de Réponse</a>

Les en-têtes de réponse sont envoyés par le serveur pour fournir des informations supplémentaires sur la réponse.

- **Content-Type** : Indique le type de contenu de la réponse.
- **Content-Length** : Indique la taille du contenu en octets.
- **Set-Cookie** : Utilisé pour envoyer des cookies au client, qui peuvent être stockés et renvoyés dans des requêtes futures.
- **Cache-Control** : Contrôle la manière dont la réponse doit être mise en cache par le client.

#### <a name="entetes-generaux">3.3.3 En-têtes Généraux</a>

Les en-têtes généraux peuvent apparaître à la fois dans les requêtes et les réponses, et fournissent des informations générales sur le message HTTP.

- **Connection** : Indique si la connexion doit être maintenue ouverte ou fermée après la transaction.
- **Date** : Indique la date et l'heure à laquelle le message a été envoyé.

#### <a name="entetes-personnalises">3.3.4 En-têtes Personnalisés</a>

Les en-têtes personnalisés sont définis par les développeurs pour répondre à des besoins

 spécifiques. Ils sont souvent précédés par `X-`, bien que cette convention ne soit plus obligatoire.

- **X-Custom-Header** : Peut contenir des informations spécifiques à l'application.
- **X-RateLimit-Limit** : Utilisé par certaines API pour indiquer le nombre maximum de requêtes autorisées par unité de temps.

[Retour en haut](#)

---

### <a name="conclusion">Conclusion</a>

Les concepts clés des services web - méthodes HTTP, codes de statut, et en-têtes - sont essentiels pour comprendre le fonctionnement des API et de la communication entre clients et serveurs. Les méthodes HTTP permettent de définir les actions sur les ressources, les codes de statut informent le client sur le succès ou l’échec de la requête, et les en-têtes offrent des informations supplémentaires nécessaires pour un échange de données efficace et sécurisé.

[Retour en haut](#)
