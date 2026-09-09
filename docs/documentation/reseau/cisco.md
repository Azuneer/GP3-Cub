# Cisco

Documentation générique de la configuration réseau **Cisco** utilisée dans le contexte CUB.

## Équipements

| Modèle | Niveau | Rôle |
|--------|--------|------|
| Switch niveau 2 | L2 | Accès, segmentation VLAN |
| Switch niveau 3 | L3 | Cœur de réseau, routage inter-VLAN |

## VLAN

Un **VLAN** isole logiquement le trafic entre groupes de machines sur un même équipement.

```cisco
! Créer un VLAN
Switch(config)# vlan 10
Switch(config-vlan)# name Production

! Affecter un port en accès
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

! Ports en trunk
Switch(config)# interface gi0/1
Switch(config-if)# switchport mode trunk
```

## Routage inter-VLAN (switch niveau 3)

```cisco
! Activer le routage
Switch(config)# ip routing

! Configurer une interface VLAN (gateway du sous-réseau)
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.3.126 255.255.255.128
Switch(config-if)# no shutdown
```

## Routage statique

```cisco
Switch(config)# ip route 192.168.33.248 255.255.255.248 192.168.33.254
```

## Sécurisation de l'accès

```cisco
! Mot de passe enable
Switch(config)# enable secret <mot-de-passe>

! SSH (à la place de Telnet)
Switch(config)# ip domain-name local.cubX.fr
Switch(config)# crypto key generate rsa
Switch(config)# line vty 0 4
Switch(config-line)# transport input ssh
Switch(config-line)# login local
```

## Voir aussi

- [Stormshield](../cybersecurite/stormshield.md)
- [DNS](../services/dns.md)
- [Maquette Packet Tracer](../../ressources/schemas.md)
