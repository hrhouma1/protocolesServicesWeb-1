# **Exercice 5 : Scénario IPv4 uniquement, sans NetBIOS, avec découverte réseau**

#### **Objectifs :**
- Désactiver **IPv6** et **NetBIOS**.
- Activer la **découverte réseau**.
- Comprendre le comportement d'un réseau IPv4 uniquement avec **découverte réseau activée**, mais sans **NetBIOS**.

---

# **1. Activer la découverte réseau sur Serv1 et Serv2 :**

   **Explication** : La découverte réseau est une fonctionnalité qui permet à une machine de "voir" les autres machines présentes sur le réseau. Quand cette option est activée, les ordinateurs apparaissent dans la fenêtre "Réseau" de l'explorateur de fichiers, mais cela n'influence pas la **résolution des noms d’hôte**, qui est la capacité de pinguer une machine par son nom.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Ouvrez le **Panneau de configuration** : appuyez sur `Win + R`, tapez **control**, puis appuyez sur **Entrée**.
   - Allez dans **Réseau et Internet** > **Centre Réseau et Partage**.
   - Dans le panneau de gauche, cliquez sur **Modifier les paramètres de partage avancés**.
   - Sous **Privé**, cochez **Activer la découverte du réseau**.
   - Cliquez sur **Enregistrer les modifications**.

   **Sur Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2** pour activer la découverte réseau.

---

# **2. Redémarrer les deux machines :**

   **Explication** : Comme toujours après avoir modifié des paramètres réseau, il est nécessaire de redémarrer les machines pour que les changements prennent effet.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez la commande suivante :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette commande sur **Serv2** pour redémarrer les deux machines.

---

# **3. Désactiver IPv6 et NetBIOS sur Serv1 et Serv2 :**

   **Explication** : Dans cet exercice, nous allons désactiver **IPv6** et **NetBIOS** pour forcer les machines à utiliser uniquement **IPv4**. **NetBIOS** est un service qui permet de résoudre les noms d’hôtes (comme Serv1 ou Serv2) en adresses IP dans les réseaux locaux, ce qui est crucial pour le ping et l'accès aux fichiers via le chemin UNC. Ici, nous désactivons **NetBIOS** pour voir ce qui se passe sans cette fonctionnalité dans un réseau **IPv4** uniquement.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur.
   - Ouvrez la fenêtre des connexions réseau en appuyant sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée**.
   - Faites un clic droit sur la carte réseau active, puis cliquez sur **Propriétés**.
   - **Désactiver IPv6** :
     - Décochez l'option **Protocole Internet Version 6 (TCP/IPv6)**.
   - **Désactiver NetBIOS** :
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Cliquez sur **Avancé**, puis allez à l'onglet **WINS**.
     - Sélectionnez **Désactiver NetBIOS sur TCP/IP**.
     - Cliquez sur **OK** pour valider les changements.

   **Sur Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2** pour désactiver **IPv6** et **NetBIOS**.

---

# **4. Redémarrer de nouveau les machines :**

   - Après avoir désactivé **IPv6** et **NetBIOS**, redémarrez les deux machines pour que les changements soient pris en compte.

---

# **5. Tester la connectivité via ping avec NetBIOS désactivé :**

   **Explication** : Maintenant que **NetBIOS** est désactivé, vous allez tester si **Serv2** peut pinguer **Serv1** via son **nom d’hôte**. **NetBIOS** est normalement responsable de la résolution des noms dans un réseau **IPv4** sans DNS. Sans **NetBIOS**, la résolution des noms échouera, même avec la découverte réseau activée, car cette dernière ne fournit pas cette fonctionnalité.

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez un message indiquant que l'hôte est introuvable. Cela se produit parce que sans **NetBIOS**, il n'y a aucun mécanisme pour résoudre le nom d’hôte **Serv1** en adresse IP. La **découverte réseau** ne fournit pas cette capacité de résolution des noms.

---

