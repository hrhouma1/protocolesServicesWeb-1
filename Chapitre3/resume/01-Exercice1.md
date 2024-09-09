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


----
# Annexe : **Pourquoi le ping par nom d'hôte fonctionne-t-il même après la désactivation de NetBIOS ?**
----

Le fait que le ping de `Serv1` fonctionne par le nom d'hôte peut indiquer que le nom d'hôte est résolu d'une autre manière que par NetBIOS.

# ==> Explications possibles :

1. **Cache de résolution DNS locale** : Si vous avez déjà résolu `Serv1` par son nom d'hôte auparavant (avant de désactiver NetBIOS), l'adresse IP de `Serv1` pourrait être encore stockée dans le cache DNS local de la machine. Ce cache permet la résolution sans qu'il soit nécessaire de passer par NetBIOS ou DNS à chaque fois. Vous pouvez vérifier cela en vidant le cache DNS avec la commande suivante :
   ```
   ipconfig /flushdns
   ```
   Ensuite, essayez de refaire un ping avec le nom d'hôte.

2. **Fichier `hosts`** : Si l'adresse IP de `Serv1` a été ajoutée manuellement dans le fichier `hosts` de Windows (situé à `C:\Windows\System32\drivers\etc\hosts`), la résolution de noms via ce fichier fonctionnera même sans DNS ou NetBIOS. Vous pouvez vérifier ce fichier pour voir si `Serv1` est présent.

3. **DNS fonctionnel** : Il est possible qu'un serveur DNS soit déjà configuré et résolve les noms d'hôtes même si vous pensiez qu'il n'était pas en place. Pour vérifier si un serveur DNS est configuré, vous pouvez exécuter la commande suivante sur la machine :
   ```
   ipconfig /all
   ```
   Recherchez la section "Serveurs DNS" pour voir s'il y a une adresse IP configurée pour un serveur DNS.

4. **NetBIOS toujours actif sur certaines interfaces** : Si vous avez plusieurs cartes réseau sur vos serveurs (par exemple, une carte réseau physique et une carte virtuelle), il se peut que NetBIOS soit encore activé sur l'une d'entre elles. Vérifiez que NetBIOS est bien désactivé sur toutes les interfaces réseau de chaque serveur.

En résumé, si le ping fonctionne via le nom d'hôte, cela signifie que la résolution des noms se fait probablement via un autre mécanisme (DNS ou fichier `hosts`). 

- Vous pouvez examiner les points mentionnés pour identifier pourquoi la résolution fonctionne malgré la désactivation de NetBIOS.
