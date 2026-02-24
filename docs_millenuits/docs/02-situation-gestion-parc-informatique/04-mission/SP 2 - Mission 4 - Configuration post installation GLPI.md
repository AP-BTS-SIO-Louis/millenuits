# SP 2 - Mission 4 - Configuration post installation GLPI

**SP 2 : Gestion parc informatique**

**Mission 4 : Installation de GLPI**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

- Date de création : 12/02/2025
- Dernière modification : 12/02/2025
- Mainteneur : Louis MEDO

---
## Sommaire

- A. Sécurité et nettoyage post-installation
- B. Configuration de la structure de l'entreprise
- C. Création des groupes et utilisateurs locaux

---
## A. Sécurité et nettoyage post-installation

1. **Suppression du dossier d'installation.** Il est vital de supprimer ce répertoire pour empêcher quiconque de relancer l'installation et d'écraser la configuration sur le serveur MN06.

```Bash
rm -r /var/www/glpi/install
```

_Explication de la commande :_ `rm` (remove) sert à supprimer. L'option `-r` (récursif) permet de vider tout le contenu du dossier et de ses sous-dossiers.

2. **Modification des mots de passe par défaut.** GLPI installe des comptes standards (glpi, tech, normal, post-only) dont le mot de passe est identique à l'identifiant. Connectez-vous avec le compte `glpi` et modifiez-les tous dans le menu _Administration > Utilisateurs_ pour sécuriser l'accès à l'outil.

3. **Sécurisation du compte Super-Admin.** Pour éviter les attaques par force brute sur l'identifiant par défaut, renommez le compte `glpi` en `coussin-chaise-longue` et appliquez un mot de passe fort issu du Bitwarden de l'entreprise. 
   
   Allez dans *Administration > Utilisateurs*, cliquez sur l'utilisateur `glpi`. Modifiez le champ "Identifiant", insérez le nouveau mot de passe, puis cliquez sur "Sauvegarder".

3. **Création des comptes administrateurs nominatifs.** Pour garantir la traçabilité des actions (principe de non-répudiation), chaque membre de l'équipe informatique doit posséder son propre accès administrateur. 
   
   Allez dans *Administration > Utilisateurs*, cliquez sur le bouton "Ajouter". Renseignez les informations (Identifiant, Nom, Prénom, Mot de passe) et ajoutez-le. Ensuite, sélectionnez ce nouvel utilisateur, allez dans l'onglet *Habilitations*, et ajoutez-lui le profil "Super-Admin" (ou "Admin") sur l'entité racine "Mille Nuits".

---
## B. Configuration de la structure de l'entreprise

1. **Création des entités.** L'entité permet de définir et cloisonner l'organisation. Allez dans _Administration > Entités_ et renommez l'entité par défaut (Root entity) en "MilleNuits".

2. **Ajout des lieux.** Dans _Configuration > Intitulés > Général > Lieux_, créez l'arborescence géographique afin de localiser facilement les problèmes remontés. Ajoutez le "Site Baugé" et le "Site logistique Joué-Lès-Tours".
   
   ![](https://raw.githubusercontent.com/AP-BTS-SIO-Louis/millenuits/main/docs_millenuits/docs/02-situation-gestion-parc-informatique/04-mission/assets/capture-ecran_creation-lieu.png)

---
## C. Création des groupes et utilisateurs locaux

1. **Configuration des profils.** Dans _Administration > Profils_, vérifiez les droits : _Self-Service_ pour les salariés qui déclarent les pannes , _Technician_ pour les informaticiens qui attribuent et résolvent les incidents , et _Super-Admin_ pour que le DSI puisse obtenir ses statistiques de temps de résolution.

2. **Création manuelle des utilisateurs.** Puisque le serveur AD n'est pas lié, allez dans _Administration > Utilisateurs_ pour créer manuellement les comptes des techniciens et des premiers utilisateurs.