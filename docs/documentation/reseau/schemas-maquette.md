# Draw.io, plans de câblage et Packet Tracer

## Rôle des documents

| Document | Informations principales |
|---|---|
| Schéma logique | VLAN, sous-réseaux, passerelles, services et zones |
| Schéma physique | Équipements, interfaces et liaisons |
| Plan de câblage | Correspondance précise des ports et raccordements |
| Maquette Packet Tracer | Configuration et simulation d'une partie du réseau |

Ces supports se complètent. Une liaison dessinée ne démontre ni l'existence d'une route ni l'autorisation d'un flux.

## Mise à jour d'une architecture

1. Modifier le fichier Draw.io source du projet concerné.
2. Répercuter la modification dans les trois représentations utiles.
3. Actualiser les tables d'adressage, de routage ou de NAT concernées.
4. Exporter les PDF après enregistrement des sources.
5. Mettre à jour la maquette lorsqu'elle simule ces équipements.
6. Vérifier que les liens publiés pointent vers les nouvelles versions.

Dans CUB, l'ajout du VLAN 51 doit être cohérent avec le sous-réseau Bastion, les interfaces L3 et le filtrage d'administration. Éviter de reprendre un plan Ecocert simplement parce qu'il se trouve dans un dossier voisin.

## Vérification et limites

Comparer chaque liaison aux ports configurés. Contrôler les libellés, les masques et les passerelles, puis ouvrir le PDF exporté pour vérifier qu'aucune page n'est tronquée. Draw.io permet notamment d'exporter le diagramme au format PDF.

Packet Tracer simule un ensemble de fonctions réseau ; il ne remplace pas la recette sur les VM, Stormshield ou Guacamole. Les services absents du simulateur doivent être vérifiés dans la maquette réelle.

Les fichiers de référence sont regroupés dans [Schémas et documents](../../ressources/schemas.md).

## Situations associées

- [Activité 0 : Mise en place de l'infrastructure réseau des agences de l'entreprise CUB](../../situations/bloc2-reseaux/activite0.md)
- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)

## Sources officielles

- [Draw.io — export PDF](https://www.drawio.com/docs/manual/export/export-to-pdf/)
- [Cisco — Packet Tracer](https://www.netacad.com/cisco-packet-tracer)
