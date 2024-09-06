# Scénario IPv4 seul, sans NetBIOS et sans découverte réseau**

- étapes à suivre pour résoudre cet exercice :

# **1. Ouvrez une session en tant qu'administrateur sur Serv1 et Serv2 :**

   - Connectez-vous sur les deux serveurs **Serv1** et **Serv2** avec les identifiants administrateurs.

# **2. Désactivez IPv6 et NetBIOS pour IPv4 :**

   - Ouvrez les paramètres réseau :
     - Appuyez sur `Win + R`, tapez `ncpa.cpl`, puis appuyez sur **Entrée**.
   - Faites un clic droit sur la **carte réseau** utilisée pour la communication locale, puis cliquez sur **Propriétés**.
   
   - **Désactivez IPv6 :**
     - Dans la boîte de dialogue des propriétés, décochez **Protocole Internet Version 6 (TCP/IPv6)**.
     
   - **Désactivez NetBIOS pour IPv4 :**
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)** pour ouvrir ses paramètres.
     - Dans la boîte de dialogue IPv4, cliquez sur **Avancé**.
     - Dans les paramètres avancés, allez à l'onglet **WINS**.
     - Sélectionnez **Désactiver NetBIOS sur TCP/IP**, puis cliquez sur **OK**.
     
   - Cliquez sur **OK** pour appliquer les modifications et fermez toutes les fenêtres.

# **3. Redémarrez la machine :**

   - Ouvrez une invite de commande en mode administrateur sur **Serv1**, et exécutez la commande suivante pour redémarrer la machine :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette étape pour **Serv2**.

# **4. Testez la connectivité entre Serv1 et Serv2 :**

   - Une fois les serveurs redémarrés, reconnectez-vous en tant qu'administrateur sur **Serv2**.
   - Ouvrez une invite de commande et essayez de **pinguer Serv1** par son nom :
     ```
     ping Serv1
     ```
   - Vous devriez recevoir un message indiquant que l’hôte est introuvable. Cela est normal puisque **NetBIOS** est désactivé et qu'aucun serveur DNS n'est configuré pour résoudre les noms d'hôtes.

   - Maintenant, **pinguez par l'adresse IP** :
     ```
     ping 192.168.0.1
     ```
   - Vous devriez recevoir une réponse, confirmant que la connectivité réseau est fonctionnelle, mais que la résolution de noms échoue.

# **5. Testez le partage de fichiers en utilisant l'adresse IP :**

   - Ouvrez la boîte de dialogue **Exécuter** (appuyez sur `Win + R`) sur **Serv2**.
   - Tapez :
     ```
     \\192.168.0.1
     ```
   - Appuyez sur **Entrée**.
   - Une fenêtre devrait s'ouvrir affichant les dossiers partagés de **Serv1**. Si la connexion est réussie, vous verrez les dossiers partagés (par exemple, le dossier des imprimantes).

# **6. Testez le partage de fichiers en utilisant le nom d'hôte (ce qui va échouer) :**

   - Dans la boîte de dialogue **Exécuter** (appuyez sur `Win + R`), tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - Vous devriez recevoir un message **Erreur Réseau** indiquant que **Windows ne peut pas accéder à \\Serv1**.

# **7. Vérifiez l'état de la découverte du réseau :**

   - Ouvrez **l'Explorateur de fichiers** sur **Serv2**.
   - Dans le panneau de gauche, cliquez sur **Réseau**.
   - Vous remarquerez que **aucune autre machine n'apparaît**, et une bannière jaune devrait indiquer que **la découverte du réseau est désactivée**.

---

### **Résumé des observations :**

- **Ping par IP fonctionne**, mais **ping par nom d'hôte échoue**, car NetBIOS est désactivé et aucun serveur DNS n'est configuré pour la résolution de noms.
- **Le partage de fichiers fonctionne via l'adresse IP**, mais échoue avec le nom d'hôte.
- **La découverte du réseau est désactivée**, donc les machines ne sont pas visibles dans le panneau **Réseau** de **l'Explorateur de fichiers**.

Cet exercice montre comment la désactivation de **NetBIOS** et de la **découverte réseau** affecte la résolution des noms et le partage de fichiers, vous laissant seulement l'option d'utiliser les **adresses IP** pour la connectivité.

