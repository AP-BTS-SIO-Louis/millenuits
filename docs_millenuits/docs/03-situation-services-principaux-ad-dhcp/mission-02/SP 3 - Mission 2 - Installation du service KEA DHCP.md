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

> Le fichier de configuration de Kea utilise le format JSON.

1. **Édition du fichier.** Ouvrez le fichier de configuration principal :

```
sudoedit /etc/kea/kea-dhcp4.conf
```

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

```bash
sudo systemctl restart kea-dhcp4-server
```

> _Explication : `systemctl restart` arrête puis relance le service (daemon) pour qu'il lise la nouvelle configuration._

2. **Activation au démarrage.** Assurez-vous que le service démarre automatiquement avec la machine :

```bash
sudo systemctl enable kea-dhcp4-server
```

> _Explication : `systemctl enable` crée un lien symbolique pour que le système d'exploitation lance ce service à chaque démarrage du serveur._

3. **Vérification du statut.** Contrôlez qu'aucune erreur de syntaxe n'entrave le fonctionnement :

```bash
sudo systemctl status kea-dhcp4-server
```

> _Explication : `systemctl status` affiche l'état actuel du service (actif, en échec) et les derniers journaux (logs), idéal pour le débogage._

| **Nom**       | **Passerelle** | **IP de début (Machine)** | **Nb machines (sans passerelle)** | **Masque décimal** |
| ------------- | -------------- | ------------------------- | --------------------------------- | ------------------ |
| Production    | 172.40.0.254   | 172.40.0.1                | 253                               | 255.255.255.0      |
| Autres        | 172.40.1.126   | 172.40.1.1                | 125                               | 255.255.255.128    |
| Administratif | 172.40.1.254   | 172.40.1.129              | 125                               | 255.255.255.128    |
| VentesEtudes  | 172.40.2.62    | 172.40.2.1                | 61                                | 255.255.255.192    |
| Logistique    | 172.40.2.126   | 172.40.2.65               | 61                                | 255.255.255.192    |
| Invité        | 172.40.2.142   | 172.40.2.129              | 13                                | 255.255.255.240    |
| Management    | 172.40.2.158   | 172.40.2.145              | 13                                | 255.255.255.240    |
| Serveurs      | 172.16.51.252  | 172.16.51.1               | 253                               | 255.255.255.0      |