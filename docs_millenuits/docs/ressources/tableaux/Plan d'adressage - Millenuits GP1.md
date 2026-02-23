# Plan d'adressage - Millenuits GP1

<div style="display: flex; gap: 10px;">
  <img src="https://github.com/AP-BTS-SIO-Louis/millenuits/raw/main/images/logo_millenuits.png" width="120" height="100">
  <img src="image2.png" width="100" height="100">
</div>

---

**Mainteneur** : Louis MEDO
**Dernière édition** : 23/02/2026

---

| **Nom**       | **N° VLAN** | **Adresse Réseau** | **Masque (CIDR)**        | **Passerelle** | **Diffusion (Broadcast)** |
| ------------- | ----------- | ------------------ | ------------------------ | -------------- | ------------------------- |
| Production    | 10          | 172.40.0.0         | 255.255.255.0 (/24)      | 172.40.0.254   | 172.40.0.255              |
| Autres        | 11          | 172.40.1.0         | 255.255.255.128 (/25)    | 172.40.1.126   | 172.40.1.127              |
| Administratif | 12          | 172.40.1.128       | 255.255.255.128 (/25)    | 172.40.1.254   | 172.40.1.255              |
| VentesEtudes  | 13          | 172.40.2.0         | 255.255.255.192<br>(/26) | 172.40.2.62    | 172.40.2.63               |
| Logistique    | 14          | 172.40.2.64        | 255.255.255.192 (/26)    | 172.40.2.126   | 172.40.2.127              |
| Invité        | 15          | 172.40.2.128       | 255.255.255.240<br>/28   | 172.40.2.142   | 172.40.2.143              |
| Management    | 99          | 172.40.2.144       | 255.255.255.240<br>/28   | 172.40.2.158   | 172.40.2.159              |
| Serveurs      | 51          | 172.16.51.0        | 255.255.255.0<br>/24     | 172.16.51.252  | 172.16.51.255             |