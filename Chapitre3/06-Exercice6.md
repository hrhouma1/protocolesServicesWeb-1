# **Exercice 6 : Scénario IPv4 avec NetBIOS et découverte réseau activés**

#### **Objectifs :**
- Activer **NetBIOS** pour permettre la résolution des noms dans un réseau **IPv4**.
- Observer comment la combinaison de **NetBIOS** et de la **découverte réseau** fournit une fonctionnalité complète pour la gestion des machines dans un réseau local sans **DNS**.
- Désactiver **IPv6** pour se concentrer uniquement sur le fonctionnement d'IPv4.

---

# **1. Activer NetBIOS et désactiver IPv6 sur Serv1 et Serv2 :**

   **Explication** : **NetBIOS** est responsable de la résolution des noms dans les réseaux **IPv4** sans serveur DNS. En activant **NetBIOS**, vous permettez à vos machines de se "trouver" et de communiquer facilement via leurs noms d'hôte. La **découverte réseau** vous permet de voir les machines dans la fenêtre **Réseau** de Windows.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée** pour ouvrir la fenêtre **Connexions réseau**.
   - Faites un clic droit sur la **carte réseau active** et sélectionnez **Propriétés**.
   - **Désactiver IPv6** :
     - Dans la fenêtre des propriétés de la carte réseau, décochez l'option **Protocole Internet Version 6 (TCP/IPv6)** pour désactiver IPv6.
   
   - **Activer NetBIOS pour IPv4** :
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Cliquez sur **Avancé**, puis allez dans l'onglet **WINS**.
     - Sous la section **Paramètres NetBIOS**, sélectionnez **Par défaut**. Cela active **NetBIOS**, sauf si un serveur DHCP le désactive explicitement.
     - Cliquez sur **OK** pour valider les changements.
   - Fermez toutes les fenêtres pour appliquer les changements.

   **Sur Serv2 :**
   - Répétez les mêmes étapes pour **Serv2** afin de désactiver **IPv6** et d'activer **NetBIOS** pour **IPv4**.

---

# **2. Redémarrer les deux machines :**

   **Explication** : Après avoir modifié les paramètres réseau, un redémarrage des deux machines est nécessaire pour appliquer les changements et s'assurer que **NetBIOS** est activé correctement.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez la commande suivante :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette commande sur **Serv2** pour redémarrer les deux machines.

---

# **3. Tester la connectivité via ping avec NetBIOS activé :**

   **Explication** : Après le redémarrage, vous allez tester la connectivité entre **Serv1** et **Serv2** via **ping** en utilisant le **nom d’hôte**. Avec **NetBIOS** activé, **Serv2** pourra pinguer **Serv1** en résolvant automatiquement le nom **Serv1** en adresse IP (sans avoir besoin de DNS).

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez une réponse de l'adresse **IPv4** de **Serv1** (par exemple `192.168.0.1`), ce qui prouve que **NetBIOS** fonctionne et permet de résoudre le nom **Serv1** dans un réseau **IPv4** sans serveur **DNS**.

---

# **4. Vérifier la fenêtre Réseau :**

   **Explication** : Une fois que **NetBIOS** et la **découverte réseau** sont activés, vous pouvez voir les machines du réseau dans la fenêtre **Réseau** de l'Explorateur Windows. Cela montre que la **découverte réseau** permet de rendre visibles les machines et que **NetBIOS** permet de les pinguer et d’y accéder.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Dans le panneau de gauche, cliquez sur **Réseau**.
   - **Résultat attendu** : Vous verrez **Serv1** apparaître dans la fenêtre **Réseau**, prouvant que la **découverte réseau** fonctionne correctement.

---

# **5. Essayer d'accéder à Serv1 via la fenêtre Réseau :**

   **Explication** : Avec **NetBIOS** et la **découverte réseau** activés, vous pouvez non seulement voir **Serv1** dans la fenêtre **Réseau**, mais vous pouvez également y accéder. En double-cliquant sur l'icône de **Serv1**, vous tenterez d'ouvrir une connexion via un chemin **UNC** (par exemple `\\Serv1`).

   - Double-cliquez sur **Serv1** dans la fenêtre **Réseau**.
   - **Résultat attendu** : Une nouvelle fenêtre s’ouvre, affichant les dossiers et ressources partagées de **Serv1**, prouvant que la combinaison **NetBIOS** + **découverte réseau** permet une résolution complète des noms et l'accès aux ressources via un chemin **UNC** dans un réseau **IPv4** sans DNS.

---

# **6. Fermer toutes les fenêtres ouvertes :**

   - Une fois les tests terminés, fermez toutes les fenêtres ouvertes sur **Serv1** et **Serv2**.

---

# **Conclusion :**

1. **Ping par nom d’hôte** : Avec **NetBIOS** activé, vous pouvez pinguer une machine par son **nom d’hôte** dans un réseau **IPv4** sans serveur DNS. Cela montre que **NetBIOS** permet la résolution des noms locaux.
   
2. **Fenêtre Réseau** : La **découverte réseau** permet de voir les machines présentes sur le réseau local dans l'explorateur **Réseau**. C’est une fonctionnalité visuelle, mais elle ne remplace pas **NetBIOS** pour la résolution des noms.

3. **Accès via chemin UNC** : En double-cliquant sur l'icône de **Serv1** dans la fenêtre **Réseau**, vous pouvez accéder aux ressources partagées via un chemin **UNC** (comme `\\Serv1`). Cela prouve que la combinaison **NetBIOS** et **découverte réseau** offre une fonctionnalité complète de résolution des noms et de navigation réseau dans un environnement **IPv4**.

---

# **Conseils supplémentaires **

- **NetBIOS** est une fonctionnalité importante dans les réseaux **IPv4** sans DNS, car elle permet de résoudre les noms d'ordinateurs locaux (comme **Serv1** ou **Serv2**) en adresses IP. Cela facilite la communication entre les machines.
- **La découverte réseau** est une fonctionnalité qui permet de voir les machines connectées au réseau dans la fenêtre **Réseau** de Windows. Cependant, cette fonctionnalité ne permet pas à elle seule de résoudre les noms (comme le fait **NetBIOS**).
- En combinant **NetBIOS** et **découverte réseau**, vous pouvez à la fois voir les machines sur le réseau et communiquer avec elles en utilisant leurs noms d’hôtes (par exemple, pinguer ou accéder aux dossiers partagés via **UNC**).
