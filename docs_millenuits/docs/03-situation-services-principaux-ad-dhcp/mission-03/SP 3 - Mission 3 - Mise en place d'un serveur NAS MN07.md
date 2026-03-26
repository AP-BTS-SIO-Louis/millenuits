# SP 3 - Mission 3 - Mise en place d'un serveur NAS avec authentification Active Directory et redirection de dossiers MN07

**SP 3 : Gestion des services principaux AD (Active Directory) et DHCP**

**Mission 3 : Mise en place d'un serveur NAS MN07

**Contexte : MILLENUITS**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---
## Informations générales

- **Date de création** : 19/03/2026
- **Dernière modification** : 19/03/2026
- **Auteur** : BISERAY Louis

___

## Prérequis et installation 

### A. Prérequis :

Le choix du Nas est donc tourner vers True NAS. Nous disposons dans l'hyperviseur Nutanix de un ISO de TrueNas Scale.

Les paramètres mis dans la VM sont les suivants :

- Réseau :
	- Vlan 51 
- Deux disques :
	-  Un disque : 20 Go pour l'installation
	- Un disque : 40 Go pour le stockage
- Mot de passe : 
	- `0247249690``

### B. Installations :

Lancer la VM nous envoie dans la page d'installation de TrueNas, Il y a 4 Options disponible :

```
1  Install/Upgrade
2  Shell
3  Reboot System
4  Shutdown System
```

L'options qui nous intéresse ici est le `Install Upgrade` .

Il sera demander de choisir le support sur le quel sera installer l'installer de TrueNas dans notre cas nous allons l'installer sur le disque à 20 Go

Il nous est demander de créé un mot de passe qu'il faudra retenir dans notre cas le mot de passe est `0247249690` .

Une fois l'installation effectuer :

