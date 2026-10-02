# Stormshield : UTM, zones et politiques de sécurité

## Rôle et fonctionnement

Un pare-feu stateful suit l'état des connexions. Une solution UTM regroupe plusieurs fonctions, comme le filtrage réseau, l'inspection applicative et la prévention d'intrusions. Les fonctions effectivement disponibles dépendent du modèle, de la version et des licences.

Dans CUB, Stormshield sépare les zones réseau et réalise des traductions d'adresses. Le choix d'un produit français ne suffit pas à démontrer une conformité générale : une certification ou qualification porte sur un périmètre et une version précis.

## Mise en œuvre

1. Identifier les interfaces WAN, LAN et DMZ à partir du schéma.
2. Configurer leur adressage et les routes nécessaires.
3. Créer des objets nommés pour les réseaux, machines et services.
4. Définir les flux autorisés, puis les règles NAT utiles.
5. Activer la politique prévue, vérifier les journaux et sauvegarder la configuration.

Une règle doit préciser sa source, sa destination, le service et son objectif. L'ordre des règles compte. Limiter les droits d'administration à la zone dédiée.

## Filtrage et NAT

La traduction ne vaut pas autorisation. Une publication en DMZ nécessite un service actif, une règle NAT, le filtrage adapté et un chemin retour cohérent. Restreindre une règle de sortie à son périmètre évite de traduire involontairement des échanges internes.

La politique **Pass All** utilisée pendant la maquette facilite un diagnostic initial ; elle ne représente pas la politique finale de sécurité.

## Vérification et dépannage

Tester un flux autorisé et un flux interdit depuis chaque zone pertinente. Examiner la politique active, les compteurs, les traces et les routes. Comparer les adresses avant et après traduction plutôt que d'attribuer systématiquement un blocage à l'IPS.

Les affectations réelles figurent dans les [tables de routage](../../ressources/tables-de-routage.md) et la [table NAT](../../ressources/table-nat.md).

## Situations associées

- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)
- [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../../situations/bloc3-cyber/situation2.md)
- [Situation 3 : Routage et NAT](../../situations/bloc3-cyber/situation3.md)

## Sources officielles

- [Stormshield — configuration du filtrage et du NAT](https://documentation.stormshield.eu/SNS/v4/en/Content/Installation_and_first_time_configuration/Securitypolicy_filteringnat.htm)
