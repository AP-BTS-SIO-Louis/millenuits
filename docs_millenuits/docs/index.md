# 📚 Introduction

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

Bienvenue sur le site de documentation technique du projet **MilleNuits**. Ce site centralise l'ensemble des procédures, architectures et configurations mises en place dans le cadre de la refonte de l'infrastructure réseau et de la gestion du parc informatique de l'entreprise.

---
## 🏢 Contexte

**MilleNuits** est une entreprise leader sur le marché français de la fabrication de couettes et oreillers. Basée sur deux sites (Baugé-en-Anjou pour la production/administratif et Joué-lès-Tours pour la logistique), l'entreprise connait une forte croissance.

Suite à des incidents de sécurité récents (propagation de virus) et pour accompagner l'arrivée de nouveaux collaborateurs, la DSI a lancé un plan de modernisation s'articulant autour de deux situations professionnelles (SP) majeures :

### 1. Gestion de l'infrastructure réseau (SP1)
L'objectif est de sécuriser les flux et de segmenter le réseau.

- **Séparation des flux** : Mise en place de VLANs (Administratif, Production, Logistique, etc.).
- **Adressage** : Refonte du plan d'adressage IP (VLSM).
- **Mobilité** : Déploiement d'une infrastructure Wi-Fi sécurisée pour les visiteurs et les commerciaux.

### 2. Gestion du parc informatique (SP2)
Le service administratif nécessite un renouvellement matériel et logiciel.

- **Migration** : Passage de Windows 8 à Windows 11 avec suite logicielle standardisée.
- **Industrialisation** : Mise en place d'une solution de déploiement (FOG) pour l'installation massive des postes.
- **Support** : Implémentation d'un outil de gestion d'incidents (GLPI) pour structurer le support utilisateur.

---

## 🎯 Compétences Mises en Œuvre

Ce projet mobilise les compétences du référentiel **BTS SIO (Option SISR)** suivantes :

- **Gérer le patrimoine informatique** :
    * Recensement et gestion des incidents.
    * Déploiement centralisé de postes clients.
- **Répondre aux incidents et aux demandes d’assistance et d’évolution** :
    * Mise en place d'outils de ticketing.
    * Rédaction de fiches procédures et conseils utilisateurs.
- **Administrer les systèmes et les réseaux** :
    * Configuration des éléments actifs (Routeurs, Switchs Cisco).
    * Administration des VLANs et du routage inter-VLAN.
- **Protéger les données à caractère personnel et la vie privée** :
    * Segmentation du réseau et sécurisation des accès Wi-Fi.

---

## 🛠️ Utilisation du Dépôt et Workflow

Ce site de documentation est généré automatiquement à partir de fichiers Markdown hébergés sur notre dépôt Git.

### Structure du dépôt
L'arborescence est organisée par mission pour faciliter la navigation :
```Bash
 📂 `config/` # Contient les fichiers de configuration brute des équipements (Switchs, Routeurs).
 📂 `docs_millenuits/docs/` # Racine de la documentation.
     `01-situation-gestion-infrastructure-reseau` # Documents relatifs à la SP1.
     `02-situation-gestion-parc-informatique` # Documents relatifs à la SP2.
```

### Workflow de rédaction (Obsidian & Git)
Nous utilisons **Obsidian** comme éditeur principal pour garantir une rédaction fluide en Markdown.

1.  **Rédaction** : Les fiches sont rédigées localement dans le coffre Obsidian lié au dossier `docs_millenuits/docs`.
2.  **Commit & Push** : Une fois la documentation validée, les modifications sont poussées sur le dépôt central.
3.  **Déploiement** : Un workflow GitHub Actions (`publish.yaml`) détecte les changements et régénère automatiquement le site statique via **MkDocs** sur GitHub Pages.

---

## 👥 Auteurs

Ce contexte est réalisé par des étudiants en BTS SIO au lycée Paul-Courier (Tours).

* **Louis MEDO** - [LinkedIn](https://www.linkedin.com/in/louismedo/) [Portfolio](https://louis.loutik.fr/) [GitHub](https://github.com/FireToak)
* **Louis BISERAY** - [LinkedIn](#) [Portfolio](#) [GitHub](#)

---
*Dernière mise à jour : Février 2026*