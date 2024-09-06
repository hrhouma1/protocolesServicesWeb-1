# **Exercice 3 : Scénario IPv4 seul, avec NetBIOS, sans découverte réseau**

#### **Objectifs :**
- Activer **NetBIOS** pour permettre la résolution des noms d'hôtes dans un réseau IPv4 uniquement.
- Désactiver **IPv6** et tester la connectivité.
- Examiner la résolution de noms avec **NetBIOS** et vérifier si elle fonctionne via le ping, le partage de fichiers et la fenêtre réseau.

---

### **Étapes de configuration et de test**

# **1. Ouvrez une session en tant qu’administrateur sur Serv1 et Serv2 :**

   - Connectez-vous à **Serv1** et **Serv2** en utilisant vos comptes administrateurs.

---

# **2. Désactiver IPv6 et activer NetBIOS sur les deux machines :**

   **Explication** : Dans cet exercice, nous désactivons IPv6 et activons NetBIOS pour voir comment il facilite la résolution de noms dans un réseau IPv4 uniquement.

   **Sur Serv1 :**
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée** pour ouvrir la fenêtre **Connexions réseau**.
   - Faites un clic droit sur la **carte réseau active** (celle utilisée pour la communication réseau) et sélectionnez **Propriétés**.

   - **Désactiver IPv6 :**
     - Dans la fenêtre des **Propriétés**, décochez l'option **Protocole Internet Version 6 (TCP/IPv6)** pour désactiver IPv6.
   
   - **Activer NetBIOS pour IPv4 :**
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Cliquez sur **Avancé**, puis allez dans l'onglet **WINS**.
     - Dans la section **Paramètres NetBIOS**, sélectionnez **Par défaut** (cette option active NetBIOS sauf si un serveur DHCP le désactive).
     - Cliquez sur **OK** pour valider.

   - Cliquez sur **OK** dans toutes les fenêtres ouvertes pour appliquer les changements.

   **Sur Serv2 :**
   - Répétez les mêmes étapes sur **Serv2**.

---

# **3. Redémarrez les deux machines :**

   **Explication** : Après avoir modifié les paramètres réseau, il est nécessaire de redémarrer les machines pour appliquer les changements.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez la commande sur **Serv2** pour redémarrer les deux machines.

---

# **4. Tester la connectivité via ping et NetBIOS :**

   **Explication** : Une fois les machines redémarrées, vous allez vérifier si **NetBIOS** permet à **Serv2** de pinguer **Serv1** par son **nom d’hôte**, prouvant que la résolution des noms fonctionne correctement.

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** en utilisant son **nom d'hôte** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez une réponse de l’adresse IP **192.168.0.1**, ce qui prouve que **NetBIOS** résout le nom d’hôte **Serv1** dans un réseau local IPv4 sans DNS.

---

# **5. Tester le partage de fichiers via le nom d’hôte (NetBIOS) :**

   **Explication** : Nous allons tester l'accès aux fichiers partagés de **Serv1** en utilisant son **nom d’hôte** avec un chemin **UNC** (Universal Naming Convention). **NetBIOS** devrait permettre de résoudre le nom de la machine et d'accéder aux ressources partagées.

   - Ouvrez la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**.
   - Tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Une fenêtre s'ouvre affichant le partage **Imprimantes** de **Serv1**. Cela montre que **NetBIOS** résout les noms d’ordinateurs locaux lorsqu’ils sont utilisés dans un chemin **UNC**.

---

# **6. Vérifier la fenêtre Réseau :**

   **Explication** : La fenêtre **Réseau** dans **Windows Explorer** permet normalement de voir les autres machines sur le réseau. Cependant, **NetBIOS** ne permet pas d’afficher les machines dans cette fenêtre si la découverte réseau est désactivée.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Cliquez sur **Réseau** dans le panneau de gauche.
   - **Résultat attendu** : La fenêtre **Réseau** n'affiche aucune machine, car la **découverte réseau** est toujours désactivée, et **NetBIOS** ne gère pas cette fonctionnalité pour afficher les ordinateurs dans cette vue.

---

# **7. Fermer toutes les fenêtres :**

   - Une fois que vous avez terminé les tests, fermez toutes les fenêtres ouvertes sur **Serv2** et **Serv1**.

---

# **Conclusion pédagogique :**

1. **Ping via le nom d’hôte** fonctionne grâce à **NetBIOS**, même sans serveur DNS, ce qui prouve que NetBIOS est un mécanisme de résolution de noms pour les réseaux locaux IPv4.
2. **Accès aux fichiers via chemin UNC** fonctionne également via le nom d’hôte grâce à **NetBIOS**, prouvant que cette technologie résout bien les noms d'ordinateurs locaux dans les chemins UNC.
3. La **fenêtre Réseau** n’affiche toujours pas les machines du réseau, car la **découverte réseau** est désactivée, et **NetBIOS** ne gère pas cette fonction.

---

### **Conseils supplémentaires :**

- **NetBIOS** est utile dans les petits réseaux locaux où il n'y a pas de serveur DNS, mais il ne fonctionne que pour les noms d'hôtes locaux.
- **IPv6** est souvent désactivé pour les réseaux plus simples si l'on n'a pas besoin de ce type d'adressage.
- **La découverte réseau** doit être activée si vous voulez que les machines s'affichent automatiquement dans la fenêtre **Réseau** de l'Explorateur Windows.
