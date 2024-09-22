--------------------------------------------
# 01 - Configuration du laboratoire
--------------------------------------------

```
          +-------------------+
          | Linux Ubuntu      |
          | Serveur           |
          | IP: 192.168.2.10  |
          | Msk: 255.255.255.0|
          +-------------------+
                  |
                  |
          +-------+-------+
          |   NATNetwork  |
          |   192.168.2.0 |
          +-------+-------+
                  |
     +------------+------------+
     |                         |
+----+----+               +----+----+
| Windows 10|             | Linux    |
| IP:       |             | Ubuntu   |
| 192.168.2.11|           | Desktop  |
| Msk:       |            | IP:      |
| 255.255.255.0|          | 192.168.2.12|
| DNS:       |            | Msk:       |
| 192.168.2.10|           | 255.255.255.0|
|             |           | DNS:       |
|             |           | 192.168.2.10|
+-------------+           +--------------+
```

### Configuration

- **Linux Ubuntu Serveur** : IP 192.168.2.10, masque 255.255.255.0
- **Windows 10** : IP 192.168.2.11, masque 255.255.255.0, DNS 192.168.2.10
- **Linux Ubuntu Desktop** : IP 192.168.2.12, masque 255.255.255.0, DNS 192.168.2.10

Les machines utilisent un réseau NAT avec l'adresse de sous-réseau 192.168.2.x pour permettre la communication entre elles, et le serveur Linux Ubuntu agit comme serveur DNS pour les autres machines du réseau.


--------------------------------------------
# 02 - Configuration du laboratoire
--------------------------------------------


# Installation d'Ubuntu Server

1. **Télécharger l'image ISO :**
   - Rendez-vous sur le site officiel d'Ubuntu pour télécharger l'image ISO du serveur Ubuntu.

2. **Suivre les instructions d'installation :**
   - Choisissez la langue, configurez le réseau, et entrez un nom d'hôte.
   - Configurez le disque dur (utilisez LVM si souhaité).
   - Sélectionnez les services à installer (SSH, LAMP, etc.).

3. **Finaliser l'installation :**
   - Retirez le support de démarrage et redémarrez le serveur[3][4][5].

# Installation d'Ubuntu Desktop

1. **Télécharger l'image ISO :**
   - Téléchargez l'image ISO d'Ubuntu Desktop depuis le site officiel.

2. **Créer un support de démarrage :**
   - Utilisez Rufus pour créer une clé USB bootable.

3. **Démarrer à partir de la clé USB :**
   - Insérez la clé USB dans votre PC et redémarrez. Sélectionnez la clé USB dans le menu de démarrage.

4. **Suivre les instructions d'installation :**
   - Sélectionnez "Installer Ubuntu".
   - Choisissez votre langue, disposition du clavier, et type d'installation (normal ou minimal).
   - Configurez le partitionnement du disque (effacez tout pour une installation propre).

5. **Finaliser l'installation :**
   - Entrez vos informations utilisateur et terminez l'installation[2][7].

# Installation de Windows 10

1. **Télécharger l'outil de création de média :**
   - Téléchargez l'outil depuis le site officiel de Microsoft.

2. **Créer un support de démarrage :**
   - Utilisez l'outil pour créer une clé USB bootable avec Windows 10.

3. **Démarrer à partir de la clé USB :**
   - Insérez la clé USB dans votre PC et redémarrez. Accédez au BIOS/UEFI pour définir la clé USB comme premier périphérique de démarrage.

4. **Suivre les instructions d'installation :**
   - Sélectionnez "Installer maintenant", entrez votre clé produit si nécessaire.
   - Choisissez "Installation personnalisée" pour une installation propre.
   - Formatez le disque dur si nécessaire et sélectionnez-le pour l'installation.

5. **Finaliser l'installation :**
   - Configurez vos préférences utilisateur après le redémarrage[8][9].

Ces étapes vous guideront à travers l'installation des systèmes d'exploitation sur vos machines respectives. Assurez-vous que vos données sont sauvegardées avant de commencer, car certaines étapes peuvent effacer vos disques existants.






--------------------------------------------
# 03 - Configuration du laboratoire
--------------------------------------------



--------------------------------------------
# 04 - Configuration du laboratoire
--------------------------------------------
