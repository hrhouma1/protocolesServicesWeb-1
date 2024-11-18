### **Objectifs du TP : Configuration des services réseau et sécurisation sur Windows Server 2019**

Ce TP vise à enseigner les **concepts fondamentaux de l'administration réseau** en configurant une infrastructure Active Directory sécurisée, redondante et fonctionnelle, tout en introduisant des pratiques de gestion et de sécurisation avancées. Voici une analyse des objectifs et du **"pourquoi"** de chaque composant :

---

### **Objectif Principal**
Créer un réseau professionnel fiable et sécurisé, basé sur un **domaine Active Directory** géré par plusieurs contrôleurs de domaine, pour permettre :
1. **Une gestion centralisée des utilisateurs et des ressources** (par Active Directory).
2. **La sécurité des données et des authentifications**.
3. **La redondance et la résilience** face aux pannes.

---

### **Pourquoi un Contrôleur de Domaine (DC) ?**

#### **Rôle d’un Contrôleur de Domaine :**
Un DC (Domain Controller) est un serveur qui gère l'authentification des utilisateurs et le contrôle d'accès dans un réseau Active Directory.

#### **Objectifs du DC dans le TP :**
1. **Centralisation :**
   - Permet de gérer **les utilisateurs**, **groupes**, **politiques de sécurité**, et **ressources réseau** à partir d’un seul point.
   - Exemple : Les utilisateurs se connectent au domaine avec des identifiants centralisés, et non des comptes locaux.

2. **Sécurisation :**
   - Toutes les connexions passent par le DC, qui valide si un utilisateur ou une machine a les droits nécessaires.

3. **Standardisation :**
   - Les configurations réseau, les droits, et les politiques de sécurité (exemple : stratégies de mot de passe) sont appliqués uniformément sur tous les appareils du domaine.

---

### **Pourquoi deux contrôleurs de domaine (DC1 et DC2) ?**

#### **Objectif de la redondance :**
1. **Fiabilité accrue :**
   - Si le DC principal (DC1) tombe en panne, le DC secondaire (DC2) prend automatiquement le relais.
   - Les utilisateurs peuvent toujours s'authentifier et accéder aux ressources.

2. **Répartition des charges :**
   - Dans les grandes entreprises, plusieurs DCs permettent de répartir la charge d’authentification et de gestion des ressources.

3. **Synchronisation des données :**
   - Les deux DCs répliquent les informations (utilisateurs, politiques, DNS, etc.) en temps réel, garantissant une continuité de service.

#### **Dans ce TP :**
- DC1 agit comme **contrôleur principal**.
- DC2 est configuré comme **contrôleur secondaire**, assurant la redondance en cas de panne de DC1.

---

### **Pourquoi un RODC (Read-Only Domain Controller) ?**

#### **Qu’est-ce qu’un RODC ?**
Un RODC est un **Contrôleur de Domaine en Lecture Seule** :
- Il contient une **copie non modifiable** des informations Active Directory.
- Les changements effectués sur les utilisateurs, les groupes ou les stratégies ne peuvent être réalisés qu’à partir d’un DC principal ou secondaire.

#### **Objectifs du RODC dans le TP :**
1. **Sécurité :**
   - Le RODC est conçu pour être déployé dans des environnements où le risque de compromission est élevé (par exemple, sites distants, succursales).
   - Les informations sensibles (mots de passe des administrateurs, politiques critiques) ne sont pas répliquées sur un RODC.

2. **Résilience locale :**
   - Les utilisateurs locaux d’un site distant peuvent toujours s’authentifier auprès du RODC, même si les DC principaux sont inaccessibles (panne réseau, défaillance).

3. **Réduction des risques :**
   - En cas de vol ou de piratage du RODC, le système principal reste protégé grâce aux restrictions de lecture seule.

#### **Dans ce TP :**
Le RODC est configuré pour :
- Illustrer comment sécuriser les environnements distants tout en maintenant une gestion centralisée.
- Tester l’authentification locale lorsque les DCs principaux sont hors ligne.

---

### **Pourquoi intégrer pfSense ?**

#### **Rôle de pfSense :**
pfSense est un routeur et pare-feu open-source utilisé pour gérer et sécuriser les connexions réseau.

#### **Objectifs dans ce TP :**
1. **Gestion des VLANs :**
   - Segmentation des serveurs et des clients pour améliorer la sécurité et optimiser le flux de données.
2. **DHCP et routage :**
   - Fournir des adresses IP aux machines connectées.
3. **Sécurité réseau :**
   - Protéger l’infrastructure contre les menaces externes.

---

### **Pourquoi un réseau avec des VLANs ?**

#### **Objectifs :**
1. **Isolation des segments du réseau :**
   - Les serveurs (VLAN 10) et les clients (VLAN 20) sont isolés pour limiter les risques de sécurité.
2. **Efficacité :**
   - Réduction du trafic inutile entre les différents segments.

---

### **Pourquoi ce TP est-il essentiel ?**

Ce TP est essentiel pour comprendre les concepts suivants :

1. **Infrastructure Active Directory :**
   - Apprendre à déployer et gérer une structure AD, utilisée dans presque toutes les organisations modernes.

2. **Redondance et résilience :**
   - Découvrir l’importance de la continuité de service dans les environnements critiques.
   - Tester des mécanismes de basculement (failover) pour DNS, DHCP, et authentification.

3. **Sécurisation réseau :**
   - Comprendre comment protéger une infrastructure en utilisant VLANs, RODC, et pfSense.

4. **Compétences pratiques :**
   - Développer des compétences concrètes en configuration réseau, gestion des serveurs Windows, et sécurisation.

---

### **En conclusion :**
- Ce TP simule une **infrastructure d’entreprise complète**.
- Vous apprenez **comment concevoir un réseau robuste** avec des mécanismes de sécurité et de résilience.
- Chaque composant (DC1, DC2, RODC, pfSense) a un rôle critique, vous préparant à des scénarios réels dans le monde professionnel.
