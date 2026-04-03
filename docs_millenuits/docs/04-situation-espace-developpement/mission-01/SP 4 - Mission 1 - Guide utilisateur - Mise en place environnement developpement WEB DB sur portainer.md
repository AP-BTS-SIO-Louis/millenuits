# SP 4 - Mission 1 - Guide Utilisateur Portainer (Étudiants SLAM)

**SP 4 : Mise en place d’un espace de développement**

**Mission 1 : Mise en place d’un environnement de test conteneurisé dans une DMZ interne avec Docker et préparation d’une formation développeurs.**

---

## Informations générales

  - **Date de création** : 03/04/2026
  - **Dernière modification** : 03/04/2026
  - **Mainteneur** : MEDO Louis

---

## Sommaire

  - A. Connexion à l'interface Portainer
  - B. Déploiement de l'environnement via le Template
  - C. Importation du code source dans le volume
  - D. Vérification et accès à l'application

---

## A. Connexion à l'interface Portainer

1.  **Accès au portail Web.** Ouvrez votre navigateur et rendez-vous sur l'URL de l'interface d'administration (acceptez l'avertissement de sécurité lié au certificat).

    ```text
    https://172.16.51.21:9443
    ```

2.  **Authentification et sélection de l'environnement.** Connectez-vous avec les identifiants (nom d'utilisateur et mot de passe) qui vous ont été fournis par l'équipe système. Une fois connecté, vous arrivez sur la page d'accueil (Home). Cliquez sur l'environnement nommé **local** pour accéder à vos ressources.

    [Capture d'écran à faire : Page d'accueil Portainer montrant le clic sur l'environnement "local"]

---

## B. Déploiement de l'environnement via le Template

Afin de garantir un environnement stable et identique pour tous, l'équipe SRE a préparé un modèle (Template) contenant un serveur Web Apache/PHP et une base de données MariaDB.

1.  **Sélection du modèle SRE.** Dans le menu de gauche, cliquez sur **App Templates**, puis accédez à l'onglet **Custom Templates**. Cliquez sur le modèle nommé `[STANDARD] Environnement Web/DB`.

    [Capture d'écran à faire : Interface Custom Templates avec le modèle sélectionné]

2.  **Déploiement de la Stack.** Une interface de configuration s'ouvre. Remplissez les informations nécessaires pour instancier votre environnement.

      * **Name** : Donnez un nom à votre projet (ex: `projet-appli-frais`, sans espaces ni majuscules).
      * Cliquez sur le bouton **Deploy the stack** en bas de la page.
      * *Note : La création peut prendre quelques secondes le temps que Portainer télécharge les images Docker.*

    [Capture d'écran à faire : Bouton "Deploy the stack" dans l'interface de création]

---

## C. Importation du code source dans le volume

Puisque vous n'avez pas d'accès SSH direct au serveur (règle de sécurité), l'ajout de vos fichiers PHP et de vos assets se fait directement via l'interface Web de Portainer, qui va les injecter dans le volume persistant.

1.  **Accès au gestionnaire de fichiers du volume.** Dans le menu de gauche, cliquez sur **Volumes**. Cherchez le volume correspondant au code de votre application (il sera généralement nommé `nomdevotrestack_app_code`). Cliquez sur son nom.

    [Capture d'écran à faire : Liste des volumes avec le volume "app\_code" mis en évidence]

2.  **Upload des fichiers.** Sur la page de détails du volume, repérez la section **Browse**.

      * Cliquez sur le bouton **Browse** pour ouvrir l'explorateur de fichiers Web.
      * Vous vous trouvez dans le dossier `/var/www/html` de votre serveur Web.
      * Utilisez le bouton **Upload** pour transférer vos fichiers source (ex: `index.php`, dossiers `css`, `js`) depuis votre ordinateur vers le conteneur.

    [Capture d'écran à faire : Interface "Browse" du volume montrant le bouton Upload]

---

## D. Vérification et accès à l'application

1.  **Vérification de l'état des conteneurs.** Dans le menu de gauche, cliquez sur **Containers**. Assurez-vous que vos deux conteneurs (le serveur Web et la base de données MariaDB) ont le statut `running`.

    [Capture d'écran à faire : Liste des conteneurs montrant les statuts "running" et les ports exposés]

2.  **Accès au site Web.** Le serveur Apache/PHP de votre template écoute sur le port `8080`. Ouvrez un nouvel onglet dans votre navigateur pour visualiser votre code en cours d'exécution :

    ```text
    http://172.16.51.21:8080
    ```