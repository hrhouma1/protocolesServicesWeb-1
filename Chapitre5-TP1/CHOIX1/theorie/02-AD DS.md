# 2. Qu'est-ce que **Active Directory Domain Services (AD DS)** ?

**Active Directory Domain Services (AD DS)** est l'un des services les plus importants fournis par **Active Directory**. AD DS est la fonction principale qui permet à Active Directory de gérer les utilisateurs, les ordinateurs et les ressources d’un réseau dans un domaine.

#### Fonctionnement d'AD DS

AD DS gère l'authentification et l'autorisation dans un réseau. Il stocke les informations sur les objets réseau (comme les utilisateurs, les ordinateurs, et les groupes) et permet aux administrateurs de contrôler qui peut accéder à ces ressources et quelles actions peuvent être effectuées. En d’autres termes, AD DS est le cœur du service Active Directory dans un environnement Windows.

Voici quelques éléments clés :

#### 1. **Stockage d'objets et base de données Active Directory**
AD DS contient la **base de données Active Directory**, qui stocke toutes les informations sur les objets du réseau, comme :
- Les comptes utilisateurs.
- Les comptes ordinateurs.
- Les groupes de sécurité.
- Les stratégies appliquées via les GPO.

Cette base de données est stockée sur les **contrôleurs de domaine (DC)**, des serveurs qui hébergent AD DS et gèrent les demandes d’authentification.

#### 2. **Rôles des contrôleurs de domaine**
Un **contrôleur de domaine** (ou Domain Controller) est un serveur exécutant AD DS, chargé de répondre aux demandes d'authentification des utilisateurs, de stocker et répliquer la base de données Active Directory, et d'appliquer les politiques de sécurité.

AD DS permet la gestion centralisée de l'authentification en :
- **Authentifiant** les utilisateurs lorsqu'ils tentent d'accéder au réseau.
- **Autorisant** ou refusant l'accès aux ressources réseau en fonction des permissions attribuées à l’utilisateur ou au groupe auquel il appartient.

#### 3. **Forêts, Domaines et Unités d'Organisation (OU)**
AD DS repose sur une architecture hiérarchique, facilitant la gestion des ressources réseau :

- **Forêt** : La structure de plus haut niveau dans Active Directory. Une forêt peut contenir un ou plusieurs **domaines** qui partagent le même schéma (structure des données).
- **Domaine** : Un regroupement logique d'objets dans une forêt. Chaque domaine possède son propre ensemble d’utilisateurs, ordinateurs, et objets, mais partage des relations de confiance avec les autres domaines de la forêt.
- **Unité d'Organisation (OU)** : Un conteneur utilisé pour regrouper des objets dans un domaine. Les OUs permettent une gestion granulaire, car des politiques peuvent être appliquées à des groupes spécifiques d'objets dans un domaine.

#### 4. **Réplication des données**
AD DS assure la **réplication** des données entre les contrôleurs de domaine afin de garantir que tous les contrôleurs de domaine dans un domaine possèdent les mêmes informations à jour. La réplication peut être :
- **Intra-site** : Réplication entre des contrôleurs de domaine dans le même site physique.
- **Inter-site** : Réplication entre des contrôleurs de domaine dans des sites différents, optimisée pour minimiser la bande passante utilisée.

#### 5. **Authentification via Kerberos**
L'authentification dans un domaine AD DS est assurée par le **protocole Kerberos**. Lorsqu’un utilisateur tente d’accéder au réseau, Kerberos émet un **ticket d’authentification** après avoir vérifié ses informations d’identification. Ce ticket est ensuite utilisé pour accéder aux ressources du domaine.

#### 6. **Group Policy Objects (GPO) et sécurité**
AD DS permet d'appliquer des **GPO** (Group Policy Objects) à des utilisateurs et des ordinateurs. Ces GPO définissent des règles et des configurations qui s'appliquent à différents objets du réseau, comme :
- Les configurations de sécurité.
- Les stratégies de mot de passe.
- L’installation de logiciels.

#### Avantages d'AD DS
- **Centralisation** : AD DS permet une gestion centralisée des utilisateurs, ordinateurs, et permissions dans un réseau.
- **Scalabilité** : Il peut être utilisé dans de petites ou grandes infrastructures, grâce à son architecture hiérarchique qui permet une gestion efficace des ressources.
- **Sécurité** : AD DS fournit un contrôle granulaire des accès aux ressources grâce aux GPO et aux groupes de sécurité.

### Résumé

**AD DS** est l'élément central d'Active Directory, permettant la gestion des utilisateurs, des ordinateurs et des ressources réseau. Il stocke les informations dans une base de données centralisée et fournit des services d'authentification, d'autorisation, et de gestion des politiques à travers un réseau, garantissant une gestion sécurisée et efficace.

