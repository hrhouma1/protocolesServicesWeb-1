
#  Ateliers ARP et MITM

--------
## Vidéos d'Introduction - Table CAM + ARP
--------

Avant de commencer les ateliers, il est fortement recommandé de visionner les vidéos suivantes pour une compréhension claire des concepts sous-jacents :

- **Vidéo 1 :** [Comprendre la table CAM et le protocole ARP](https://www.youtube.com/watch?v=e_k5K9MB-Mo)
- **Vidéo 2 :** [Explications détaillées du fonctionnement du protocole ARP](https://www.youtube.com/watch?v=SiOkTKrcjwo)


--------
## Introduction
--------

Cher.e.s étudiant.e.s,

Je tiens à préciser que les manipulations proposées dans cette section ne sont en aucun cas obligatoires. L'objectif de cet exercice est avant tout de vous permettre de discuter et de comprendre les concepts théoriques sous-jacents, tels que l'empoisonnement ARP et l'attaque Man-in-the-Middle (MITM). Il s'agit d'une opportunité pour vous d'approfondir vos connaissances sur ces sujets, sans nécessairement passer à la pratique. Prenez ce temps pour réfléchir aux implications de ces attaques, aux mesures de prévention, et à l'importance de la sécurité des protocoles dans les réseaux.

Bon apprentissage !


--------
## Description des Manipulations
--------

### 1. Théorie ARP et MITM

Le premier document, `arp_1_theorie_MITM.md`, explore les fondements théoriques de l'empoisonnement ARP et de l'attaque MITM. Vous y trouverez un schéma illustrant le déroulement d'une attaque MITM ainsi qu'une explication détaillée des mécanismes impliqués.

### 2. Tutoriel Pratique sur Ubuntu

Les documents `arp_2_tutoriel_MITM_Ubuntu.md` et `arp_3_suite_tutoriel_MITM_Ubuntu.md` vous guident à travers un tutoriel complet pour simuler une attaque MITM sur une machine virtuelle Ubuntu 22.04. Les étapes incluent la configuration réseau, l'activation du transfert IP, l'empoisonnement ARP, et l'utilisation d'outils comme `mitmproxy` pour intercepter le trafic.

### 3. Détection et Mitigation

Dans `arp_4_detection_mitigation_MITM.md`, vous découvrirez les méthodes de détection et de mitigation pour se protéger contre les attaques MITM. Ce document détaille les bonnes pratiques à adopter pour sécuriser un réseau contre ce type d'attaque.

### 4. Étapes pour Installer les Certificats `mitmproxy`

Le fichier `arp_5_etapes_MITM.md` décrit les étapes pour installer les certificats générés par `mitmproxy` sur les machines victimes. Cette configuration est nécessaire pour que les requêtes HTTP soient correctement interceptées par l'attaquant sans provoquer d'erreurs liées aux certificats.

### 5. Configuration et Problèmes Courants

Le document `arp_6_configuration_MITM.md` explique comment configurer correctement une machine attaquante pour exécuter une attaque MITM, tandis que `arp_7_probleme_redirection_HTTP.md` et `arp_8_acceptation_certificats.md` discutent des problèmes potentiels que vous pourriez rencontrer, tels que la redirection HTTP et l'acceptation des certificats sur les machines victimes.

---

En suivant ce guide, vous serez en mesure de comprendre et d'explorer en profondeur les mécanismes des attaques ARP et MITM, tout en apprenant à configurer les systèmes pour les protéger contre ces menaces.
