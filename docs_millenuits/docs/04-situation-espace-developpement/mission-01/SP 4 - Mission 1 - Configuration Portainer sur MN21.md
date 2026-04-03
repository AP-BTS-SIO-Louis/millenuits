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
  - B. Configuration du RBAC (Rôles et Groupes)
  - C. Optimisation de l'Interface et des Ressources
  - D. Préparation du Workflow Développeur (Stacks & Registries)

---

## A. Initialisation et Sécurisation de l'accès

1.  **Définition du mot de passe Administrateur.** Lors de la première connexion sur `https://172.16.51.21:9443`, un mot de passe fort doit être généré. Nous utiliserons ensuite des comptes nominatifs pour les administrateurs, mais nous gardons ce compte en secours au cas où nous n'aurions plus accès au compte nominatif.

    a. Utiliser bitwarden pour générer un nom d'utilisateur aléatoire.
    b. Utiliser bitwarden pour générer un mot de passe fort pour ce compte.

    [Image à faire : Capture de l'écran de création du compte initial Portainer avec les identifiants masqués]

---

## B. Configuration du RBAC (Rôles et Groupes)

Le principe de moindre privilège impose de ne pas donner de droits "Admin" aux développeurs. Portainer permet une segmentation fine via le RBAC (Role-Based Access Control).

1.  **Création de l'équipe "Développeurs".** Centraliser les utilisateurs dans une équipe pour faciliter l'attribution des permissions sur les ressources (conteneurs, volumes).

      * Dans le menu de gauche, cliquez sur **Users**, puis sur l'onglet **Teams**.
      * Dans le champ *Name*, saisissez `Developpeurs` et cliquez sur **Create team**.
      * *Explication* : L'utilisation d'équipes (Teams) évite la gestion des droits utilisateur par utilisateur, ce qui réduit les erreurs humaines.

    [Image à faire : Capture de l'onglet Teams avec l'équipe Developpeurs nouvellement créée]

2.  **Création des comptes et attribution du rôle "Standard User".** Ce rôle permet aux utilisateurs de gérer exclusivement les ressources qu'ils ont créées ou qui leur ont été assignées, sans pouvoir modifier la configuration du nœud Docker (MN21).

      * Allez dans `Users` > `Users`.
      * Ajoutez un utilisateur (ex: `dev_user`), définissez son mot de passe et décochez impérativement la case *Administrator*.
      * Dans la section *Team memberships*, sélectionnez l'équipe `Developpeurs` créée précédemment.

3.  **Restriction de l'accès à l'environnement.** Dans la section *Environments*, limitez l'accès au endpoint "local" uniquement à l'équipe "Développeurs".

      * Allez dans **Environments** dans le menu de gauche.
      * Cliquez sur votre environnement (généralement nommé `local`).
      * Allez dans l'onglet **Manage access**.
      * Sélectionnez l'équipe `Developpeurs` dans la liste et cliquez sur **Create access**.
      * *Explication* : Cela garantit que seuls les membres de cette équipe peuvent interagir avec le démon Docker de ce serveur spécifique.

    [Image à faire : Capture de l'écran "Manage access" de l'environnement local avec l'équipe Developpeurs ajoutée]

---

## C. Optimisation de l'Interface et des Ressources

L'interface doit être épurée pour éviter les erreurs de manipulation et faciliter le dépôt de fichiers/images.

1.  **Activation et utilisation de la gestion des Stacks.** S'assurer que les développeurs peuvent utiliser `docker-compose` via l'onglet *Stacks*. Cela leur permet de lier facilement PHP, MariaDB et leur frontend Javascript.

      * Allez dans votre environnement `local`.
      * Cliquez sur **Stacks** dans le menu de gauche.
      * Cliquez sur **Add stack** pour vérifier que l'éditeur Web est bien disponible et fonctionnel.
      * *Explication* : L'onglet Stacks est le point d'entrée principal pour l'approche Infrastructure as Code (IaC) simplifiée des développeurs.

2.  **Configuration des volumes persistants.** Portainer permet de créer des volumes via l'interface. Les développeurs pourront y déposer leurs fichiers de configuration ou leurs données de base de données.

      * Dans le menu de gauche de l'environnement, cliquez sur **Volumes**.
      * Cliquez sur **Add volume**.
      * Nommez le volume (ex: `projet_db_data`) et laissez les options par défaut (`local` driver), puis validez avec **Create the volume**.
      * Dans *Manage access* du volume créé, restreignez l'accès à l'équipe `Developpeurs` pour éviter que d'autres équipes n'altèrent les données.

    [Image à faire : Capture de l'interface de création d'un Volume avec la section "Access control" visible]

---

## D. Préparation du Workflow Développeur (Stacks & Registries)

Pour que les développeurs puissent tester leurs applications de bout en bout, Portainer doit être prêt à recevoir leurs scripts et images.

1.  **Configuration d'un Registry (Optionnel).** Si les développeurs utilisent des images personnalisées stockées sur Docker Hub ou un registre privé, configurez les identifiants dans *Registries*.

      * Dans le menu principal (hors environnement), cliquez sur **Registries**.
      * Cliquez sur **Add registry**.
      * Choisissez le fournisseur (Docker Hub, GitLab, Custom) et entrez les *credentials* (Username/Password ou Token) fournis par les développeurs.
      * *Explication* : Cela permet à Portainer de s'authentifier automatiquement pour pull des images privées lors du déploiement d'une Stack.

    [Image à faire : Capture du formulaire d'ajout d'un Registry personnalisé]

2.  **Mise en place des Templates de Stacks.** Pour accélérer le développement, il est recommandé de créer des "Custom Templates" avec des structures Docker Compose types (ex: Stack LAMP : Linux Apache MariaDB PHP).

      * Dans le menu principal, allez dans **App Templates** puis dans l'onglet **Custom Templates**.
      * Cliquez sur **Add Custom Template**.
      * Nommez-le "Stack LAMP de Base" et collez le code suivant dans l'éditeur Web :

    ```yaml
    version: '3.8'
    services:
      web:
        image: php:8.2-apache
        volumes:
          - web_data:/var/www/html
      db:
        image: mariadb:10.11
        environment:
          MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_password
        volumes:
          - db_data:/var/lib/mysql
    volumes:
      web_data:
      db_data:
    ```

      * Ce script permet de déployer l'intégralité de l'environnement en un clic via l'interface Portainer.

3.  **Vérification de l'isolation.** Tester avec un compte utilisateur "Développeur" que l'accès aux paramètres système de Portainer et au Shell de l'hôte MN21 est strictement impossible.

      * Déconnectez-vous du compte Administrateur.
      * Connectez-vous avec le compte `dev_user` créé à l'étape B.
      * Vérifiez que les menus `Settings`, `Registries` (mode édition) et `Users` sont invisibles.