# Cisco IOS : VLAN, SVI et routes

## Rôle et prérequis

Les commutateurs Cisco assurent la commutation et, sur les modèles L3, le routage. Les noms d'interfaces et fonctions disponibles dépendent du modèle et de l'image IOS. Relever les ports réels sur le [plan de câblage](../../ressources/schemas.md) avant toute configuration.

## Exemple : VLAN Production 53

Sur un switch L3 compatible, exemple à adapter à un port d'accès libre et à la liaison montante réellement utilisée :

```text
configure terminal
vlan 53
 name PRODUCTION
interface GigabitEthernet1/0/1
 switchport mode access
 switchport access vlan 53
interface GigabitEthernet1/0/48
 switchport mode trunk
 switchport trunk allowed vlan add 10,20,51,53
interface Vlan53
 ip address 192.168.3.126 255.255.255.128
 no shutdown
ip routing
end
```

Cette séquence ne configure pas les SVI des autres VLAN. L'état opérationnel d'une SVI dépend notamment de la présence du VLAN et de ports actifs associés.

## Route par défaut

Si la liaison LAN2 est correctement configurée et si `192.168.33.254` est joignable, le prochain saut vers Stormshield peut être défini ainsi :

```text
configure terminal
ip route 0.0.0.0 0.0.0.0 192.168.33.254
end
```

Il est inutile d'ajouter une route statique vers un réseau déjà directement connecté. Vérifier aussi les routes de retour sur le pare-feu.

## Vérification et sauvegarde

```text
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show access-lists
copy running-config startup-config
```

Sauvegarder seulement après validation. Documenter le port modifié, le résultat attendu et le résultat observé. Pour l'administration, privilégier SSH, un compte nominatif et un accès limité au réseau autorisé ; Telnet transmet les échanges sans chiffrement.

## Situations associées

- [Activité 0 : Mise en place de l'infrastructure réseau des agences de l'entreprise CUB](../../situations/bloc2-reseaux/activite0.md)
- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)
- [Situation 3 : Routage et NAT](../../situations/bloc3-cyber/situation3.md)

## Sources officielles

- [Cisco — configuration du routage inter-VLAN](https://www.cisco.com/c/en/us/support/docs/lan-switching/inter-vlan-routing/41260-189.html)
