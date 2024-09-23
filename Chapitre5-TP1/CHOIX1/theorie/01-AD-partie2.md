# Active Directory : Approfondissement

**Active Directory** est un système d'annuaire qui utilise plusieurs concepts clés pour organiser et gérer les ressources dans un réseau Windows. Voici quelques points plus détaillés pour mieux comprendre son fonctionnement.

#### 1. **Composants principaux d'Active Directory**

- **Domaines** : Un **domaine** est un regroupement logique d’objets (utilisateurs, ordinateurs, groupes) qui partagent la même base de données Active Directory. Il constitue la frontière de sécurité : toutes les ressources dans un domaine sont soumises aux mêmes règles de sécurité et aux mêmes politiques d'administration.
  
- **Forêts** : Une **forêt** est un regroupement de plusieurs domaines Active Directory. Tous les domaines d'une forêt partagent un schéma commun (les règles qui définissent les objets dans AD) et des relations de confiance, permettant ainsi la communication et l'accès aux ressources entre les domaines.
  
- **Unités d'Organisation (OU)** : Les **OU** sont des sous-ensembles dans un domaine qui permettent de structurer les objets de manière hiérarchique. Elles sont souvent utilisées pour refléter la structure d'une entreprise, par exemple avec des OU pour chaque département (RH, IT, Finance, etc.). Les OU facilitent l'application de **GPO (Group Policy Objects)** à des groupes spécifiques sans affecter l'ensemble du domaine.

#### 2. **Objets dans Active Directory**

Active Directory est constitué d'objets, et chaque objet représente une ressource individuelle du réseau. Voici les principaux types d'objets :

- **Utilisateurs** : Les comptes utilisateurs permettent à des individus d'accéder aux ressources réseau avec un nom d'utilisateur et un mot de passe. Chaque utilisateur est un objet unique dans Active Directory avec des attributs (nom, email, etc.).
  
- **Groupes** : Les groupes permettent de gérer l'accès aux ressources en affectant des permissions à un groupe d'utilisateurs plutôt qu'à chaque utilisateur individuellement. Il existe deux types de groupes :
  - **Groupes de sécurité** : utilisés pour attribuer des permissions sur les ressources.
  - **Groupes de distribution** : utilisés pour créer des listes de diffusion d’emails.
  
- **Ordinateurs** : Les ordinateurs dans un réseau sont également des objets dans AD, ce qui permet aux administrateurs de gérer et sécuriser les machines de manière centralisée.

- **Imprimantes et autres ressources réseau** : Les périphériques tels que les imprimantes, serveurs de fichiers, et autres ressources réseau peuvent également être inclus dans l’annuaire pour faciliter leur gestion.

#### 3. **Authentification et Autorisation**

Active Directory joue un rôle central dans l'**authentification** (qui vous êtes) et l'**autorisation** (à quoi vous avez accès) dans un réseau Windows.

- **Authentification** : AD utilise le protocole **Kerberos** pour authentifier les utilisateurs et les appareils. Lorsqu'un utilisateur se connecte au réseau, son identité est vérifiée par AD, qui génère ensuite un ticket d'authentification pour accéder aux ressources autorisées.
  
- **Autorisation** : Après authentification, AD vérifie les droits et permissions associées au compte utilisateur ou au groupe auquel il appartient pour déterminer quelles ressources sont accessibles et quelles actions peuvent être effectuées.

#### 4. **Contrôleur de domaine (Domain Controller - DC)**

Le **contrôleur de domaine** est un serveur qui exécute Active Directory et gère les requêtes d'authentification. Il stocke une copie complète de la base de données Active Directory pour le domaine qu'il sert et est chargé de :

- Authentifier les utilisateurs.
- Appliquer les politiques de sécurité du domaine.
- Répliquer les changements d'AD avec d'autres contrôleurs de domaine dans le même domaine ou dans d'autres domaines de la forêt.

La redondance des contrôleurs de domaine dans un réseau est cruciale pour assurer la disponibilité et la résilience des services d'annuaire.

#### 5. **LDAP (Lightweight Directory Access Protocol)**

Active Directory repose sur le **protocole LDAP**, un standard de communication pour accéder et modifier des services d’annuaires. Les applications et les services utilisent LDAP pour interagir avec AD, par exemple pour récupérer des informations sur les utilisateurs ou vérifier les permissions.

#### 6. **Sites et réplication**

Active Directory est conçu pour fonctionner dans des environnements distribués géographiquement. Les **sites AD** sont des regroupements de sous-réseaux qui reflètent la structure physique du réseau. Les contrôleurs de domaine dans différents sites synchronisent les informations d’AD par un processus appelé **réplication**.

- **Réplication intra-site** : Réplication entre des contrôleurs de domaine dans le même site. Elle est rapide et se produit fréquemment.
- **Réplication inter-site** : Réplication entre des contrôleurs de domaine dans des sites différents. Elle est optimisée pour être plus économique en bande passante, se produisant à intervalles réguliers ou programmés.

#### 7. **Group Policy Objects (GPO)**

Les **GPO** sont des ensembles de règles et de configurations appliquées à des utilisateurs et des ordinateurs dans un domaine AD. 
Elles permettent aux administrateurs de définir des configurations spécifiques (comme des politiques de sécurité, des scripts de démarrage, des installations de logiciels, etc.) et de les appliquer de manière centralisée. Les GPO peuvent être associées à des domaines, des unités d'organisation, ou même des sites.

