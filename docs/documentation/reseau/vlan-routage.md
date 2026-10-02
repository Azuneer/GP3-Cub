# VLAN, trunks et routage inter-VLAN

## Rôle et fonctionnement

Un VLAN sépare les domaines de diffusion de niveau 2. Un port access raccorde généralement un équipement à un VLAN ; un trunk transporte plusieurs VLAN, habituellement étiquetés en 802.1Q. La cohérence doit être vérifiée entre les deux extrémités et les bridges des hyperviseurs.

Le passage entre deux sous-réseaux nécessite un équipement de niveau 3. Une SVI est l'interface logique du switch associée à un VLAN ; elle peut servir de passerelle aux machines de ce VLAN.

## Organisation CUB

| VLAN | Rôle |
|---|---|
| 53 | Production |
| 10 | Clients |
| 20 | Administration |
| 51 | Bastion |

Les IP et masques figurent dans le [plan de référence](../../situations/bloc3-cyber/situation1.md). Les identifiants de VLAN ne déterminent pas automatiquement les adresses IP.

## Routes et filtrage

Une route directement connectée apparaît lorsque l'interface correspondante est opérationnelle. Une route statique précise un prochain saut. La route par défaut `0.0.0.0/0` s'utilise lorsqu'aucune route plus spécifique ne correspond.

Si le switch L3 assure le routage entre deux VLAN, leurs échanges ne traversent pas nécessairement le pare-feu Stormshield. Le filtrage doit être appliqué sur le chemin réel, via ACL ou architecture adaptée. La seule création d'un VLAN Bastion ne bloque pas les accès directs.

## Vérification et dépannage

Contrôler successivement : appartenance du port, VLAN autorisés sur le trunk, état des SVI, table de routage, filtrage et chemin retour. Une passerelle configurée dans la table documentaire ne garantit pas que l'interface est active.

Les commandes et un exemple de configuration sont disponibles dans la [fiche Cisco](cisco.md).

## Situations associées

- [Activité 0 : Mise en place de l'infrastructure réseau des agences de l'entreprise CUB](../../situations/bloc2-reseaux/activite0.md)
- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [Cisco — routage inter-VLAN](https://www.cisco.com/c/en/us/support/docs/lan-switching/inter-vlan-routing/41260-189.html)
