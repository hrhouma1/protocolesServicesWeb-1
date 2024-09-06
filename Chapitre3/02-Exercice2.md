# **Exercice 2 : Scénario IPv4/IPv6, sans NetBIOS, sans découverte réseau**

#### **Objectifs :**
- Activer IPv6 tout en conservant IPv4.
- Maintenir NetBIOS et la découverte réseau désactivés.
- Examiner la connectivité et la résolution de noms dans ce contexte.


---
---
---
# **1. Activer IPv6 sur Serv1 et Serv2 :**

   **Explication** : Nous allons réactiver IPv6 pour que les deux machines puissent communiquer à la fois avec leurs adresses **IPv4** et **IPv6**. La différence est qu'**IPv4** utilise des adresses de type `192.168.x.x`, tandis qu'**IPv6** utilise des adresses plus longues comme `fd00::1`.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, et appuyez sur **Entrée** pour ouvrir les connexions réseau.
   - Faites un clic droit sur la carte réseau utilisée et sélectionnez **Propriétés**.
   - **Activer IPv6** :
     - Cochez l'option **Protocole Internet Version 6 (TCP/IPv6)**.
     - Cliquez sur **OK** pour valider.
   
   **Sur Serv2 :**
   - Répétez les mêmes étapes pour **Serv2** afin d'activer IPv6.



---
---
---
# **2. Redémarrer les deux machines :**

   **Explication** : Le redémarrage est souvent nécessaire pour appliquer les changements de configuration réseau.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette commande pour **Serv2**.


---
---
---
### **3. Tester la connectivité avec ping via le nom d’hôte :**

   **Explication** : Maintenant que **IPv6** est activé et que **NetBIOS** est désactivé, essayons de **pinguer** **Serv1** en utilisant son nom d'hôte depuis **Serv2**.

   - Ouvrez une session en tant qu'administrateur sur **Serv2**.
   - Ouvrez une invite de commande (`cmd`) et tapez :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez un message indiquant que l'hôte n'a pas pu être trouvé. Cela s'explique par le fait qu'IPv6, en soi, ne résout pas les noms d’hôtes sans un service de DNS ou autre mécanisme de résolution comme **NetBIOS** ou **LLMNR** (Link-Local Multicast Name Resolution).


---
---
---
# **4. Tester la connectivité avec ping via l’adresse IPv6 :**

   **Explication** : Maintenant, vous allez essayer de pinguer **Serv1** via son adresse **IPv6** directement.

   - Toujours dans l'invite de commande sur **Serv2**, tapez :
     ```
     ping fd00::1
     ```
   - **Résultat attendu** : Vous devriez recevoir une réponse, confirmant que la machine peut être atteinte via son adresse **IPv6**.



---
---
---
# **5. Test du partage de fichiers via le nom d’hôte (qui échouera) :**

   **Explication** : Testons maintenant l'accès aux fichiers partagés sur **Serv1** en utilisant son nom d’hôte, mais cette tentative échouera, car la résolution des noms via NetBIOS est désactivée.

   - Ouvrez la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**.
   - Tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Vous recevrez un message **Erreur Réseau**, indiquant que **Windows ne peut pas accéder à \\Serv1**. Cela est attendu car **IPv6**, sans autre mécanisme de résolution de noms comme DNS, ne permet pas de se connecter à une machine via son nom d’hôte.


---
---
---
# **6. Se connecter via l’adresse IPv6 dans un chemin UNC :**

   **Explication** : Vous pouvez accéder aux fichiers partagés de **Serv1** via l’adresse IPv6, mais cela nécessite une syntaxe spéciale pour le chemin **UNC** (Universal Naming Convention). La syntaxe pour une adresse IPv6 dans un chemin UNC est un peu différente : vous remplacez les deux-points par des tirets et ajoutez le suffixe `ipv6-literal.net`.

   - Dans la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**, tapez :
     ```
     \\fd00--1.ipv6-literal.net
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Une fenêtre s'ouvre affichant le dossier partagé **Imprimante** de **Serv1**, prouvant que la connectivité fonctionne avec cette syntaxe **UNC IPv6**.



---
---
---
# **7. Vérifier la fenêtre Réseau :**

   **Explication** : Comme la **découverte réseau** est désactivée, vous ne verrez toujours pas les machines s’afficher dans l’explorateur réseau.

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Cliquez sur **Réseau** dans le panneau de gauche.
   - **Résultat attendu** : Aucune machine ne sera visible, car la découverte réseau est désactivée.

---

# **Conclusion :**

- Même avec **IPv6** activé en plus d’**IPv4**, sans **NetBIOS**, **découverte réseau** ou **DNS**, la résolution de noms d’hôtes échoue. Vous ne pouvez pas pinguer une machine par son nom d’hôte, ni vous connecter à elle via un chemin **UNC** standard.
- Vous pouvez cependant atteindre une machine via son adresse **IPv6** (par ping) ou en utilisant un chemin **UNC IPv6** spécial.
- **IPv6**, à lui seul, ne fournit pas de mécanisme de résolution de noms ; il a besoin d'un service complémentaire comme **DNS** ou **LLMNR**.


---
---
---
---


### **Conseils supplémentaires à tous le monde :**

- **IPv6** est une adresse plus complexe que **IPv4** mais offre un espace d'adressage beaucoup plus large.
- Dans un réseau local, si vous ne configurez pas un service de **résolution de noms** comme DNS, vous devrez utiliser les adresses IP directement pour accéder aux autres machines.
- Pour les débutants : n'ayez pas peur des longues adresses **IPv6**. Vous pouvez les manipuler comme les adresses **IPv4**, mais il est souvent plus simple de les copier-coller pour éviter les erreurs.
