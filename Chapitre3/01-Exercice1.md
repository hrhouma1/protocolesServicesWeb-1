----
# **Étapes préliminaires – Configuration de Serv1 et Serv2**
----

# Exercice 1

![image](https://github.com/user-attachments/assets/ea626b9b-92d6-47bd-94c3-f14ccd79c1b8)

# **1. Créer deux machines virtuelles exécutant Windows Server 2019/2022 :**

   - **Étape 1 : Choix de l'outil de virtualisation**  
     Vous devez utiliser un outil de virtualisation comme **Hyper-V**, **VirtualBox**, ou **VMware** pour créer vos machines virtuelles (VM).
     - **Si vous utilisez Hyper-V** : vous pouvez créer des machines virtuelles en suivant un assistant de création de VM.
     - **Si vous utilisez VirtualBox** : cliquez sur "Nouvelle machine", choisissez Windows Server 2019/2022, et suivez les instructions.

   - **Étape 2 : Créer Serv1 et Serv2**  
     Nommez vos deux machines virtuelles respectivement **Serv1** et **Serv2**.

   - **Étape 3 : Installer Windows Server 2019/2022**  
     Si vous commencez de zéro, installez **Windows Server 2019** ou **2022** sur chaque machine virtuelle. Suivez le processus d'installation en utilisant l'ISO de Windows Server. À la fin, vous aurez un bureau Windows Server fonctionnel sur chaque VM.

# **2. Configurer les adresses IP (IPv4 et IPv6) pour Serv1 et Serv2 :**

   **Explication** : Chaque machine sur un réseau doit avoir une adresse IP unique pour communiquer avec d'autres machines. Dans cet exercice, nous allons configurer deux types d'adresses IP : **IPv4** (192.168.0.x) et **IPv6** (fd00::x).

   **Serv1 :**
   - Ouvrez une session en tant qu'administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez `ncpa.cpl`, et appuyez sur **Entrée** pour ouvrir les connexions réseau.
   - Faites un clic droit sur la carte réseau active (celle qui permet à la machine de se connecter au réseau) et sélectionnez **Propriétés**.
   - **Configurer l'adresse IPv4 :**
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Sélectionnez l'option **Utiliser l'adresse IP suivante :** et entrez les informations suivantes :
       - **Adresse IP** : `192.168.0.1`
       - **Masque de sous-réseau** : `255.255.255.0` (cela est généré automatiquement après avoir entré l'adresse IP)
       - Cliquez sur **OK**.
   - **Configurer l'adresse IPv6 :**
     - Retournez dans les **Propriétés de la carte réseau** et double-cliquez sur **Protocole Internet Version 6 (TCP/IPv6)**.
     - Sélectionnez **Utiliser l'adresse IP suivante :** et entrez l'adresse suivante :  
       - **Adresse IP** : `fd00::1`
     - Cliquez sur **OK** pour valider.

   **Serv2 :**
   - Répétez les mêmes étapes sur **Serv2**, mais utilisez ces adresses IP :
     - **Adresse IP (IPv4)** : `192.168.0.2`
     - **Masque de sous-réseau** : `255.255.255.0`
     - **Adresse IP (IPv6)** : `fd00::2`

# **3. Activer le partage de fichiers sur les deux machines :**

   **Explication** : Le partage de fichiers vous permet d'accéder aux fichiers d'une autre machine via le réseau.

   **Serv1 :**
   - Ouvrez le **Panneau de configuration** en tapant `Win + R`, puis en entrant **control**.
   - Allez dans **Réseau et Internet** > **Centre Réseau et Partage**.
   - Dans la colonne de gauche, cliquez sur **Modifier les paramètres de partage avancés**.
   - **Activer le partage de fichiers** : Dans la section **Privé**, cochez l'option **Activer le partage de fichiers et d'imprimantes**.
   - Répétez la même opération dans la section **Invité ou Public** si nécessaire.

   **Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2** afin d'activer le partage de fichiers.

---
---
---
# **Réalisation de l'exercice : Configuration pour IPv4 seul, sans NetBIOS et sans découverte réseau**
---

Maintenant que vos machines sont configurées, nous allons procéder à l'exercice en désactivant certains services et en testant la connectivité.

# **1. Désactiver IPv6 et NetBIOS sur les deux machines :**

   **Explication** : Dans cet exercice, nous allons désactiver le protocole **IPv6** et **NetBIOS** (qui permet la résolution de noms d’hôtes sur un réseau local) pour voir comment cela affecte la communication entre les deux machines.

   **Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez `ncpa.cpl`, et appuyez sur **Entrée**.
   - Faites un clic droit sur la carte réseau utilisée et sélectionnez **Propriétés**.

   - **Désactiver IPv6 :**
     - Décochez l’option **Protocole Internet Version 6 (TCP/IPv6)** pour désactiver l'IPv6.
     - Cliquez sur **OK** pour fermer la fenêtre.

   - **Désactiver NetBIOS pour IPv4 :**
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Cliquez sur **Avancé**, puis allez dans l'onglet **WINS**.
     - Sélectionnez **Désactiver NetBIOS sur TCP/IP** et cliquez sur **OK**.

   - Répétez les mêmes étapes sur **Serv2**.

#### **2. Redémarrer les deux machines :**

   **Explication** : Après avoir modifié les paramètres réseau, il est souvent nécessaire de redémarrer les machines pour que les changements soient pris en compte.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur. Pour redémarrer la machine, tapez la commande suivante :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez cette étape pour **Serv2**.

#### **3. Tester la connectivité entre les machines :**

   **Explication** : Vous allez vérifier si les deux machines peuvent communiquer via leur adresse IP en utilisant la commande **ping**.

   **Sur Serv2** :
   - Ouvrez une session en tant qu’administrateur.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** en utilisant son **nom d’hôte** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez un message indiquant que l'hôte n'a pas pu être trouvé, car **NetBIOS** (qui permet la résolution des noms) a été désactivé et aucun DNS n'a été configuré.

   - Tapez ensuite la commande suivante pour tester la communication avec **Serv1** en utilisant son **adresse IP** :
     ```
     ping 192.168.0.1
     ```
   - **Résultat attendu** : Vous recevrez une réponse indiquant que la machine peut être jointe par son adresse IP. Cela confirme que la connectivité réseau fonctionne bien, mais que la résolution des noms échoue.

#### **4. Test du partage de fichiers via l’adresse IP :**

   **Explication** : Vous allez maintenant vérifier si vous pouvez accéder aux fichiers partagés de **Serv1** en utilisant son adresse IP.

   - Ouvrez la boîte de dialogue **Exécuter** (`Win + R`) sur **Serv2**.
   - Tapez l’adresse IP suivante :
     ```
     \\192.168.0.1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Une fenêtre s’ouvre montrant les dossiers partagés de **Serv1** (si vous avez activé le partage de fichiers). Vous pourrez accéder aux dossiers partagés si tout fonctionne.

#### **5. Test du partage de fichiers via le nom d’hôte (qui échouera) :**

   - Dans la boîte de dialogue **Exécuter** (`Win + R`), tapez :
     ```
     \\Serv1
     ```
   - Appuyez sur **Entrée**.
   - **Résultat attendu** : Vous recevrez un message d’**Erreur Réseau**, indiquant que **Windows ne peut pas accéder à \\Serv1**. C’est normal puisque la résolution de noms est désactivée.

#### **6. Vérification de la découverte du réseau :**

   **Explication** : Vous allez vérifier si les autres machines sont visibles dans le réseau.

   - Ouvrez **l’Explorateur de fichiers** sur **Serv2**.
   - Cliquez sur **Réseau** dans le panneau de gauche.
   - **Résultat attendu** : Aucune machine ne sera visible dans la fenêtre Réseau, et une bannière jaune

 indiquera que la **découverte réseau est désactivée**.



---
---
---
# **Résumé :**

1. **Ping par IP** : Cela fonctionne, ce qui signifie que les deux machines peuvent communiquer via leurs adresses IP.
2. **Ping par nom d’hôte** : Cela échoue, car **NetBIOS** et **la découverte réseau** sont désactivés, donc les machines ne peuvent pas résoudre les noms d’hôtes sans serveur DNS.
3. **Partage de fichiers via IP** : Cela fonctionne, vous pouvez accéder aux fichiers partagés avec l'adresse IP.
4. **Partage de fichiers via nom d’hôte** : Cela échoue pour les mêmes raisons de résolution des noms.
5. **Découverte réseau désactivée** : Les machines ne sont pas visibles dans la fenêtre Réseau.



----
----
----
# Annexe 1 : **Pourquoi le ping par nom d'hôte fonctionne-t-il même après la désactivation de NetBIOS ?**
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


---
----
-----

# **Annexe 2 : Vider le cache NetBIOS**

---

Pour vider le cache NetBIOS sur un système Windows, vous pouvez utiliser la commande suivante dans l'invite de commandes :

```bash
nbtstat -R
```

### **Explication de la commande :**
- **`nbtstat -R`** : Cette commande vide le cache NetBIOS des noms résolus et force la mise à jour des entrées de noms NetBIOS enregistrés.

---

### **Étapes détaillées :**
1. **Ouvrir l'invite de commandes en tant qu'administrateur** :
   - Cliquez sur **Démarrer**, tapez **cmd**, faites un clic droit sur **Invite de commandes**, puis sélectionnez **Exécuter en tant qu'administrateur**.

2. **Exécuter la commande** :
   - Tapez la commande suivante pour vider le cache NetBIOS :
     ```bash
     nbtstat -R
     ```
   - Appuyez sur **Entrée**.

3. **Confirmation** :
   - Après l'exécution, vous verrez un message indiquant que le cache NetBIOS a été vidé.

---

### **Utilité :**
- Cette commande est utile pour résoudre les problèmes de résolution des noms NetBIOS, surtout après des modifications sur le réseau ou sur les configurations d'adresses IP des ordinateurs.

---

