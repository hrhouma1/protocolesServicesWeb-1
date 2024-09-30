## Qu'est-ce que l'Active Directory ?

L'Active Directory est un annuaire créé par Microsoft pour les systèmes Windows. Il permet de centraliser deux fonctions essentielles dans un réseau informatique :

1. L'identification des utilisateurs et des ordinateurs
2. L'authentification (vérification de l'identité)

Son but principal est de simplifier l'administration d'un réseau en regroupant toutes les informations au même endroit

## Structure de base

### Objets et conteneurs

- L'Active Directory contient différents types d'objets comme les utilisateurs et les ordinateurs
- Ces objets peuvent être regroupés dans des conteneurs appelés "groupes" ou "unités d'organisation"
- Les unités d'organisation fonctionnent comme des dossiers pour organiser les objets

### Domaine

Un domaine est la structure de base de l'Active Directory. Il regroupe tous les objets (utilisateurs, ordinateurs, etc.) d'un réseau

## Concepts avancés

### Arbre

Un arbre est formé lorsqu'un domaine principal a plusieurs sous-domaines. Par exemple :
- it-connect.local (domaine principal)
  - paris.it-connect.local (sous-domaine)
  - londres.it-connect.local (sous-domaine)

### Forêt

Une forêt est un ensemble d'arbres. Elle permet de regrouper plusieurs structures indépendantes tout en facilitant la communication entre elles[1].

## Avantages de l'Active Directory

1. Administration centralisée : tout est géré depuis un seul endroit
2. Authentification unifiée : un seul compte par utilisateur pour accéder à toutes les ressources
3. Meilleure organisation : possibilité de structurer les objets de manière logique
4. Sécurité renforcée : gestion centralisée des droits d'accès

## Contrôleurs de domaine

Ce sont des serveurs spéciaux qui :
- Stockent une copie de l'annuaire Active Directory
- Gèrent les identifications et authentifications
- Assurent la disponibilité du service (il est recommandé d'en avoir au moins deux)

En comprenant ces notions de base, vous aurez une bonne vue d'ensemble de ce qu'est l'Active Directory et de son fonctionnement dans un réseau d'entreprise.

