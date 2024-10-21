# La Haute Disponibilité (HA) pour le DHCP - Théorie

#### Introduction à la Haute Disponibilité (HA)

La haute disponibilité (HA) désigne un ensemble de principes et de technologies visant à garantir la continuité des services informatiques, en minimisant les interruptions et en assurant une reprise rapide en cas de défaillance. Pour tout service critique en réseau, comme le serveur DHCP (Dynamic Host Configuration Protocol), la haute disponibilité est cruciale. Le DHCP est responsable de la distribution d’adresses IP dynamiques et de la configuration réseau aux clients. Toute interruption du service DHCP peut entraîner une incapacité pour les périphériques du réseau à obtenir une adresse IP et, par conséquent, à communiquer sur le réseau.

Les principales stratégies pour assurer la haute disponibilité dans un environnement DHCP sont :
1. **Le fractionnement d’étendue (Split Scope)**
2. **Le basculement DHCP (DHCP Failover)**
3. **Le clustering de basculement (Failover Clustering)**

Chacune de ces approches a ses propres avantages, inconvénients, et cas d’utilisation spécifiques. Elles garantissent toutes la continuité du service DHCP en répartissant la charge entre plusieurs serveurs et en fournissant des mécanismes de secours en cas de panne d’un serveur.

---

#### 1. Fractionnement d’étendue (Split Scope)

Le **fractionnement d’étendue** est une méthode simple et largement utilisée pour assurer la haute disponibilité du service DHCP. Dans cette approche, une étendue DHCP (qui définit une plage d’adresses IP à distribuer) est répartie entre deux serveurs DHCP indépendants. Typiquement, **80 %** des adresses de l’étendue sont attribuées à un serveur principal et les **20 %** restants à un serveur de secours.

**Principes du fractionnement d'étendue :**
- Le serveur principal attribue la majorité des adresses IP (80 % par défaut).
- Le serveur de secours prend le relais et attribue des adresses si le serveur principal devient indisponible.
- Les deux serveurs fonctionnent indépendamment et ne synchronisent pas les informations sur les baux IP, mais ils partagent la responsabilité de la même plage d’adresses.

Cette méthode est simple à mettre en place et ne nécessite pas de synchronisation active entre les deux serveurs. Toutefois, elle a l'inconvénient que les baux ne sont pas partagés ou répliqués, ce qui peut entraîner une duplication des baux en cas de basculement manuel.

**Avantages** :
- Simplicité de configuration et d’administration.
- Réduction des coûts, car aucun matériel ou logiciel spécifique n’est requis.

**Inconvénients** :
- Pas de synchronisation des baux entre les serveurs.
- Répartition statique des adresses, qui peut ne pas être efficace en cas de forte demande soudaine.

---

#### 2. Basculement DHCP (DHCP Failover)

Le **basculement DHCP** est une fonctionnalité introduite dans les versions récentes de Windows Server qui permet à deux serveurs DHCP de partager dynamiquement la responsabilité de fournir des adresses IP et des configurations réseau aux clients. Contrairement au fractionnement d’étendue, cette méthode assure une synchronisation active entre les deux serveurs, ce qui signifie que les informations de bail sont répliquées en temps réel. Ainsi, en cas de panne d’un serveur, l’autre peut prendre le relais sans interruption du service.

Le basculement DHCP fonctionne selon deux modes :
- **Mode actif/passif** : Un serveur est actif et l’autre est en mode veille (serveur de secours). Le serveur passif ne prend le relais qu’en cas de défaillance du serveur principal.
- **Mode actif/actif** : Les deux serveurs sont actifs et se partagent la charge de distribution des adresses IP. Ce mode inclut un mécanisme d’équilibrage de charge (load balancing), où les requêtes DHCP sont réparties entre les deux serveurs.

**Principes du basculement DHCP :**
- Les serveurs DHCP répliquent en continu les informations de bail pour éviter les conflits d’adresses IP.
- Le basculement automatique se produit lorsque l’un des serveurs est hors ligne.
- Le mode d’équilibrage de charge permet de répartir la charge en fonction des besoins du réseau (par exemple, 50/50).

**Avantages** :
- Synchronisation continue des baux entre les serveurs.
- Permet une transition sans interruption pour les clients DHCP.
- Flexibilité dans la répartition des charges.

**Inconvénients** :
- Configuration plus complexe que le fractionnement d’étendue.
- Nécessite des ressources supplémentaires en raison de la synchronisation des serveurs.

---

#### 3. Cluster de basculement (Failover Clustering)

Le **cluster de basculement** est une solution plus avancée qui repose sur la fonctionnalité de clustering de Windows Server. Dans cette approche, plusieurs serveurs DHCP sont regroupés dans un **cluster**. Un cluster est une collection de serveurs qui fonctionnent ensemble pour fournir un service unique. En cas de panne d’un serveur, un autre serveur du cluster prend en charge les tâches de manière transparente.

Dans un cluster de basculement DHCP, il existe deux adresses IP virtuelles :
- Une adresse IP qui représente le cluster lui-même sur le réseau.
- Une adresse IP qui représente le service DHCP du cluster.

**Principes du clustering de basculement :**
- Le cluster assure que, même si un ou plusieurs serveurs tombent en panne, les autres membres du cluster continuent à fournir le service DHCP.
- Le service DHCP utilise une adresse IP virtuelle pour garantir que les clients DHCP communiquent toujours avec le même service, indépendamment du serveur physique qui exécute le rôle DHCP.
- Les serveurs du cluster partagent un espace de stockage commun pour garantir la continuité de la gestion des baux IP.

**Avantages** :
- Solution robuste offrant un haut niveau de disponibilité.
- Tolérance de panne sans interruption du service DHCP.
- Gestion centralisée du service DHCP.

**Inconvénients** :
- Nécessite une infrastructure plus complexe, y compris un stockage partagé (iSCSI, par exemple).
- Configuration plus complexe et coûteuse en termes de matériel et de ressources réseau.

---

#### Conclusion

La haute disponibilité du service DHCP est essentielle pour assurer que les clients réseau puissent obtenir des adresses IP de manière continue, même en cas de panne d’un serveur. Les trois approches – **fractionnement d’étendue**, **basculement DHCP**, et **cluster de basculement** – offrent des solutions adaptées à différents niveaux de complexité et de besoins en disponibilité. 

Le choix de la méthode dépendra de l’infrastructure réseau, du budget, et du niveau de tolérance aux pannes requis par l’organisation. Le fractionnement d’étendue est une solution simple et peu coûteuse, tandis que le basculement DHCP et le clustering de basculement offrent des niveaux plus élevés de continuité de service, au prix d’une plus grande complexité.
