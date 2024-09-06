# **Exercice 7 : Scénario IPv6 uniquement, avec découverte réseau**

#### **Objectifs :**
- Désactiver **IPv4** et activer **IPv6**.
- Observer comment la **découverte réseau** fonctionne dans un réseau **IPv6** seul.
- Comprendre que dans un réseau **IPv4**, la résolution des noms nécessite **NetBIOS**, mais dans un réseau **IPv6**, la **découverte réseau** fournit cette fonctionnalité.

---

# **1. Activer IPv6 et désactiver IPv4 sur Serv1 et Serv2 :**

   **Explication** : L'objectif de cet exercice est de basculer vers un réseau **IPv6 uniquement**, sans **IPv4**. **NetBIOS**, qui est utilisé pour la résolution des noms dans **IPv4**, ne fonctionne pas avec **IPv6**. Nous allons utiliser la **découverte réseau** pour permettre la résolution des noms dans un réseau **IPv6**.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée** pour ouvrir la fenêtre **Connexions réseau**.
   - Faites un clic droit sur la **carte réseau active** et sélectionnez **Propriétés**.
   
   - **Activer IPv6** :
     - Cochez l'option **Protocole Internet Version 6 (TCP/IPv6)**.
   - **Désactiver IPv4** :
     - Décochez l'option **Protocole Internet Version 4 (TCP/IPv4)** pour désactiver IPv4.
   - Cliquez sur **OK** pour valider les changements.

   **Sur Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2** afin d'activer **IPv6** et de désactiver **IPv4**.

---

# **2. Redémarrer les deux machines :**

   **Explication** : Le redémarrage est nécessaire après toute modification des paramètres réseau pour s'assurer que les changements sont appliqués correctement.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez la commande suivante :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette commande sur **Serv2** pour redémarrer les deux machines.

---

# **3. Tester la connectivité via ping avec IPv6 :**

   **Explication** : Une fois que les machines ont redémarré et que **IPv6** est activé (et **IPv4** désactivé), vous allez tester si **Serv2** peut pinguer **Serv1** par son nom. Dans un réseau **IPv6**, la **découverte réseau** joue le rôle de **NetBIOS** dans **IPv4** et permet la résolution des noms.

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous devriez recevoir une réponse de l’adresse **IPv6** de **Serv1** (du type `fd00::1` par exemple). Cela prouve que la **découverte réseau** permet la résolution des noms en **IPv6**, ce que **NetBIOS** faisait pour **IPv4**.

---

# **4. Vérifier la fenêtre Réseau :**

   **Explication** : Dans les réseaux **IPv6**, la **découverte réseau** permet aux ordinateurs de se voir et de se connecter les uns aux autres sans utiliser de serveur **DNS** ou de service comme **NetBIOS**. Nous allons maintenant vérifier si **Serv1** apparaît dans la fenêtre **Réseau** de **Serv2**.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Dans le panneau de gauche, cliquez sur **Réseau**.
   - **Résultat attendu** : **Serv1** devrait apparaître dans la fenêtre **Réseau**, prouvant que la **découverte réseau** fonctionne correctement en **IPv6**, comme elle le ferait en **IPv4**.

---

# **5. Essayer d'accéder à Serv1 via la fenêtre Réseau :**

   **Explication** : Une fois que **Serv1** apparaît dans la fenêtre **Réseau**, vous pouvez y accéder en double-cliquant sur son icône. Cela prouve que la **découverte réseau** permet non seulement de voir les machines dans le réseau, mais aussi d'accéder à leurs ressources partagées dans un environnement **IPv6**.

   - Double-cliquez sur l'icône de **Serv1** dans la fenêtre **Réseau**.
   - **Résultat attendu** : Une nouvelle fenêtre s'ouvre, affichant les dossiers et ressources partagées de **Serv1** (par exemple, le partage **Imprimantes**). Cela montre que la **découverte réseau** permet de naviguer jusqu'aux ressources partagées dans un réseau **IPv6**.

---

# **6. Tester l'accès via un chemin UNC en IPv6 :**

   **Explication** : Le chemin **UNC** (Universal Naming Convention) permet d'accéder aux ressources partagées sur un réseau en utilisant le nom d’hôte, par exemple `\\Serv1`. Dans un réseau **IPv4**, **NetBIOS** est requis pour cette fonctionnalité. En **IPv6**, c'est la **découverte réseau** qui permet de résoudre les noms d'hôte.

   - Ouvrez la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**.
   - Tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : La connexion au dossier partagé de **Serv1** devrait s'établir, prouvant que la **découverte réseau** offre des services similaires à ceux de **NetBIOS** dans un réseau **IPv6**. Cela montre que dans un réseau **IPv6**, la **découverte réseau** remplace efficacement **NetBIOS** pour la résolution des noms et l'accès via chemin **UNC**.

---

# **7. Réessayer via la fenêtre Réseau :**

   **Explication** : Vous pouvez à nouveau vérifier si **Serv1** apparaît dans la fenêtre **Réseau** et si vous pouvez y accéder sans problème. Cela montre que la **découverte réseau** en **IPv6** fonctionne de la même manière que dans un réseau **IPv4**, où **NetBIOS** permettait une résolution des noms et un accès aux ressources partagées.

   - Ouvrez la fenêtre **Réseau** dans l'Explorateur de fichiers sur **Serv2**.
   - Double-cliquez sur **Serv1** dans la fenêtre **Réseau**.
   - **Résultat attendu** : La fenêtre de **Serv1** s'ouvre et affiche le partage **Imprimantes** ou d'autres dossiers partagés de **Serv1**. Cela prouve que la **découverte réseau** apporte la même fonctionnalité en **IPv6** que **NetBIOS** en **IPv4**.

---

# **8. Fermer toutes les fenêtres ouvertes :**

   - Une fois que vous avez terminé toutes les vérifications, fermez toutes les fenêtres ouvertes sur **Serv1** et **Serv2**.

---

# Conclusion 

1. **Ping par nom d’hôte** : Dans un réseau **IPv6** uniquement, la **découverte réseau** permet la résolution des noms et remplace **NetBIOS**, qui n’est utilisé que dans **IPv4**. Vous pouvez pinguer une machine par son nom d’hôte sans avoir besoin d'un serveur **DNS**.
   
2. **Fenêtre Réseau** : Comme en **IPv4**, la **découverte réseau** permet de voir les machines présentes sur le réseau local dans la fenêtre **Réseau** de Windows. La principale différence est que pour **IPv6**, cette découverte fonctionne sans **NetBIOS**.

3. **Accès via chemin UNC** : Dans un réseau **IPv6**, vous pouvez accéder aux ressources partagées en utilisant un chemin **UNC** (comme `\\Serv1`), grâce à la **découverte réseau**. Cela prouve que la **découverte réseau** en **IPv6** offre des services de résolution de noms et d'accès aux ressources partagées similaires à ceux de **NetBIOS** en **IPv4**.

---

# Conseils

- **IPv6** est le futur de l'Internet, car il offre un plus grand espace d’adressage que **IPv4**. Dans ce type de réseau, la **découverte réseau** est cruciale pour la résolution des noms d’hôte et l'accès aux machines du réseau.
- **NetBIOS** est utilisé uniquement dans les réseaux **IPv4**. Il est remplacé par la **découverte réseau** dans les réseaux **IPv6**, qui joue un rôle similaire pour résoudre les noms et permettre l'accès aux ressources partagées.
- Comprendre les différences entre **IPv4** et **IPv6** ainsi que leurs mécanismes de résolution des noms est essentiel pour travailler dans des environnements réseau modernes.

