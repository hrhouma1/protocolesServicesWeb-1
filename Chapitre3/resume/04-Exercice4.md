# **Exercice 4 : Test de résolution automatique de noms sur un groupe de travail IPv4/IPv6, avec NetBIOS activé et Découverte réseau désactivée**

#### **Objectifs :**
- Activer **IPv6** sur les deux machines tout en conservant **IPv4**.
- Maintenir **NetBIOS activé** et **la découverte réseau désactivée**.
- Examiner le comportement du réseau dans ce contexte et tester la connectivité entre les deux machines.

---

# **1. Activer IPv6 sur les deux machines (Serv1 et Serv2)**

   **Explication** : Nous allons activer **IPv6** sur les deux machines, ce qui permet aux machines de communiquer en utilisant à la fois les adresses **IPv4** et **IPv6**.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée** pour ouvrir la fenêtre **Connexions réseau**.
   - Faites un clic droit sur la **carte réseau active** et sélectionnez **Propriétés**.
   - **Activer IPv6** :
     - Cochez l'option **Protocole Internet Version 6 (TCP/IPv6)** pour activer IPv6.
     - Cliquez sur **OK** pour appliquer les modifications.

   **Sur Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2** afin d'activer **IPv6**.

---

# **2. Redémarrer les deux machines**

   **Explication** : Comme toujours après avoir modifié des paramètres réseau, il est nécessaire de redémarrer les machines pour appliquer les changements.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez la commande suivante :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez la commande sur **Serv2** pour redémarrer les deux machines.

---

# **3. Tester la connectivité avec ping et observer l’utilisation d’IPv6**

   **Explication** : Une fois les deux machines redémarrées, vous allez vérifier si **Serv2** peut pinguer **Serv1** via son **nom d’hôte** et voir si la réponse provient de l’adresse **IPv6** de **Serv1**.

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** en utilisant son **nom d’hôte** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous devriez recevoir une réponse provenant de l’adresse **IPv6** de **Serv1** (du type `fd00::1`). Cela prouve que bien que **IPv4** soit activé, dans cette configuration, **NetBIOS** et **la découverte réseau désactivée**, le système utilise l’adresse **IPv6** pour répondre.

---

# **4. Vérifier la fenêtre Réseau (Découverte réseau désactivée)**

   **Explication** : Maintenant, nous allons vérifier si les machines apparaissent dans la fenêtre **Réseau** de l’explorateur Windows. Puisque la **découverte réseau** est désactivée, vous ne verrez pas d'autres machines, même si **NetBIOS** est activé.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Dans le panneau de gauche, cliquez sur **Réseau**.
   - **Résultat attendu** : La fenêtre **Réseau** sera vide, car la **découverte réseau** est désactivée. Même si **NetBIOS** est activé et que vous pouvez pinguer **Serv1**, cela n’affecte pas l’affichage dans cette fenêtre.

---

# **5. Pourquoi il n’est pas nécessaire de tester la connectivité par chemin UNC ?**

   **Explication** : Le test via **UNC** (Universal Naming Convention) permet d'accéder aux fichiers partagés sur un autre ordinateur via son **nom d’hôte** (par exemple `\\Serv1`). Cependant, comme nous avons déjà vu dans les exercices précédents que cela fonctionne avec **NetBIOS** activé, ajouter un protocole supplémentaire comme **IPv6** n'affectera pas ce fonctionnement. Le fait d’ajouter **IPv6** ne retire jamais une fonctionnalité existante.

   - **Vous pouvez donc sauter cette étape**, sachant que le partage de fichiers via chemin UNC fonctionnera toujours avec **NetBIOS** activé.

---

# **6. Fermez toutes les fenêtres ouvertes**

   - Une fois que vous avez terminé les tests, fermez toutes les fenêtres ouvertes sur **Serv2** et **Serv1**.

---

# **Conclusion pédagogique :**

1. **Ping par le nom d’hôte** avec **NetBIOS** activé fonctionne, et dans ce cas particulier, la réponse provient de l’adresse **IPv6** de **Serv1**, car **IPv6** est activé.
2. **La fenêtre Réseau** reste vide, même avec **NetBIOS** activé, car la **découverte réseau** est désactivée. **NetBIOS** n'est pas responsable de l'affichage des machines dans cette fenêtre.
3. **Le partage de fichiers via chemin UNC** fonctionne toujours avec **NetBIOS**, et l’ajout d’**IPv6** n’a aucun effet négatif sur ce fonctionnement.

---

# **Conseils :**

- **NetBIOS** facilite la résolution des noms sur un réseau local en IPv4 ou IPv6. Si vous activez **IPv6** sur votre réseau, les systèmes peuvent utiliser les adresses IPv6 sans pour autant perdre la fonctionnalité d’IPv4.
- **La découverte réseau** doit être activée si vous voulez voir les autres machines dans l'explorateur **Réseau**. Sans cela, même si vous pouvez pinguer les machines ou accéder à leurs fichiers, elles n'apparaîtront pas dans la fenêtre **Réseau**.
- **IPv6** ne supprime pas les fonctionnalités existantes d’un réseau **IPv4** ; il ajoute simplement une autre manière de communiquer entre les machines.

