**SP 2 : Gestion du Parc Informatique**

**Mission 3 : Outil de déploiement des postes**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

___
## Informations générales

- __Auteur__ : Biseray Louis
- __Date de création__ : 17/02/2026
- **Sujet** : Utilisation FOG

___

# Introduction 

Déployer Windows 11 avec FOG demande quelques étapes cruciales, notamment à cause des spécificités de Windows 11 (TPM, Secure Boot) et de la manière dont FOG gère les images.

Voici la marche à suivre pour transformer ta machine virtuelle (VM) en un "Master" prêt à être distribué.

---

## 1. Préparation du Master (Côté Windows 11)

Avant de capturer l'image, tu dois "généraliser" Windows pour qu'il puisse s'adapter à différents matériels.

- **Le Sysprep :** Ouvre l'invite de commande en administrateur et lance la commande suivante : `C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown`

    - `/oobe` : Lance l'écran de configuration initiale au prochain démarrage.

    - `/generalize` : Supprime les identifiants uniques (SID).

    - **Attention :** Une fois la VM éteinte, **ne pas la rallumer** avant la capture, sinon le Sysprep est annulé.


## 2. Configuration sur l'interface Web de FOG

Connecte-toi à ton serveur FOG via ton navigateur.

### Créer l'image

1. Va dans **Image Management** -> **Create New Image**.

2. Donne-lui un nom (ex: "Win11_Master").

3. **Paramètres importants :**

    - **Image Type :** "Single Disk - Resizable" (pour pouvoir déployer sur des disques plus petits ou plus grands).

    - **Partition :** "Everything".


### Enregistrer l'hôte (la VM)

1. Va dans **Host Management** -> **Add New Host**.

2. Saisis l'adresse MAC de ta VM VirtualBox.

3. Associe cet hôte à l'image que tu viens de créer ("Win11_Master").


---

## 3. Capture de l'image

C'est le moment où les données passent de VirtualBox vers ton serveur Debian.

1. Sur l'interface FOG, va sur la fiche de ton hôte (la VM), clique sur l'onglet **Task** puis sur **Capture**.

2. Démarre ta VM VirtualBox.

3. **Boot réseau (PXE) :** S'assurer que la VM démarre sur le réseau (Bridged Adapter dans les réglages VirtualBox).

4. FOG va prendre le relais automatiquement, copier le disque dur vers le serveur Debian et s'éteindre une fois terminé.


---

## 4. Déploiement sur d'autres machines

Une fois que ta barre de progression de capture est finie :

1. Crée un nouvel hôte dans FOG avec l'adresse MAC de la machine cible.

2. Associe-lui l'image "Win11_Master".

3. Va dans **Task** -> **Deploy**.

4. Démarre la machine cible en PXE.