# 3. Qu'est-ce qu'une **GPO (Group Policy Object)** ?

Une **Group Policy Object (GPO)** est un ensemble de configurations et de règles que les administrateurs système peuvent utiliser pour contrôler et gérer le comportement des utilisateurs et des ordinateurs dans un environnement Active Directory. Ces objets de stratégie de groupe permettent de définir des paramètres de sécurité, des configurations de bureau, des règles de mot de passe, des scripts de démarrage/arrêt, l'installation de logiciels, et bien plus encore. L'avantage principal des GPOs est leur **gestion centralisée**.

#### Fonctionnement des GPO

Les GPOs sont appliquées à différents niveaux de l'architecture Active Directory : **forêt**, **domaine**, **unités d'organisation (OU)**, ou même à des groupes spécifiques d'ordinateurs ou d'utilisateurs.

##### 1. **Création et Application des GPO**
- **Création** : Les administrateurs peuvent créer des GPO dans la console de gestion des stratégies de groupe (Group Policy Management Console - GPMC). Ils peuvent y configurer divers paramètres tels que des paramètres de sécurité, des options de contrôle de l'environnement utilisateur, et des scripts de démarrage/arrêt.
  
- **Lien** : Une fois la GPO créée, elle doit être **liée** à un objet Active Directory, tel qu'un domaine, une unité d'organisation (OU), ou même un site. La GPO s'applique alors à tous les objets (utilisateurs ou ordinateurs) dans cette zone.

##### 2. **Types de GPO**
- **GPO de domaine** : Ces GPOs sont appliquées à l'ensemble du domaine et affectent tous les utilisateurs et ordinateurs du domaine.
  
- **GPO d'OU (Unité d'Organisation)** : Elles s'appliquent uniquement aux objets (utilisateurs ou ordinateurs) dans une unité d'organisation spécifique, permettant une gestion plus granulaire.

- **GPO locale** : Ces GPOs ne concernent qu'un seul ordinateur individuel et ne sont pas gérées à partir d'Active Directory. Elles sont utiles pour appliquer des paramètres sur des machines hors du réseau ou indépendantes.

##### 3. **Paramètres GPO**
Une GPO peut contenir deux types de paramètres principaux :
  
- **Paramètres utilisateur** : Ces paramètres s'appliquent aux comptes utilisateurs et définissent ce que les utilisateurs peuvent ou ne peuvent pas faire sur leur session. Par exemple, ces paramètres peuvent contrôler les éléments suivants :
  - Restrictions sur l'accès à certains paramètres du panneau de configuration.
  - Configuration des stratégies de sécurité des mots de passe.
  - Définition des options de bureau, comme l'arrière-plan ou les écrans de veille.

- **Paramètres ordinateur** : Ces paramètres s'appliquent aux ordinateurs et définissent leur comportement global, quels que soient les utilisateurs qui s'y connectent. Quelques exemples :
  - Activation ou désactivation des mises à jour automatiques.
  - Installation de logiciels à distance.
  - Configuration des scripts de démarrage et d'arrêt.

##### 4. **Priorité et Héritage des GPO**
L'une des forces des GPO est qu'elles respectent une **hiérarchie** et peuvent être **héritées** par des niveaux inférieurs de l'architecture Active Directory. Voici comment cela fonctionne :

- **Priorité** : Les GPOs peuvent être appliquées à différents niveaux de l'AD (forêt, domaine, OU). En cas de conflit entre plusieurs GPO, celle la plus proche de l'objet concerné dans la hiérarchie (par exemple, une OU) a généralement la priorité sur celle appliquée à un niveau supérieur (par exemple, le domaine).
  
- **Héritage** : Les GPO appliquées à un domaine ou à une OU parent peuvent être héritées par des sous-domaines ou sous-OU, sauf si l’héritage est bloqué manuellement.

##### 5. **Filtrage des GPOs**
Les administrateurs peuvent également filtrer l'application d'une GPO pour des groupes d'utilisateurs ou d'ordinateurs spécifiques au sein d'un domaine ou d'une OU. Cela se fait en appliquant des **permissions** sur la GPO et en configurant un **filtrage de sécurité** basé sur les **groupes de sécurité**.

##### 6. **Réplication des GPOs**
Dans un environnement multi-site, les GPOs sont répliquées entre les contrôleurs de domaine. Cela garantit que les règles de groupe sont cohérentes à travers toute l'infrastructure réseau, indépendamment de la localisation géographique des utilisateurs ou des ordinateurs.

#### Exemples d’utilisation des GPOs

Voici quelques exemples courants de l’utilisation des GPOs dans la gestion d’un réseau Windows :

- **Politiques de mot de passe** : Définir des règles strictes sur la longueur, la complexité, et la durée de vie des mots de passe.
  
- **Scripts de démarrage/arrêt** : Exécuter des scripts automatiquement lorsque les ordinateurs démarrent ou s'arrêtent (par exemple, des tâches de maintenance ou des installations de logiciels).
  
- **Déploiement de logiciels** : Installer à distance des applications sur plusieurs ordinateurs simultanément sans avoir à intervenir sur chaque machine individuellement.
  
- **Restriction des accès** : Empêcher les utilisateurs d'accéder à des éléments sensibles du système, comme le panneau de configuration ou le registre Windows.

#### Avantages des GPOs

- **Gestion centralisée** : Les GPOs permettent aux administrateurs de configurer les règles et les paramètres une seule fois, puis de les appliquer à des milliers de machines ou d'utilisateurs dans le réseau.
  
- **Flexibilité et granularité** : Grâce aux OUs et aux filtres de sécurité, les GPOs permettent de personnaliser précisément les configurations pour des groupes spécifiques d'utilisateurs ou de machines.

- **Sécurité renforcée** : En appliquant des politiques uniformes de sécurité, comme des règles de mot de passe ou des restrictions d’accès, les GPOs contribuent à améliorer la sécurité globale du réseau.

### Résumé

Les **GPO (Group Policy Objects)** sont un outil puissant pour gérer centralement les configurations et la sécurité des utilisateurs et des ordinateurs dans un environnement Active Directory. Elles permettent d'automatiser et d'uniformiser la gestion de politiques à grande échelle, tout en offrant une grande flexibilité grâce à la gestion hiérarchique et à l'héritage.

