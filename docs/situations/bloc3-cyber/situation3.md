# Situation 3 : Routage et NAT

![Logo CUB](../../assets/logo_cub.png){ width="150" }

> :bust_in_silhouette: Fiche rédigée par : GADONNAUD Ewen
> :mortar_board: Formation : BTS SIO 2ème année - Option SISR
> :school: Établissement : Lycée Paul-Louis Courier, Tours
> :calendar: Date : Septembre 2026

---

## 1. Rédiger la table de routage du pare-feu Stormshield de l’agence

| Destination    | Masque          | Passerelle     | Interface (de sortie) | Type |
| -------------- | --------------- | -------------- | --------------------- | ---- |
| 192.36.3.0     | 255.255.255.0   | 192.36.3.254   | 192.36.3.254          | C    |
| 192.168.33.248 | 255.255.255.248 | 192.168.33.254 | 192.168.33.254        | C    |
| 192.36.253.0   | 255.255.255.0   | 192.36.253.30  | 192.36.253.30         | C    |
| 192.168.3.0    | 255.255.255.0   | 192.168.33.253 | 192.168.33.254        | S    |
| 0.0.0.0        | 0.0.0.0         | 192.36.253.254 | 192.36.253.30         | S*   |

## 2. Déterminer quelle adresse IP du WAN doit servir de passerelle pour aller sur internet ? Puis créer un objet réseau afin que cette adresse IP soit représentée dans l'interface d'administration

L'interface WAN (out) doit utiliser l'adresse IP 192.36.253.254/24 qui corresponds à au pare feu prof. Nous créons un objet Machine avec cette adresse IP dans l'onglet Objets -> Réseau 

![2. Déterminer quelle adresse IP du WAN doit servir de passerelle pour aller sur internet ? Puis créer un objet réseau afin que cette adresse IP soit représentée dans l'interface d'administration](../../assets/situations/bloc3-cyber/objet-reseau-wan.png)

## 3. Utiliser cet objet afin de pouvoir implémenter la table de routage sur le pare-feu 

L'objet créé précédemment sera utilisé comme route par défaut pour notre pare feu, afin d'envoyer les paquets entrants vers le pare feu des profs et donc, sur internet. Pour se faire, nous nous rendons dans l'onglet Réseau -> Routage, ou nous pouvons configurer la route par défaut et notre route résumée pour le réseau privé LAN.

Un objet "RéseauLANResume" est créé au préalable pour cibler tout notre LAN

![3. Utiliser cet objet afin de pouvoir implémenter la table de routage sur le pare-feu ](../../assets/situations/bloc3-cyber/routage-reseau.png)

## 4. Proposer et paramétrer une solution technique permettant aux adresses IP privées du site de pouvoir communiquer sur le réseau WAN public et internet. 

Une solution technique afin que nos adresses IP privées de notre réseau LAN puisse communiquer sur internet est la mise en place de NAT avec surcharge. Voici le tableau résumant les règles de NAT à instaurer sur le pare feu : 

| **IP Src**        | **Port Src** | **IP Dst**     | **Port Dst** | **IP Src**    | **Port Src**         | **IP Dst**     | **Port Dst** |
| ----------------- | ------------ | -------------- | ------------ | ------------- | -------------------- | -------------- | ------------ |
| 192.168.3.0/24    | Any          | Any (Internet) | Any          | 192.36.253.30 | Dynamique (éphémère) | Any (Internet) | Any          |
| 192.168.33.248/29 | Any          | Any            | Any          | 192.36.253.30 | Dynamique            | Any            | Any          |
Afin de paramétrer le NAT, nous nous rendons dans l'onglet Politique de sécurité -> Filtrage et NAT : 

![4. Proposer et paramétrer une solution technique permettant aux adresses IP privées du site de pouvoir communiquer sur le réseau WAN public et internet. ](../../assets/situations/bloc3-cyber/nat-regles.png)

## 5. Peut-on joindre le pare-feu général CUB puis les serveurs présents dans sa DMZ. Proposer une analyse des résultats obtenus.

Nous essayons de ping depuis un poste présent dans le VLAN 10 Clients un serveur présent dans la DMZ de l'agence Hong Kong (Pare Feu prof) et un serveur présent dans la DMZ du pare feu du siège 

* IP SRV DMZ HONG KONG : 192.36.8.10
* IP SRV DMZ SIEGE :  192.36.250.11

![5. Peut-on joindre le pare-feu général CUB puis les serveurs présents dans sa DMZ. Proposer une analyse des résultats obtenus.](../../assets/situations/bloc3-cyber/test-dmz-ping.png)

Nous arrivons à ping les deux serveurs, l'analyse est la suivante : 

Etant donné que notre réseau LAN est NATé, nos paquets en direction d'internet (ici, vers les différents serveurs DMZ) obtiennent l'IP source de notre patte OUT du Pare Feu de notre agence. Ainsi, nous arrivons à communiquer avec les serveurs dans les différentes DMZ car ce sont des adresses publiques.

## 6. Proposer et paramétrer une solution technique permettant aux services WEB et FTP de la DMZ d'être interrogé par le réseau WAN. 

Il suffit de ne pas utiliser de NAT pour le réseau DMZ et d'assigner à chaque serveur une adresse publique.

## 7. Réaliser une recette permettant de valider les objectifs de cette situation.

Dans l'ordre : 

* Etablir la connexion de l'agence à Internet
* Assurer la communication entre le réseau interne et le réseau public
* Rendre le service Web accessible depuis Internet
* Contrôler et sécuriser les flux entrants et sortants

Premièrement, la connexion de l'agence à Internet fonctionne, essayons d'aller sur google depuis un ordinateur présent dans le VLAN Client : 

![7. Réaliser une recette permettant de valider les objectifs de cette situation.](../../assets/situations/bloc3-cyber/internet-access.png)

Après, nous vérifions que la communication inter réseau (privé et public) fonctionne, pour se faire, il faut ping un ordinateur présent dans la DMZ depuis un poste dans le LAN.

![7. Réaliser une recette permettant de valider les objectifs de cette situation.](../../assets/situations/bloc3-cyber/comm-priv-pub.png)

Ensuite, nous demandons à une agence voisine de ping un serveur présent dans notre DMZ à l'adresse 192.36.3.1 depuis un poste présent dans leur VLAN Administration : 

![7. Réaliser une recette permettant de valider les objectifs de cette situation.](../../assets/situations/bloc3-cyber/ping-dmz-externe.jpg)

## Documentation technique associée

- [Cisco IOS : VLAN, SVI et routes](../../documentation/reseau/cisco.md)
- [NAT, PAT et publication de services](../../documentation/reseau/nat.md)
- [TCP, UDP, ICMP et diagnostic réseau](../../documentation/reseau/protocoles-diagnostic.md)
- [Stormshield : UTM, zones et politiques de sécurité](../../documentation/cybersecurite/stormshield.md)
- [Services Web : Apache, HTTP et publication en DMZ](../../documentation/services/services-web.md)
- [FTP, FTPS et SFTP](../../documentation/services/ftp.md)
