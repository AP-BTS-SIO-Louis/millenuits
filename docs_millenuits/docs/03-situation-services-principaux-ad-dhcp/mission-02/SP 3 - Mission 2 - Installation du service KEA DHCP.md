# SP 3 - Mission 2 - Installation du service KEA DHCP

**SP 3 : Gestion des services principaux AD (Active Directory) et DHCP**

**Mission 2 : Mise en place du serveur DHCP sous Linux**

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

- **Date de création** : 05/02/2026
- **Dernière modification** : 12/03/2026
- **Auteur** : MEDO Louis

---
## Sommaire

1. Prérequis et Installation
2. Configuration multi-réseaux
3. Démarrage et Vérification

---
## 1. Prérequis et Installation

1. **Mise à jour du système.** Ouvrez le terminal et tapez :
  ```bash
  sudo apt update && sudo apt upgrade -y
  ``` 

   > *Explication : `apt update` met à jour la liste des paquets disponibles. `apt upgrade -y` installe les dernières versions de ces paquets sans demander de confirmation.*

3. **Installation de Kea.** Installez le paquet spécifique à l'IPv4 :
```bash
sudo apt install kea-dhcp4-server -y
```

   > *Explication : `apt install` télécharge et installe le logiciel Kea DHCP. Le module `dhcp4` indique qu'il s'agit du serveur de distribution d'adresses IPv4.*

---
## 2. Configuration multi-réseaux

Le fichier de configuration de Kea utilise le format JSON. Il est plus strict mais plus lisible que les anciennes configurations ISC DHCP.

1. **Édition du fichier.** Ouvrez le fichier de configuration principal :
   `sudoedit /etc/kea/kea-dhcp4.conf`

   > *Explication : `nano` est un éditeur de texte en ligne de commande. `/etc/kea/` est le répertoire standard des configurations Kea.*

3. **Déclaration des sous-réseaux.** Repérez le bloc `"subnet4": []` et ajoutez vos réseaux (exemple pour deux services distincts) :
   ```json
   "subnet4": [
       {
           "subnet": "192.168.10.0/24",
           "pools": [ { "pool": "192.168.10.100 - 192.168.10.200" } ],
           "option-data": [
               { "name": "routers", "data": "192.168.10.254" }
           ]
       },
       {
           "subnet": "192.168.20.0/24",
           "pools": [ { "pool": "192.168.20.100 - 192.168.20.200" } ],
           "option-data": [
               { "name": "routers", "data": "192.168.20.254" }
           ]
       }
   ]
```

> _Explication : `subnet` définit l'adresse réseau. `pools` définit la plage d'adresses distribuables. `option-data` permet d'envoyer des paramètres supplémentaires aux clients, ici l'adresse de la passerelle par défaut (`routers`)._

---

## 3. Démarrage et Vérification

1. **Application des paramètres.** Redémarrez le service pour prendre en compte le nouveau fichier JSON :
    
    `sudo systemctl restart kea-dhcp4-server`
    
    > _Explication : `systemctl restart` arrête puis relance le service (daemon) pour qu'il lise la nouvelle configuration._
    
2. **Activation au démarrage.** Assurez-vous que le service démarre automatiquement avec la machine :
    
    `sudo systemctl enable kea-dhcp4-server`
    
    > _Explication : `systemctl enable` crée un lien symbolique pour que le système d'exploitation lance ce service à chaque démarrage du serveur._
    
3. **Vérification du statut.** Contrôlez qu'aucune erreur de syntaxe n'entrave le fonctionnement :
    
    `sudo systemctl status kea-dhcp4-server`
    
    > _Explication : `systemctl status` affiche l'état actuel du service (actif, en échec) et les derniers journaux (logs), idéal pour le débogage._
    

```

Veux-tu que j'ajoute une étape pour configurer le serveur DNS que Kea doit distribuer aux postes clients, ou une procédure pour les réservations d'IP fixes ?
```