### L'Active Directory - Partie 1

#### Qu'est-ce que l'Active Directory ?

L'Active Directory (AD) est un service d'annuaire développé par Microsoft pour les systèmes Windows. Il permet de centraliser deux fonctions essentielles dans une organisation : **l'authentification** et **l'identification** des utilisateurs et des ressources (ordinateurs, imprimantes, etc.). 

#### Pourquoi utiliser un annuaire comme l'Active Directory ?

Les avantages principaux sont :
- **Administration centralisée** : Toutes les informations sur les utilisateurs et ordinateurs sont stockées à un seul endroit, ce qui simplifie la gestion.
- **Unification de l'authentification** : Un seul compte utilisateur peut être utilisé pour se connecter à tous les ordinateurs du réseau.
- **Identification des objets sur le réseau** : Chaque objet (utilisateur, ordinateur) a une identité unique dans le réseau.
- **Simplification de la gestion** : Grâce à des **unités d'organisation** (OUs), les objets peuvent être organisés logiquement, facilitant la gestion et la délégation de tâches.

#### Structure de l'Active Directory

1. **Les classes et les attributs** : Les objets (comme les utilisateurs) dans AD sont organisés en **classes**. Chaque classe a des **attributs** spécifiques, par exemple, un utilisateur a un nom, un mot de passe, etc. Les **groupes** permettent de regrouper plusieurs utilisateurs pour simplifier la gestion. Les **unités d'organisation (OUs)** fonctionnent comme des dossiers pour organiser les objets.

2. **Le schéma** : C'est la structure qui définit toutes les classes et les attributs d'objets dans l'AD. Pensez à cela comme une "carte" qui décrit ce que vous pouvez créer et gérer dans l'AD.

3. **Les partitions d'annuaire** :
   - **Partition de schéma** : Contient les définitions des classes et attributs d'objets.
   - **Partition de configuration** : Contient des informations sur la topologie du réseau, comme les relations entre les domaines.
   - **Partition de domaine** : Stocke les informations des objets pour un domaine spécifique.

#### Groupe de travail vs Domaine

- **Modèle "Groupe de travail"** : Chaque machine a sa propre base d’utilisateurs, ce qui rend la gestion compliquée lorsqu'il y a beaucoup d'ordinateurs et d'utilisateurs.
- **Modèle "Domaine"** : Toutes les informations sont centralisées dans l'Active Directory. Un seul compte utilisateur est nécessaire pour accéder à toutes les ressources du réseau.

#### Les contrôleurs de domaine

Un **contrôleur de domaine** est un serveur qui gère l'AD pour un domaine. Il traite les demandes d'authentification, applique les politiques de sécurité, et stocke une copie de la base de données AD, représentée par le fichier **NTDS.dit**. Il est essentiel d'avoir plusieurs contrôleurs de domaine pour assurer la disponibilité du service grâce à la **réplication**.

#### Symbolisation d’un domaine

Dans un schéma AD, un domaine est souvent représenté par un triangle. Ce domaine peut être subdivisé en **sous-domaines**, par exemple pour gérer des succursales dans différentes villes. Ces sous-domaines forment un **arbre**, et plusieurs arbres peuvent être regroupés dans une **forêt**.

#### Arbre et Forêt

- Un **arbre** est une hiérarchie de domaines partageant un même espace de noms, comme `paris.it-connect.local` et `londres.it-connect.local`.
- Une **forêt** est un ensemble de plusieurs arbres. Les domaines dans une forêt partagent un schéma et un catalogue global, facilitant les communications entre eux, mais chaque arbre conserve son indépendance.

#### Niveau fonctionnel

Le **niveau fonctionnel** détermine les fonctionnalités de l'Active Directory en fonction de la version du système d'exploitation utilisé. Plus le niveau est élevé, plus vous bénéficiez des fonctionnalités récentes. Il est important de maintenir un niveau fonctionnel compatible avec les systèmes les plus récents pour pouvoir ajouter de nouveaux contrôleurs de domaine.

---

### Conclusion

L'Active Directory permet de centraliser et d'organiser l'administration des utilisateurs et des ressources au sein d'un réseau d'entreprise. Grâce à ses concepts d'arbres, de forêts, et de contrôleurs de domaine, AD offre une solution flexible et robuste pour gérer de grandes infrastructures informatiques.
