_____
__SP 1 : Gestion de l'infrastructure réseau

__Mission 2 : Mise en place de l'infrastructure réseau__

__Contexte : MilleNuits__



____
# Informations générales

__Date de création :__ 15/01/2026
__Dernière modification :__ 15/01/2026
__Auteur :__ BISERAY Louis

____

# Sommaire

__A.__ Adressage des postes et serveurs
__B.__
__C.__

____

# __A.__ Adressage des postes et serveurs

__Adresses Postes différents services :__

```
Adresse Réseau : 172.40.0.0 /16
Pc ADM : 172.40.1.129       Masque : 255.255.255.128
Pc AUTRES : 172.40.1.1      Masque : 255.255.255.128
Pc PROD : 172.40.0.1        Masque : 255.255.255.0
Pc LOGI : 172.40.2.129      Masque : 255.255.255.192
Pc VENTE : 172.40.2.1       Masque : 255.255.255.128
```

__Adressage Serveurs :__

```
Adresse Réseau : 172.16.51.0 /24
MN01 (AD,DNS,DHCP) : 172.16.51.1        Masque : 255.255.255.0
MN02 Messagerie : 172.16.51.2           Masque : 255.255.255.0
MN03 PGI : 172.16.51.3                  Masque : 255.255.255.0
MN04 Web : 172.16.51.4                  Masque : 255.255.255.0
MN05 Sauvegarde : 172.16.51.5           Masque : 255.255.255.0
MN06 Gestion d'incidents : 172.16.51.6  Masque : 255.255.255.0
MN07 Serveur de fichiers : 172.16.51.7  Masque : 255.255.255.0
```

__Création des VLAN sur les commutateur des services :__

```
Switch>en
Switch#conf t
Switch(config)#vlan 10
Switch(config-vlan)#name Production
Switch(config-vlan)#exit
Switch(config)#exit
```

Répéter ces commandes sur les autres commutateurs et changer le nom ainsi que le numéro du Vlan en fonction du service qui est indiquer sur le commutateur :

VLAN 10 : Production
VLAN 11 : Autres
VLAN 12 : Administratif
VLAN 13 : VentesEtudes
VLAN 14 : Logistique

Nommer également les Vlan sur le Commutateur 2960 central a la maquette

Pour vérifier la création des Vlan :

```
Switch>en
Switch#sh vlan
```

Cette commande permet d'afficher tout les Vlan enregistrer ainsi que leurs noms

__Création du Trunk entre le Commutateur 2960 et le Routeur 1921 :__

```
Switch(config)#interface Gig0/2
Switch(config-if)#switchport mode trunk 
Switch(config-if)#switchport trunk allowed vlan 10,11,12,13,14
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)#no shutdown
Switch(config-if)#exit
```