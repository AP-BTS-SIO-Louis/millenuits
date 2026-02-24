# SP 2 - Mission 4 - Choix de l'outil de ticketing

**SP 2 : Gestion parc informatique**

**Mission 4 : Installation de GLPI**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

* Date de création : 24/02/2025
* Dernière modification : 24/02/2025
* Auteur(s) : Louis MEDO

---
## Sommaire

- [A. Expression des besoins](#a-expression-des-besoins)
- [B. Les solutions du marché](#b-les-solutions-du-marché)
  - [B.1 GLPI](#b1-glpi)
  - [B.2 osTicket](#b2-osticket)
  - [B.3 Zammad](#b3-zammad)
  - [B.4 Hesk](#b4-hesk)
  - [B.5 Récapitulatif des solutions](#b5-récapitulatif-des-solutions)
- [C. Solution retenue](#c-solution-retenue)

---
## A. Expression des besoins

> Tout d'abord, nous allons définir les besoins de Millenuits pour connaître nos critères dans le choix du logiciel.

| ID     | Catégorie                 | Description du besoin                                                                                                   |
| :----- | :------------------------ | :---------------------------------------------------------------------------------------------------------------------- |
| **B1** | **Coût**                  | La solution ne doit pas être payante.                                                                                   |
| **B2** | **Hébergement**           | L'outil doit pouvoir être installé sur le serveur Linux MN06.                                                           |
| **B3** | **Portail Utilisateur**   | Les salariés (167 employés sur les deux sites doivent pouvoir ouvrir un ticket et en suivre l'avancement.               |
| **B4** | **Gestion Technicien**    | Les 3 informaticiens doivent pouvoir déclarer, prioriser, s'attribuer, escalader, résoudre et clôturer des incidents.   |
| **B5** | **Base de connaissances** | L'outil doit permettre de documenter la résolution des incidents pour un partage de connaissances entre informaticiens. |
| **B6** | **Pilotage DSI**          | Le DSI doit pouvoir obtenir des statistiques (nombre d'incidents en cours, résolus, temps de résolution).               |

---
## B. Les solutions du marché

> Nous présentons les différentes solutions du marché afin d'avoir une vue globale des solutions existantes ainsi que leurs fonctionnalités.

### B.1 GLPI

[https://glpi-project.org/](https://glpi-project.org/)

| Fonctionnalités clés |
| :--- |
| Gestion des tickets et incidents ITIL |
| Base de connaissances centralisée |
| Statistiques et tableaux de bord avancés |
| Gestion de parc informatique intégrée |

| Avantages | Inconvénients |
| :--- | :--- |
| Solution open-source très complète et gratuite. | L'interface peut sembler chargée pour les novices. |
| Très grande communauté francophone. | Nécessite un temps de configuration initial important. |

### B.2 osTicket

[https://osticket.com/](https://osticket.com/)

| Fonctionnalités clés |
| :--- |
| Création de tickets via email ou portail web |
| Affectation et routage automatique |
| Rapports basiques |

| Avantages | Inconvénients |
| :--- | :--- |
| Très léger et simple à installer sur serveur Linux. | Interface utilisateur vieillissante. |
| Configuration rapide et intuitive. | Moins de fonctionnalités ITIL (comme la base de connaissances avancée). |

### B.3 Zammad

[https://zammad.org/](https://zammad.org/)

| Fonctionnalités clés |
| :--- |
| Support multicanal (Email, Web, Chat) |
| Interface web moderne et réactive |
| Suivi des temps et historique |

| Avantages | Inconvénients |
| :--- | :--- |
| Expérience utilisateur (UX) excellente. | Prérequis techniques plus lourds (Elasticsearch, Redis). |
| Très rapide à l'usage. | Communauté francophone moins développée. |

### B.4 Hesk

[https://www.hesk.com/](https://www.hesk.com/)

| Fonctionnalités clés |
| :--- |
| Helpdesk basique |
| Base de connaissances simple |
| Système de catégories |

| Avantages | Inconvénients |
| :--- | :--- |
| Extrêmement léger et rapide à déployer. | Trop basique pour une gestion fine des escalades. |
| Facile à utiliser pour les salariés. | Statistiques très limitées pour le DSI. |

### B.5 Récapitulatif des solutions

| Critères du cahier des charges                | GLPI | osTicket | Zammad | Hesk |
| :-------------------------------------------- | :--: | :------: | :----: | :--: |
| **B1** - Solution gratuite                    |  🟢  |    🟢    |   🟢   |  🟢  |
| **B2** - Installation serveur Linux           |  🟢  |    🟢    |   🟢   |  🟢  |
| **B3** - Portail de suivi utilisateur         |  🟢  |    🟢    |   🟢   |  🟢  |
| **B4** - Gestion complète par les techniciens |  🟢  |    🟢    |   🟢   |  ⚙️  |
| **B5** - Base de connaissances intégrée       |  🟢  |    ⚙️    |   🟢   |  🟢  |
| **B6** - Statistiques avancées pour le DSI    |  🟢  |    ⚙️    |   🟢   |  ⚙️  |
⚙️ : ne possède pas (ou ne remplit pas pleinement) le critère demandé.
🟢 : remplit pleinement le critère demandé.

---
## C. Solution retenue

**GLPI (Gestionnaire Libre de Parc Informatique)**
GLPI est une solution open-source de gestion des services informatiques (ITSM) et de gestion de parc informatique. Elle intègre nativement un puissant module de Helpdesk conforme aux bonnes pratiques ITIL.

**Arguments du choix :**
GLPI répond parfaitement aux contraintes imposées par la DSI de Millenuits. C'est une solution gratuite qui s'installera sans difficulté sur le serveur Linux MN06. Elle intègre nativement la gestion du cycle de vie des tickets (priorisation, attribution, escalade) requise pour l'équipe de 3 informaticiens. De plus, son module de base de connaissances permettra d'arrêter la perte de savoir technique, et ses outils de reporting fourniront au DSI les statistiques de résolution attendues.