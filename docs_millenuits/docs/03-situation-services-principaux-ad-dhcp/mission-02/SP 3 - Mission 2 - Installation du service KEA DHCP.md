# SP 3 - Mission 2 - Installation et configuration de KEA DHCP

**SP 3 : Gestion des services principaux AD (Active Directory) et DHCP**

**Mission 2 : Mise en place du serveur DHCP sous Linux**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

- **Date de création** : 05/02/2026
- **Dernière modification** : 19/03/2026
- **Mainteneur** : MEDO Louis

---
## Sommaire

- A. Installation du service KEA DHCP
- B. Sauvegarde et préparation de la configuration
- C. Configuration globale, DNS et Passerelle
- D. Vérification et activation du service

---
## A. Installation du service KEA DHCP

1. **Mise à jour des paquets et installation.** Cette étape garantit l'installation de la dernière version stable du service DHCPv4.

```Bash
sudo apt update
sudo apt install kea-dhcp4-server
```

- `apt update` : Met à jour la liste des paquets disponibles dans les dépôts configurés.
- `apt install kea-dhcp4-server` : Télécharge et installe spécifiquement le module IPv4 du serveur Kea.

---
## B. Sauvegarde et préparation de la configuration

> **Bonne pratique :** Il est impératif de toujours sauvegarder le fichier de configuration par défaut avant d'y apporter des modifications. Cela permet un retour en arrière rapide en cas d'erreur bloquante.

1. **Création d'une copie de sauvegarde.**

```Bash
sudo cp /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.bak
```

- `cp` : Commande de copie de fichiers.
- `/etc/kea/kea-dhcp4.conf` : Fichier de configuration source (format JSON).
- `/etc/kea/kea-dhcp4.conf.bak` : Fichier de destination servant d'archive de sécurité.

---
## C. Configuration globale, DNS et Passerelle

1. **Édition du fichier de configuration.** Définition de l'interface d'écoute, des options globales (DNS, routeur) et de la plage d'adresses.

```Bash
sudo nano /etc/kea/kea-dhcp4.conf
```

- `nano` : Éditeur de texte en ligne de commande.

2. **Structure JSON de la configuration.** Remplacez le contenu par les paramètres adaptés à votre réseau.

```JSON
{
  "Dhcp4": {
    "interfaces-config": {
      "interfaces": [ "eth0" ]
    },
    "valid-lifetime": 4000,
    "subnet4": [
      {
        "subnet": "172.16.51.0/24",
        "pools": [ { "pool": "172.16.51.100 - 172.16.51.200" } ],
        "option-data": [
          {
            "name": "routers",
            "data": "172.16.51.254"
          },
          {
            "name": "domain-name-servers",
            "data": "172.16.51.10, 8.8.8.8"
          }
        ]
      }
    ]
  }
}
```

- `"interfaces": [ "eth0" ]` : Spécifie l'interface réseau sur laquelle Kea écoutera les requêtes DHCP (à adapter selon votre système, ex: `ens33`).
- `"valid-lifetime": 4000` : Temps (en secondes) pendant lequel un client peut utiliser l'adresse IP attribuée (le bail).
- `"subnet"` : Déclare l'adresse réseau et son masque (notation CIDR).
- `"pools"` : Définit la plage d'adresses IP distribuables aux clients.
- `"routers"` : Paramètre la passerelle par défaut envoyée aux clients.
- `"domain-name-servers"` : Paramètre les serveurs DNS primaire et secondaire envoyés aux clients.

---
## D. Vérification et activation du service

> **Bonne pratique :** Toujours valider la syntaxe JSON avant de relancer le service pour éviter une interruption en production.

1. **Validation de la syntaxe.**

```Bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

- `kea-dhcp4 -t` : Analyse le fichier de configuration sans démarrer le serveur pour détecter les erreurs de syntaxe JSON.

2. **Démarrage et activation.**

```Bash
sudo systemctl restart kea-dhcp4-server
sudo systemctl enable kea-dhcp4-server
sudo systemctl status kea-dhcp4-server
```

- `systemctl restart` : Redémarre le service pour appliquer la nouvelle configuration.
- `systemctl enable` : Configure le service pour qu'il se lance automatiquement au démarrage du système.
- `systemctl status` : Affiche l'état actuel du service (vérifier qu'il est "active (running)").