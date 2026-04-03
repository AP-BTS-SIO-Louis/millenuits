# SP 4 - Mission 1 - Installation de Portainer sur MN21

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

  - A. Préparation de la persistance des données
  - B. Déploiement du conteneur Portainer
  - C. Vérification et premier accès

---

## A. Préparation de la persistance des données

1.  **Création du volume Docker dédié.** Cette action provisionne un espace de stockage géré nativement par Docker, isolé du système de fichiers hôte classique, pour stocker la base de données de Portainer.

    ```bash
    sudo docker volume create portainer_data
    ```

    `docker volume create` : Commande de création d'un volume persistant.

    `portainer_data` : Nom explicite attribué au volume pour faciliter sa gestion future (sauvegardes, migrations).

---

## B. Déploiement du conteneur Portainer

1.  **Exécution du conteneur avec configuration de production.** Lancement du service avec attachement au socket Docker local et mappage des ports nécessaires.

    ```bash
    sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.40.0-alpine
    ```

    `-d` (detach) : Exécute le conteneur en arrière-plan et libère le terminal.

    `-p 8000:8000` : Ouvre le port TCP 8000 pour les connexions des agents Portainer Edge (utile si d'autres nœuds doivent être gérés ultérieurement).

    `-p 9443:9443` : Expose l'interface d'administration Web de Portainer en HTTPS (certificat auto-signé généré par défaut).

    `--name portainer` : Assigne un nom fixe au conteneur, évitant un nom aléatoire et facilitant son pilotage en CLI.

    `--restart=always` : Stratégie de résilience SRE. Le démon Docker redémarrera automatiquement ce conteneur en cas de crash, ou lors du redémarrage du serveur MN21.

    `-v /var/run/docker.sock:/var/run/docker.sock` : Bind mount critique. Permet à Portainer d'interagir avec l'API du démon Docker local pour le piloter.

    `-v portainer_data:/data` : Monte le volume persistant créé précédemment dans le répertoire `/data` du conteneur.

    `portainer/portainer-ce:2.40.0-alpine` : Cible l'image officielle sur le Docker Hub, version communautaire.

---

## C. Vérification et premier accès

1.  **Vérification du statut du service.** S'assurer que le conteneur est en cours d'exécution et écoute sur les ports définis.

    ```bash
    sudo docker ps --filter "name=portainer"
    ```

    `docker ps` : Liste les conteneurs actifs.

    `--filter "name=portainer"` : Isole l'affichage au seul conteneur nommé "portainer" pour une lecture concise.

2.  **Initialisation de l'administrateur.** Accéder à l'interface web pour configurer le compte root de l'application.

      * Ouvrez un navigateur web et accédez à : `https://[IP-DU-SERVEUR-MN21]:9443`
      * Acceptez l'avertissement de sécurité lié au certificat auto-signé.
      * Créez le mot de passe du compte administrateur initial (cette étape doit être réalisée dans les 5 minutes suivant le lancement du conteneur par mesure de sécurité).

---

## Annexe

- [Install Portainer CE with Docker on Linux](https://docs.portainer.io/start/install-ce/server/docker/linux)