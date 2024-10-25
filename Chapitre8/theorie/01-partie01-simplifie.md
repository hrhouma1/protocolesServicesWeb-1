### **Plan de matière : Services d'annuaire et d'authentification**

1. [Introduction aux services d'annuaire et d'authentification](#intro)
2. [Active Directory](#active-directory)
   - [Définition et rôle](#definition-role)
   - [Structure d'un domaine AD](#structure-ad)
   - [Installation de l'AD](#installation-ad)
   - [Configuration de base](#configuration-base)
3. [Modèle de contrôle d'accès et privilèges](#controle-acces)
   - [Concept de privilèges et permissions](#privilèges-permissions)
   - [Modèle de contrôle d'accès basé sur les rôles (RBAC)](#rbac)
   - [Configuration des permissions dans l'AD](#configuration-permissions)
4. [Protocoles d'authentification](#protocoles-auth)
   - [Kerberosv5](#kerberos)
     - [Fonctionnement de Kerberos](#fonctionnement-kerberos)
     - [Avantages et limitations](#avantages-limitations)
   - [NTLM](#ntlm)
     - [Fonctionnement de NTLM](#fonctionnement-ntlm)
     - [Comparaison NTLM et Kerberos](#comparaison)
5. [Étapes de configuration avancée](#config-avancee)
   - [Configuration des politiques de groupe (GPO)](#gpo)
   - [Mise en place d'une authentification multifacteur (MFA)](#mfa)
6. [Conclusion](#conclusion)

---

### **Contenu détaillé**

#### <a name="intro"></a>Introduction aux services d'annuaire et d'authentification
Les services d'annuaire sont des bases de données spécialisées qui permettent de stocker des informations sur les ressources réseau et les utilisateurs. Leur fonction principale est de centraliser et de sécuriser les accès.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="active-directory"></a>Active Directory

##### <a name="definition-role"></a>Définition et rôle
Active Directory (AD) est un service d'annuaire développé par Microsoft, utilisé dans les réseaux pour gérer les utilisateurs, les ordinateurs, les groupes et autres objets.

<a href="#top">[Retour en haut](#)</a>

##### <a name="structure-ad"></a>Structure d'un domaine AD
Un domaine AD est organisé en différents conteneurs comme les unités d'organisation (OU), permettant la structuration logique des objets.

<a href="#top">[Retour en haut](#)</a>

##### <a name="installation-ad"></a>Installation de l'AD
Voici les étapes pour installer AD sur un serveur Windows Server.

<a href="#top">[Retour en haut](#)</a>

##### <a name="configuration-base"></a>Configuration de base
Après l'installation, une configuration de base est nécessaire pour définir les groupes et les politiques de sécurité.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="controle-acces"></a>Modèle de contrôle d'accès et privilèges

##### <a name="privilèges-permissions"></a>Concept de privilèges et permissions
Les privilèges et permissions sont les droits d'accès définis pour chaque utilisateur en fonction de leur rôle dans l'entreprise.

<a href="#top">[Retour en haut](#)</a>

##### <a name="rbac"></a>Modèle de contrôle d'accès basé sur les rôles (RBAC)
Le RBAC est un modèle de gestion des autorisations qui attribue des droits d'accès en fonction des rôles dans l'organisation.

<a href="#top">[Retour en haut](#)</a>

##### <a name="configuration-permissions"></a>Configuration des permissions dans l'AD
Les permissions peuvent être configurées via les groupes AD et les politiques de sécurité appliquées aux objets.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="protocoles-auth"></a>Protocoles d'authentification

##### <a name="kerberos"></a>Kerberosv5

###### <a name="fonctionnement-kerberos"></a>Fonctionnement de Kerberos
Kerberos utilise des tickets pour permettre aux utilisateurs d'accéder aux services en toute sécurité sans envoyer de mot de passe.

<a href="#top">[Retour en haut](#)</a>

###### <a name="avantages-limitations"></a>Avantages et limitations
Kerberos offre une sécurité accrue mais nécessite une synchronisation horaire stricte entre les systèmes.

<a href="#top">[Retour en haut](#)</a>

##### <a name="ntlm"></a>NTLM

###### <a name="fonctionnement-ntlm"></a>Fonctionnement de NTLM
NTLM utilise des hash pour valider l'identité de l'utilisateur et est compatible avec les anciennes versions de Windows.

<a href="#top">[Retour en haut](#)</a>

###### <a name="comparaison"></a>Comparaison NTLM et Kerberos
NTLM est moins sécurisé que Kerberos mais reste utile pour les environnements plus anciens.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="config-avancee"></a>Étapes de configuration avancée

##### <a name="gpo"></a>Configuration des politiques de groupe (GPO)
Les GPO permettent de définir des règles de sécurité à l'échelle de l'organisation pour contrôler l'accès et la configuration des postes clients.

<a href="#top">[Retour en haut](#)</a>

##### <a name="mfa"></a>Mise en place d'une authentification multifacteur (MFA)
L'authentification multifacteur ajoute une couche de sécurité en demandant des informations supplémentaires pour valider l'identité de l'utilisateur.

<a href="#top">[Retour en haut](#)</a>

---

#### <a name="conclusion"></a>Conclusion
Les services d'annuaire et d'authentification, tels qu’Active Directory, sont essentiels pour sécuriser et gérer les ressources réseau. La configuration correcte de ces services est cruciale pour protéger les informations de l'entreprise.

<a href="#top">[Retour en haut](#)</a>

