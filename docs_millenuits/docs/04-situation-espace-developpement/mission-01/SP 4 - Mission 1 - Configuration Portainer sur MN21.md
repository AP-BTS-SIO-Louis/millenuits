# SP 4 - Mission 1 - Configuration de Portainer sur MN21

**SP 4 : Mise en place d’un espace de développement**

**Mission 1 : Mise en place d’un environnement de test conteneurisé dans une DMZ interne avec Docker et préparation d’une formation développeurs.**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---

## Informations générales

  - **Date de création** : 03/04/2026
  - **Dernière modification** : 03/04/2026
  - **Mainteneur** : MEDO Louis

---

## Sommaire

  - A. Initialisation et Sécurisation de l'accès
  - B. Configuration des Accès Développeurs (RBAC)
  - C. Mise à disposition du catalogue de services (Self-Service)

---

## A. Initialisation et Sécurisation de l'accès

1.  **Définition du compte de secours Administrateur.** Lors de la première connexion sur `https://172.16.51.21:9443`, un compte d'urgence doit être créé. Ce compte ne sera utilisé qu'en cas de perte d'accès aux comptes nominatifs.

  * Générer un nom d'utilisateur non standard (éviter "admin") via Bitwarden.
  * Générer un mot de passe très fort (ex: 30 caractères) via Bitwarden et le sauvegarder dans le coffre-fort de l'équipe système.

---

## B. Configuration des Accès Développeurs (RBAC)

L'objectif est d'isoler les développeurs dans un espace contrôlé en appliquant le principe de moindre privilège.

1.  **Création du groupe logique "Développeurs".**

  * Aller dans **Users** > onglet **Teams**.
  * Saisir `Developpeurs` et cliquer sur **Create team**.
  * *Explication* : L'attribution des droits se fait par équipe, ce qui simplifie l'onboarding de nouveaux développeurs.

2.  **Création des comptes nominatifs "Standard User".**

  * Aller dans **Users** > onglet **Users**.
  * Créer les comptes (ex: `pierre.jean`), attribuer un mot de passe initial et **décocher** impérativement *Administrator*.
  * Dans *Team memberships*, affecter l'utilisateur à l'équipe `Developpeurs`.

3.  **Délégation de l'environnement de développement.**

  * Aller dans **Environments-related** dans le menu de gauche et cliquer sur **Environnements**.
  * Cliquez sur **Manage access**, sélectionner l'équipe `Developpeurs` et cliquer sur **Create access**.
  * *Explication* : Les développeurs ont désormais le droit de créer des stacks, conteneurs et volumes, mais uniquement au sein de cet environnement.

  [Interface de gestion des droits](./assets/portainer03_gestion-des-droits-environnements.png)

---

## C. Mise à disposition du catalogue de services (Self-Service)

En tant que SRE, nous facilitons le travail des développeurs en préconfigurant les connexions externes et en fournissant des modèles de déploiement sécurisés.

1.  **Connexion aux registres d'images (Registries).** Si l'équipe de développement utilise un dépôt privé (GitLab, Harbor) pour stocker ses images.

  * Aller dans **Registries** > **Add registry**.
  * Configurer les accès au registre privé. Cela permettra aux Stacks des développeurs de télécharger (pull) les images automatiquement sans gérer l'authentification dans leurs scripts.

2.  **Création d'un "Custom Template" standardisé.** Mettre à disposition un modèle Docker Compose prêt à l'emploi pour les développeurs.

  * Aller dans votre environnement (`local`) **Templates** > **Custom** > **Add Custom Template**.
  * Nommer le template `Environnement Web/DB` et insérer les templates ci-dessous :

  👉 [Template - Dev - Environnement WEB DB](./template_docker-compose_environnement-web-db.yaml)