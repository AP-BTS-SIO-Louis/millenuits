# SP 4 - Mission 2 - Plan d'adressage

**SP 4 : Mise en place d’un espace de développement**

**Mission 1 : Mise en place d’un environnement de test conteneurisé dans une DMZ interne avec Docker et préparation d’une formation développeurs.**

![Logo MilleNuits](https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png)

---

## Informations générales

  - **Date de création** : 02/04/2026
  - **Dernière modification** : 03/04/2026
  - **Mainteneur** : BISERAY Louis

---

## Contexte 

Il faut mettre en place un nouveaux sous-réseau pour que les développeurs puissent se connecter via un Wi-Fi spécifique, mais avant de mettre en place ce réseau il faut tout d'abord établir le plan d'adressage.

## 1. Plan d'adressage

Voici le plan d'adressage de tout le réseau avec le VLAN 16 dédier pour les développeurs,

| **Nom**       | **N° VLAN** | **Adresse Réseau** | **Masque (CIDR)**        | **Passerelle** | **Diffusion (Broadcast)** |
| ------------- | ----------- | ------------------ | ------------------------ | -------------- | ------------------------- |
| Production    | 10          | 172.40.0.0         | 255.255.255.0 (/24)      | 172.40.0.254   | 172.40.0.255              |
| Autres        | 11          | 172.40.1.0         | 255.255.255.128 (/25)    | 172.40.1.126   | 172.40.1.127              |
| Administratif | 12          | 172.40.1.128       | 255.255.255.128 (/25)    | 172.40.1.254   | 172.40.1.255              |
| VentesEtudes  | 13          | 172.40.2.0         | 255.255.255.192<br>(/26) | 172.40.2.62    | 172.40.2.63               |
| Logistique    | 14          | 172.40.2.64        | 255.255.255.192 (/26)    | 172.40.2.126   | 172.40.2.127              |
| Invité        | 15          | 172.40.2.128       | 255.255.255.240<br>/28   | 172.40.2.142   | 172.40.2.143              |
| Management    | 99          | 172.40.2.144       | 255.255.255.240<br>/28   | 172.40.2.158   | 172.40.2.159              |
| Developpeur   | 16          | 172.40.2.160       | 255.255.255.240<br>/28   | 172.40.2.174   | 172.40.2.175              |
| Serveurs      | 51          | 172.16.51.0        | 255.255.255.0<br>/24     | 172.16.51.252  | 172.16.51.255             |
| DMZ           | 53          | 10.10.10.0         | 255.255.255.0<br>/24     | 10.10.10.254   | 10.10.10.255              |

## 2. Limiter l'accès

Pour plus de sécurité il faut autoriser l'accès a cet access point seulement au développeur, nous utiliserons une pass phrase qui se définit sur notre access point et nous autorisons seulement les personnes faisant parties du bon VLAN à y accéder,  Pour que les développeur n'accède que à certaine parties du réseaux dédier il faut mettre en place des règles avec des listes d'accès,