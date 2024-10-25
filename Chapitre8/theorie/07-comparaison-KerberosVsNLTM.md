# Table comparative détaillée entre NTLM et Kerberos.

*Cette table met en évidence les différences en termes de sécurité, performance, configuration, et cas d'utilisation.*

---

| **Aspect**                               | **NTLM**                                                                                              | **Kerberos**                                                                                               |
|------------------------------------------|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Modèle d'Authentification**            | Défi-Réponse (Challenge-Response)                                                                     | Basé sur des Tickets (Ticket-Granting Ticket)                                                              |
| **Mécanisme de Sécurité**                | Utilise des hachages de mots de passe pour l'authentification                                         | Utilise des tickets cryptés et un centre de distribution de clés (KDC)                                     |
| **Architecture**                         | Pas de centre de gestion centralisée                                                                  | Requiert un KDC pour l’authentification centralisée                                                        |
| **Authentification Mutuelle**            | Non (seul le client est authentifié par le serveur)                                                   | Oui (le client et le serveur s’authentifient mutuellement)                                                 |
| **Exposition des Mots de Passe**         | Vulnérable aux attaques par force brute et hachage                                                    | Minimise l'exposition des mots de passe grâce aux tickets                                                  |
| **Synchronisation du Temps**             | Non requise                                                                                           | Requise pour la synchronisation des tickets (maximum 5 minutes de décalage recommandé)                    |
| **Complexité de Configuration**          | Relativement simple, ne nécessite pas de serveur dédié                                                | Complexe, nécessite la mise en place et la gestion d’un KDC                                                |
| **Performances Réseau**                  | Moins performant en raison des nombreux défis-réponses                                               | Plus performant avec moins d'échanges réseau grâce aux tickets                                             |
| **Sécurité**                             | Moins sécurisé ; vulnérable aux attaques de relais et aux attaques de l'homme du milieu (MITM)        | Très sécurisé ; conçu pour résister aux attaques de relais et MITM                                         |
| **Gestion de Session**                   | Génère un défi pour chaque nouvelle demande de session                                                | Le ticket est réutilisé pour une durée limitée, minimisant les demandes répétées                           |
| **Utilisation dans Active Directory**    | Protocole de secours, utilisé lorsque Kerberos n’est pas disponible ou dans des environnements anciens | Protocole principal pour Active Directory                                                                  |
| **Compatibilité**                        | Compatible avec les anciennes versions de Windows et applications plus anciennes                      | Conçu pour les environnements modernes, supporté à partir de Windows 2000                                  |
| **Interopérabilité**                     | Moins interopérable avec d'autres systèmes que Windows                                                | Interopérable avec de nombreux systèmes (ex. UNIX, Linux) via l'implémentation du protocole standard Kerberos |
| **Gestion des Identités**                | Moins avancée, pas de centralisation                                                                  | Centralisée via le KDC, permettant une gestion des identités cohérente et sécurisée                        |
| **Horodatage et Expiration**             | Non applicable                                                                                        | Les tickets expirent après un certain temps, ce qui augmente la sécurité                                   |
| **Tolérance aux Pannes**                 | Pas de point central de défaillance                                                                   | Dépendant du KDC ; si le KDC est indisponible, l'authentification Kerberos échoue                          |
| **Protocole Recommandé pour**            | Environnements avec applications anciennes ou nécessitant une compatibilité héritée                   | Environnements modernes nécessitant une sécurité avancée et une gestion centralisée des accès              |
| **Authentification Multi-Serveur**       | Complexe, chaque serveur nécessite une demande d'authentification séparée                             | Simplifiée grâce au ticket de service une fois le TGT obtenu                                               |
| **Scalabilité**                          | Moins scalable dans de grands réseaux                                                                 | Conçu pour les grandes infrastructures, hautement scalable avec une gestion centralisée                    |
| **Vulnérabilités Communes**              | Attaques de hachage, attaques par force brute, attaques de relais, man-in-the-middle (MITM)          | Vulnérabilités liées au KDC (si compromis), mais sécurisé contre les attaques de relais et MITM            |
| **Exemples d'Utilisation**               | Accès aux ressources locales dans un réseau domestique ou un ancien réseau d'entreprise               | Accès sécurisé dans un domaine Active Directory, services intranet, applications critiques                 |

---

### Conclusion

Kerberos est généralement préférable dans les environnements modernes nécessitant une sécurité avancée, une gestion centralisée des accès, et une authentification multi-serveur efficace. NTLM, bien que plus simple et compatible avec les environnements anciens, est limité en termes de sécurité et de performance pour les infrastructures modernes.
