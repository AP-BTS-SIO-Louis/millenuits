**SP 2 : Gestion du Parc Informatique**

**Mission 3 : Outil de déploiement des postes**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

___
## Informations générales

- __Auteur__ : Biseray Louis
- __Date de création__ : 19/03/2026
- **Sujet** : Installation de FOG

___

# Introduction

Maintenant que le server Fog est fonctionnel et en place il faut enregistre le poste Maitre que Fog va "Aspirer" et ensuite le déployer, pour cette Mission le pc maitre est sur VirtualBox il y a donc plusieurs configuration à faire.

Il y a donc 3 étapes :

- **Paramétrage de VirtualBox pour "Aspirer" le poste maitre**
- **Aspiration et sauvegarde du poste maitre**
- **Déploiement sur un poste vierge **

___

## Paramétrage de VirtualBox

### 1. Le Réseau : Passer en "Pont" (Bridged)

Par défaut, VirtualBox met les machines en mode **NAT**. Dans ce mode, le serveur FOG ne pourra jamais contacter le Windows 11.

- Aller dans les **Paramètres** de la machine virtuelle Windows 11.

- Section **Réseau**.

- Mode d'accès : Choisir **Accès par pont**.

- Sélectionner la carte réseau physique (Ethernet ou Wi-Fi) celle qui est sur le même réseau que le serveur FOG.


### 2. Le Type de carte réseau 

FOG utilise un noyau Linux (FOS) pour capturer les images. Certaines cartes virtuelles de VirtualBox sont mal reconnues.

- Dans l'onglet **Réseau** -> **Avancé**.

- Type de carte : Choisir **Intel PRO/1000 MT Desktop** (ou Server).

### 3. Activer le Boot Réseau (PXE)

Pour que FOG prenne la main au démarrage, la VM doit chercher le serveur sur le réseau avant de lancer le disque dur.

- Dans **Système** -> onglet **Carte mère**.

- Dans **Ordre d'amorçage**, cocher **Réseau** et le placer tout en haut de la liste avec les flèches.

### 4. Le défi de l'EFI et du Secure Boot

Windows 11 exige l'EFI.

- Dans **Système** -> **Carte mère**, s'assurer que **Activer l'EFI** est coché.

---

### Résumé de la manipulation :

1. Fais ton **Sysprep** dans la VM Windows 11.

2. Éteins la VM.

3. Vérifie les réglages **Réseau (Pont)** et **Ordre de Boot (Réseau en premier)**.

4. Sur ton interface FOG, lance la tâche de **Capture**.

5. Démarre la VM. Elle devrait afficher l'écran "iPXE", charger le noyau FOG, et commencer à envoyer les données.