# **6. Test du partage de fichiers via chemin UNC :**

   **Explication** : Dans un réseau **IPv4** avec **NetBIOS désactivé**, il est impossible d'accéder à un ordinateur en spécifiant son **nom d’hôte** dans un chemin **UNC** (comme `\\Serv1`). La **découverte réseau** ne peut pas remplacer **NetBIOS** pour cette fonctionnalité.

   - Ouvrez la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**.
   - Tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Vous recevrez un message d'erreur disant que **Windows ne peut pas accéder à \\Serv1**, car sans **NetBIOS**, le nom **Serv1** ne peut pas être résolu en une adresse IP.

---

# **7. Vérifier la fenêtre Réseau :**

   **Explication** : La **découverte réseau** permet d'afficher les machines disponibles sur le réseau dans la fenêtre **Réseau** de l'explorateur de fichiers. Bien que **NetBIOS** soit désactivé, la **découverte réseau** fonctionne toujours et permet d'afficher les machines dans cette fenêtre.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Cliquez sur **Réseau** dans le panneau de gauche.
   - **Résultat attendu** : **Serv1** et **Serv2** devraient apparaître dans cette fenêtre après avoir actualisé la page. La **découverte réseau** permet aux ordinateurs de s'afficher, même si **NetBIOS** est désactivé. Toutefois, cela ne signifie pas que vous pouvez accéder à **Serv1** par son nom.

---

# **8. Essayer d'accéder à Serv1 via la fenêtre Réseau :**

   **Explication** : Même si **Serv1** apparaît dans la fenêtre **Réseau**, tenter d'y accéder via un double-clic entraînera une erreur. Cela équivaut à essayer de se connecter via un chemin **UNC** en utilisant le nom de la machine. Or, comme **NetBIOS** est désactivé, cette tentative échouera.

   - Double-cliquez sur **Serv1** dans la fenêtre **Réseau**.
   - **Résultat attendu** : Vous recevrez un message d'erreur disant que **Windows ne peut pas accéder à \\Serv1**, car sans **NetBIOS**, le nom **Serv1** ne peut toujours pas être résolu en une adresse IP.

---

# **9. Fermer toutes les fenêtres ouvertes**

   - Après avoir terminé toutes les vérifications et tests, fermez toutes les fenêtres ouvertes sur **Serv1** et **Serv2**.

---

# **Conclusion pédagogique :**

1. **La découverte réseau** permet de voir les machines dans la fenêtre **Réseau**, mais elle ne remplace pas **NetBIOS** pour la résolution des noms. Même si vous pouvez voir **Serv1** dans la fenêtre **Réseau**, vous ne pourrez pas pinguer cette machine ou vous y connecter par son nom.
   
2. **NetBIOS** est essentiel dans un réseau **IPv4** sans DNS pour permettre la résolution des noms d’hôtes. Sans **NetBIOS**, vous ne pouvez pas pinguer une machine par son nom, ni accéder aux ressources partagées via un chemin **UNC**.

3. **IPv6** a été désactivé pour cet exercice, mais cela n'a pas d'impact direct ici car nous avons testé un réseau uniquement **IPv4**. Cependant, cela vous montre l'importance d'avoir un mécanisme de résolution de noms comme **NetBIOS** ou **DNS** pour les réseaux **IPv4**.

---

### **Conseils supplémentaires **

- **La découverte réseau** est pratique pour voir les machines dans un environnement local, mais sans **NetBIOS

**, cette fonctionnalité reste limitée.
- **NetBIOS** est essentiel pour la résolution des noms dans un réseau **IPv4**. Il permet de traduire les noms d'ordinateurs (comme **Serv1**) en adresses IP, ce qui est crucial pour des opérations comme le ping et le partage de fichiers.
- Sans un mécanisme de résolution des noms (comme **NetBIOS** ou **DNS**), même si vous voyez les machines dans la fenêtre **Réseau**, vous ne pourrez pas les pinguer ou y accéder par leur nom.

