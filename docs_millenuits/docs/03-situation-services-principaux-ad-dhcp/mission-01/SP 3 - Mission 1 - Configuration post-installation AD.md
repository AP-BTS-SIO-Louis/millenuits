# SP 3 - Mission 1 - Configuration post-installation AD

**SP 3 : Gestion des services principaux AD (Active Directory) et DHCP**

**Mission 1 : Mise en place du serveur Windows Server 2019**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

- **Date de création** : 05/02/2026
- **Dernière modification** : 12/03/2026
- **Auteur** : MEDO Louis

---
## Sommaire

1. Création unité d'organisation
2. Création utilisateur

---
## 1. Création unité d'organisation

> L'objectif d'une Unité d'Organisation (UO) est de structurer logiquement l'annuaire Active Directory. Pour le réseau MILLENUITS, la bonne pratique exige de créer une UO distincte pour chaque service de l'entreprise. Chaque UO regroupera les utilisateurs, les postes de travail et les imprimantes propres à ce service, facilitant la délégation d'administration et l'application des stratégies de groupe (GPO).

1. **Ouverture de la console.** Depuis le **Gestionnaire de serveur**, cliquez sur le menu **Outils**, puis sélectionnez **Utilisateurs et ordinateurs Active Directory**.

2. **Initialisation de la création.** Dans l'arborescence à gauche, faites un clic droit sur le nom du domaine (ou l'UO parente), sélectionnez **Nouveau**, puis **Unité d'organisation**.

3. **Paramétrage.** Renseignez le nom du service (ex: `Comptabilite`, `Ressources_Humaines`).

> **Bonne pratique :** Laissez toujours la case "Protéger le conteneur contre une suppression accidentelle" cochée pour sécuriser l'arborescence.

4. **Validation.** Cliquez sur **OK** pour finaliser la création. Recommencez l'opération pour créer des sous-UOs si nécessaire (ex: UO "Utilisateurs" et UO "Ordinateurs" dans l'UO du service).

---
## 2. Création utilisateur

> La création d'un utilisateur permet d'attribuer une identité unique sur le domaine. Sur le réseau MILLENUITS, il est impératif de respecter une convention de nommage standardisée (ex: *p.nom* pour la première lettre du prénom et le nom). L'utilisateur doit être créé directement au sein de l'UO de son service d'affectation pour hériter des bons droits et paramètres.

1. **Sélection de l'emplacement.** Dans **Utilisateurs et ordinateurs Active Directory**, naviguez et cliquez sur l'UO du service concerné.

2. **Initialisation de la création.** Faites un clic droit dans la zone centrale vide, choisissez **Nouveau**, puis **Utilisateur**.

3. **Saisie de l'identité.** Renseignez les champs **Prénom** et **Nom de famille**. Le champ **Nom d'ouverture de session de l'utilisateur** (le login) doit respecter la nomenclature établie. Cliquez sur **Suivant**.

4. **Configuration de la sécurité.** Saisissez un mot de passe temporaire respectant la politique de complexité (au moins 8 caractères, majuscules, minuscules, chiffres, caractères spéciaux).

> **Bonne pratique :** Cochez impérativement l'option "L'utilisateur doit changer le mot de passe à la prochaine ouverture de session". Cela garantit que seul l'employé connaîtra son mot de passe définitif.

5. **Validation.** Cliquez sur **Suivant**, vérifiez le récapitulatif des informations, puis cliquez sur **Terminer**.