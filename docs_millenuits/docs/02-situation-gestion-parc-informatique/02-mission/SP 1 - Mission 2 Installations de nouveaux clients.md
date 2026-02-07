**SP 1 : Gestion du Parc Informatique**

**Mission 2 : Installation du PC maître**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)
## Informations générales

- __Auteur__ : Biseray Louis
- __Date de création__ : 05/02/2026

___

# Guide de Création d'un Poste Maître (Master) - Windows 11

Ce document détaille la procédure de mise en place d'un **Poste Maître**. Ce poste sert de modèle de référence : une fois configuré, son image disque sera capturée puis déployée à l'identique sur l'ensemble du parc informatique client.

## 1. Préparation de l'Environnement

Avant de débuter, assurez-vous de disposer des ressources nécessaires pour l'isolation du système maître.

### Prérequis Matériels et Logiciels

- **Hyperviseur :** VirtualBox (recommandé) ou tout autre logiciel de virtualisation.

- **Image ISO :** Windows 11 (version officielle).

- **Stockage :** 30 Go d'espace libre minimum.

- **Mémoire vive (RAM) :** 4 Go minimum alloués à la VM.


> [!IMPORTANT] **Sécurité et Intégrité :** Téléchargez l'ISO exclusivement sur le site de [Microsoft](https://www.microsoft.com). Utilisez le code de hachage (SHA-256) fourni sur le site pour vérifier que le fichier n'a pas été corrompu ou altéré durant le téléchargement.

---

## 2. Configuration de la Machine Virtuelle (VirtualBox)

L'utilisation d'une machine virtuelle permet de travailler dans un environnement sain avant la capture de l'image.

### Création de la VM

1. **Initialisation :** Créez une nouvelle machine, nommez-la (ex: `Master_Win11`).

2. **Type d'OS :** Sélectionnez `Microsoft Windows`, version `Windows 11 (64-bit)`.

3. **Ressources :** Allouez la RAM (4 Go+) et créez un disque dur virtuel (30 Go+).


### Montage de l'ISO

Pour que la machine puisse démarrer sur l'installateur Windows :

1. Allez dans les **Configuration** de la VM > onglet **Stockage**.

2. Dans l'arborescence, sélectionnez l'icône du disque vide (lecteur optique).

3. Cliquez sur l'icône du disque à droite et choisissez votre fichier `Win11_25H2_French_x64.iso`.

4. Validez et lancez la machine.


---

## 3. Installation du Système d'Exploitation

Cette phase installe la base du système qui sera commune à tous les futurs postes.

### Phases initiales

- Définissez la langue et la disposition du clavier.

- Lors de l'installation, choisissez **"Personnalisé : installer uniquement Windows"** pour formater le disque proprement.

- Sélectionnez **"Je n'ai pas de clé de produit"** (l'activation se fera post-déploiement).

- Choisissez l'édition **Windows 11 Professionnel**.


### Création du compte temporaire (`ThrowAway`)

L'objectif est de créer un premier compte pour passer les écrans de configuration initiale (OOBE).

1. Choisissez une configuration pour un **usage professionnel** (ou entreprise).

2. Dans les options de connexion, ne connectez pas de compte Microsoft. Choisissez **"Se joindre au domaine à la place"** pour forcer la création d'un **compte local**.

3. Nommez ce compte `ThrowAway`.


---

## 4. Gestion des Comptes Utilisateurs

Une fois sur le bureau, nous allons créer le compte d'administration définitif et supprimer les traces du compte temporaire.

### Création du compte Administrateur final

Nous utilisons l'outil de gestion des comptes avancés pour plus de précision.

1. Faites `Windows + R`, tapez **`netplwiz`** et validez.

2. Cliquez sur **Ajouter**, puis choisissez **"Se connecter sans compte Microsoft"** (Compte local).

3. **Identifiants :**

	- Nom d'utilisateur : `Admin`

    - Mot de passe : `Mille_2026`

    - Indice : `Mille`

4. **Élévation des privilèges :** Une fois créé, double-cliquez sur `Admin` dans la liste, allez dans l'onglet **Appartenance au groupe** et sélectionnez **Administrateur**.


### Nettoyage du poste

1. Déconnectez la session `ThrowAway`.

2. Connectez-vous avec le nouveau compte `Admin`.

3. Retournez dans la gestion des utilisateurs (ou via les Paramètres) et **supprimez le compte `ThrowAway`** ainsi que ses fichiers.

___
## 5. Installation des applications

Afin que le poste maître soit opérationnel dès son déploiement, nous installons un socle logiciel standard. Il est recommandé de télécharger les versions de ces applications sur le site des concepteurs

- **Navigateur Web Firefox (dernière version) :** Alternative sécurisée et performante au navigateur par défaut, permettant une gestion centralisée des préférences.

Etapes : 

- Aller sur le site de ``Firefox``
- Télécharger l'installeur
- Autoriser ``Firefox``
- Ajouter ``Firefox`` à la taskbar et en moteur principal
- Définir ``Firefox`` par défaut
- Transférer les données entre ``Edge`` et `Firefox`
- Cliquer sur `Commencer la navigation`


- **LibreOffice :** Suite bureautique complète et gratuite (Traitement de texte, Tableur, Présentation), compatible avec les formats Microsoft Office.

Etapes : 

- Aller sur le site de ``LibreOffice``
- Télécharger l'installeur pour ``Windows x64``
- Sélectionner le mode d'installation normale
- Créer un raccourci sur le bureau
- Autoriser `LibreOffice`


- **Lecteur PDF (Adobe Acrobat Reader) :** Logiciel indispensable pour la consultation de documents administratifs et techniques avec une gestion précise de l'impression.

Etapes :

- Aller sur le site de ``Adobe`` 
- Télécharger l'installeur de ``Adobe Acrobate Reader``
- Autoriser l'installeur
- 


- **VLC Media Player :** Lecteur multimédia universel capable de lire presque tous les formats audio et vidéo sans ajout de codecs tiers.

Etapes :

-