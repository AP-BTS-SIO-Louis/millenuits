# SP 2 - Mission 4 - Installation de GLPI

**SP 2 : Gestion parc informatique**

**Mission 4 : Installation de GLPI**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

* Date de création : 12/02/2025
* Dernière modification : 12/02/2025
* Auteur(s) : Louis MEDO

---
## Objectif

Dans cette procédure nous allons mettre en place GLPI, un outil de gestion de tickets et de parc, sur une VM Debian. L'installation s'appuie sur la version GLPI 11 et PHP 8.4.

---
## Prérequis

* Serveur Debian opérationnel avec accès Internet.
* Accès administrateur (root ou via sudo).
* Un sous-domaine ou une adresse IP configuré (ex: `support.millenuits.com`).

---
## Sommaire

* A. Préparation du serveur pour l'installation de GLPI
* B. Installation de GLPI
* Annexe de configuration
* Ressources

---
## A. Préparation du serveur pour l'installation de GLPI

### 1. Installation du socle LAMP

1. **Mise à jour du système.**  Avant toute installation, il est nécessaire de mettre à jour la liste des paquets et le système pour garantir la sécurité et la stabilité.

```BASH
apt update && apt upgrade
```

* `apt-get update` : Met à jour la liste des paquets disponibles depuis les dépôts.
* `apt-get upgrade` : Installe les mises à jour des paquets déjà installés.


