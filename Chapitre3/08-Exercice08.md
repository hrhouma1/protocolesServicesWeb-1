# **Exercice 8 : Scénario IPv4/IPv6 avec NetBIOS et découverte réseau activés**

#### **Objectifs :**
- Activer **IPv4**, **NetBIOS**, et **IPv6** sur les deux machines.
- Observer le comportement du ping avec cette configuration et comprendre comment la résolution des noms est priorisée entre **IPv6** et **IPv4**.
- Comprendre le rôle de **LLMNR** (Link-Local Multicast Name Resolution) dans la résolution des noms lorsque **DNS** est absent.

---
---
---

# **1. Activer IPv4, IPv6 et NetBIOS sur Serv1 et Serv2 :**

   **Explication** : Dans cet exercice, nous allons activer **IPv4**, **IPv6**, et **NetBIOS** ensemble. **NetBIOS** est utilisé dans les réseaux **IPv4** pour la résolution des noms d’hôte, tandis que **LLMNR** peut résoudre les noms dans les réseaux sans **DNS** et fonctionne avec **IPv6**.

   **Sur Serv1 :**
   - Ouvrez une session en tant qu’administrateur sur **Serv1**.
   - Appuyez sur `Win + R`, tapez **ncpa.cpl**, puis appuyez sur **Entrée** pour ouvrir la fenêtre **Connexions réseau**.
   - Faites un clic droit sur la **carte réseau active** et sélectionnez **Propriétés**.
   
   - **Activer IPv4 et IPv6** :
     - Assurez-vous que les options **Protocole Internet Version 4 (TCP/IPv4)** et **Protocole Internet Version 6 (TCP/IPv6)** sont toutes les deux cochées.
   
   - **Activer NetBIOS pour IPv4** :
     - Double-cliquez sur **Protocole Internet Version 4 (TCP/IPv4)**.
     - Cliquez sur **Avancé**, puis allez dans l'onglet **WINS**.
     - Sélectionnez **Par défaut** pour activer **NetBIOS**.
     - Cliquez sur **OK** pour valider les changements.

   - Cliquez sur **OK** pour fermer toutes les fenêtres.

   **Sur Serv2 :**
   - Répétez exactement les mêmes étapes pour **Serv2**, en activant à la fois **IPv4**, **IPv6**, et **NetBIOS** pour **IPv4**.

---
---
---

# **2. Redémarrer les deux machines :**

   **Explication** : Le redémarrage est nécessaire après la modification des paramètres réseau pour que les modifications soient appliquées et que **NetBIOS** et **LLMNR** fonctionnent correctement.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez :
     ```
     shutdown -r -t 0 -f
     ```
   - Répétez la même commande sur **Serv2** pour redémarrer les deux machines.

---
---
---

# **3. Tester la connectivité via ping avec IPv4, IPv6, et NetBIOS activés :**

   **Explication** : Une fois les machines redémarrées, vous allez tester la connectivité en pingant **Serv1** depuis **Serv2**. Lorsque **IPv6**, **IPv4**, **NetBIOS**, et **la découverte réseau** sont activés sans serveur **DNS**, c’est souvent **LLMNR** qui prend le relais pour résoudre les noms d’hôte. Dans ce cas, **LLMNR** résoudra d'abord le nom en une adresse **IPv6** si elle est disponible.

   - Ouvrez une session en tant qu’administrateur sur **Serv2**.
   - Ouvrez une invite de commande (tapez **cmd** dans le menu Démarrer).
   - Tapez la commande suivante pour tester la communication avec **Serv1** :
     ```
     ping Serv1
     ```
   - **Résultat attendu** : Vous recevrez une réponse de l'adresse **IPv6** locale de **Serv1** (par exemple `fe80::...`). Cela montre que **LLMNR** a résolu le nom en une adresse **IPv6** avant de considérer une adresse **IPv4**, car **IPv6** a une priorité plus élevée que **IPv4** dans les réseaux modernes.

---
---
---

# **4. Explication détaillée du comportement de la résolution de noms :**

   **Explication** : 
   - **IPv6** a une priorité plus élevée que **IPv4** dans la plupart des systèmes, ce qui signifie que si les deux protocoles sont activés et qu'une adresse **IPv6** est disponible, elle sera utilisée en premier.
   - **NetBIOS** est utilisé pour résoudre les noms dans les réseaux **IPv4**, mais ici, **LLMNR** (Link-Local Multicast Name Resolution) intervient en l'absence de **DNS**. **LLMNR** permet de résoudre les noms sur un réseau local sans avoir besoin d'un serveur **DNS**.
   - Lors du ping, **LLMNR** résout d'abord le nom **Serv1** en une adresse **IPv6**. Si aucune adresse **IPv6** n'était disponible, le système serait passé à **IPv4**.

---
---
---

# **5. Arrêter les deux machines :**

   **Explication** : Après avoir observé la réponse du ping et compris comment **LLMNR** et **NetBIOS** travaillent ensemble dans un réseau avec **IPv6** et **IPv4**, vous pouvez maintenant arrêter les deux machines.

   - Sur **Serv1**, ouvrez une invite de commande en mode administrateur et tapez :
     ```
     shutdown -s -t 0 -f
     ```
   - Répétez cette commande sur **Serv2** pour arrêter les deux machines.

---
---
---

# **Conclusion pédagogique :**

1. **Ping via IPv6** : Dans ce scénario, avec **IPv4**, **IPv6**, **NetBIOS**, et **la découverte réseau** activés, c’est **LLMNR** qui résout les noms d’hôte lorsqu'il n'y a pas de **DNS**. Par défaut, **LLMNR** favorise **IPv6** pour la résolution des noms, ce qui explique pourquoi le ping retourne une adresse **IPv6**.
   
2. **Priorité de IPv6** : Lorsque **IPv6** et **IPv4** sont activés, la plupart des systèmes privilégient **IPv6** pour la communication. Cela est dû au fait qu'**IPv6** est conçu pour remplacer **IPv4** à long terme.

3. **LLMNR et NetBIOS** : **LLMNR** est un mécanisme de résolution des noms qui fonctionne à la fois avec **IPv4** et **IPv6**, et il est particulièrement utile dans les réseaux sans serveur **DNS**. **NetBIOS**, quant à lui, n’est utilisé que dans les réseaux **IPv4** pour résoudre les noms d’hôte.

4. **Comportement du réseau** : Dans un réseau où **IPv6** est activé en plus de **IPv4**, vous remarquerez que **IPv6** est souvent utilisé par défaut pour la communication, même si **IPv4** est activé et fonctionnel.

---
---
---

# **Conseils :**

- **LLMNR** est utile lorsque vous n'avez pas de serveur **DNS** dans votre réseau. Il vous permet de résoudre les noms d'hôte sur un réseau local sans avoir besoin d’un serveur DNS.
- **NetBIOS** est utilisé pour résoudre les noms dans un réseau **IPv4**, mais dans un environnement où **IPv6** est activé, **LLMNR** joue un rôle plus important, surtout si aucun **DNS** n’est disponible.
- Il est important de comprendre que dans un réseau moderne, **IPv6** est souvent prioritaire par rapport à **IPv4**, même si les deux protocoles sont activés.