2. **Installation des composants LAMP et extensions.** Installation du serveur Web (Apache2), du langage de script (PHP 8.4 via FPM pour la performance) et de la base de données (MariaDB). Nous installons également les extensions PHP requises par GLPI 11 (ex: `intl` pour la langue, `gd` pour les images, `mbstring` pour l'UTF-8).

```BASH
sudo apt-get install apache2 php8.4-fpm mariadb-server 
sudo apt install php8.4-{curl,gd,intl,mysql,zip,bcmath,mbstring,xml,bz2,ldap}
```

* `install` : Commande pour installer de nouveaux paquets.
* `apache2` : Le serveur HTTP.
* `php8.4-fpm` : FastCGI Process Manager, gère l'exécution du code PHP indépendamment d'Apache.
* `mariadb-server` : Le système de gestion de base de données.
* `{curl,gd...}` : Liste des bibliothèques additionnelles nécessaires au cœur de GLPI.

### 2. Préparation d'une base de données pour GLPI

1. **Sécurisation de MariaDB.** Exécution du script interactif pour sécuriser l'instance de base de données (définition du mot de passe root, suppression des accès anonymes et de la base de test).

```BASH
sudo mariadb-secure-installation 
```

* `mariadb-secure-installation` : Script binaire lançant l'assistant de sécurité.

  **Exemple de configuration :**
  
  - Switch to unix_socket authentification : `n`
  - Change the root password : `y`
  - Remove anonymous users : `y`
  - Dissallow root login remotely : `y`
  - Remove test database : `y`
  - Reload privilege tables : `y`

2. **Création de la base et de l'utilisateur.** Création d'une base dédiée et d'un utilisateur spécifique avec des droits restreints à cette seule base, par mesure de sécurité.
   
```bash
sudo mysql -u root -p

# Une fois dans la console SQL, saisir les lignes suivantes :

CREATE DATABASE imdeo_glpi;
GRANT ALL PRIVILEGES ON imdeo_glpi.* TO poppy@localhost IDENTIFIED BY "VoirBitwarden";
FLUSH PRIVILEGES;
EXIT;
```

* `mysql -u root -p` : Connexion à la console SQL en tant que root.
* `CREATE DATABASE` : Crée le conteneur de données.
* `GRANT ALL PRIVILEGES` : Donne tous les droits (lecture/écriture) sur la base `imdeo_glpi` à l'utilisateur `poppy`.
* `FLUSH PRIVILEGES` : Recharge les tables de droits pour appliquer les changements immédiatement.

### 3. Téléchargement de GLPI

1. **Récupération des sources.** Téléchargement de l'archive officielle de GLPI 11 depuis le dépôt GitHub dans le dossier temporaire `/tmp`.
   > 💡 Pensez à vérifier que vous télécharger la dernière version de GLPI présente dans le dépôt GitHub : https://github.com/glpi-project/glpi/releases

```BASH
cd /tmp wget https://github.com/glpi-project/glpi/releases/download/11.0.5/glpi-11.0.5.tgz
```

* `cd /tmp` : Change le répertoire courant vers le dossier temporaire.
* `wget` : Utilitaire de téléchargement de fichiers en ligne de commande.

2. **Extraction et installation.** Décompression de l'archive directement dans le répertoire racine du serveur web.
   
```BASH
sudo tar -xzvf glpi-11.0.4.tgz -C /var/www/
```

* `tar` : Utilitaire d'archivage.
* `-xzvf` : **x** (extraire), **z** (décompresser gzip), **v** (verbeux/afficher les détails), **f** (fichier cible).
* `-C` : Indique le répertoire de destination (`/var/www/`).

### 4. Préparation de l'installation

1. **Attribution des droits.** Transfert de la propriété des fichiers GLPI à l'utilisateur système d'Apache (`www-data`) pour qu'il puisse lire et écrire dans les dossiers nécessaires.

```BASH
sudo chown www-data:www-data /var/www/glpi/ -R
```

* `chown` : Change Owner (changer le propriétaire).
* `-R` : Récursif (s'applique à tous les sous-dossiers et fichiers).
* `www-data:www-data` : Utilisateur et groupe par défaut d'Apache sur Debian.
 
2. **Sécurisation des dossiers.** Déplacement des dossiers sensibles (`config`, `files`, `log`) hors de la racine web accessible publiquement, conformément aux bonnes pratiques de l'éditeur. Création des fichiers de redirection pour que GLPI retrouve ces dossiers.

```bash
# Création et déplacement

sudo mkdir /etc/glpi /var/lib/glpi /var/log/glpi
sudo chown www-data /etc/glpi /var/lib/glpi /var/log/glpi
sudo mv /var/www/glpi/config /etc/glpi
sudo mv /var/www/glpi/files /var/lib/glpi

# Création du fichier de lien 1
sudoedit /var/www/glpi/inc/downstream.php

# Ajouter le contenu suivant dans le fichier :
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}

# Création du fichier de lien 2
sudoedit /etc/glpi/local_define.php

# Ajouter le contenu suivant dans le fichier :
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi/files');
define('GLPI_LOG_DIR', '/var/log/glpi');
```

* `mkdir` : Création de répertoires (`/etc/glpi`, `/var/lib/glpi`, `/var/log/glpi`).
* `mv` : Déplacement des dossiers originaux vers les nouveaux emplacements sécurisés.
* `nano` : Éditeur de texte pour créer les fichiers `downstream.php` et `local_define.php`.

### 5. Configuration de Apache2 pour GLPI

1. **Création du VirtualHost.** Configuration d'un fichier hôte virtuel Apache pour indiquer le dossier racine (`DocumentRoot`) et les règles de réécriture d'URL.

```bash
 sudo nano /etc/apache2/sites-available/glpi.conf
 
 #  Insérer la configuration VirtualHost standard (voir dans le dépôt git de la mission 4)
```

- Configuration du VirtualHost Apache2 : [Millenuits Virtualhost GLPI](https://github.com/AP-BTS-SIO-Louis/millenuits/blob/main/docs_millenuits/docs/02-situation-gestion-parc-informatique/04-mission/configuration-virtualhost-apache2-glpi.conf)

   2. **Activation du site.** Activation de la nouvelle configuration, désactivation du site par défaut et activation du module de réécriture. 

```bash
sudo a2ensite glpi.conf
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

   * `a2ensite` : Active le fichier de configuration du site spécifié. 
   * `a2dissite` : Désactive le site par défaut (000-default). 
   * `a2enmod rewrite` : Active le module `mod_rewrite` d'Apache. 
   * `systemctl restart` : Redémarre le service pour appliquer tout changement.  

### 6. Utilisation de PHP8.4-FPM avec Apache2

1. **Liaison Apache et PHP-FPM.** Activation des modules nécessaires pour qu'Apache puisse communiquer avec le service FPM.

```BASH
sudo a2enmod proxy_fcgi setenvif 
sudo a2enconf php8.4-fpm 
sudo systemctl reload apache2
```

* `proxy_fcgi` : Module permettant à Apache de transmettre les requêtes au serveur FastCGI (PHP-FPM).
* `setenvif` : Permet de définir des variables d'environnement basées sur la requête.
* `a2enconf` : Active la configuration globale spécifiée (php8.4-fpm).

2. **Durcissement de la configuration PHP.** Modification du fichier `php.ini` de FPM pour sécuriser les cookies de session.

```bash
# Éditer /etc/php/8.4/fpm/php.ini pour modifier cookie_httponly et cookie_samesite

sudo nano /etc/php/8.4/fpm/php.ini
sudo systemctl restart php8.4-fpm.service
```

* `session.cookie_httponly = on` : Empêche le JavaScript d'accéder au cookie de session (protection XSS).
* `session.cookie_samesite = Lax` : Protège contre les attaques CSRF.
* `systemctl restart php8.4-fpm` : Applique les modifications de configuration PHP.

---
## B. Installation de GLPI

1. **Initialisation Web.** Accéder à l'URL du serveur (ex: `http://ip-serveur` ou nom de domaine). Suivre l'assistant : sélectionner la langue "Français", accepter la licence, et cliquer sur "Installer".

2. **Connexion Base de données.** À l'étape de configuration SQL:

* Serveur SQL : `localhost`
* Utilisateur SQL : `poppy`
* Mot de passe SQL : `VoirBitwarden`
* Sélectionner la base existante : `imdeo_glpi`
* Terminer l'installation et supprimer le fichier d'installation pour la sécurité : `sudo rm /var/www/glpi/install/install.php`.

---
## Annexe de configuration

1. [Configuration de GLP](#)
2. [Ouvrir un ticket sur GLPI](#)
3. [Utiliser GLPI en tant qu'helpdesk](#)

---
## Ressources

* [Installation pas-à-pas de GLPI 11 sur Debian 13](https://www.it-connect.fr/installation-pas-a-pas-de-glpi-10-sur-debian-12/)