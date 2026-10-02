# NAT, PAT et publication de services

## Rôle et fonctionnement

La traduction NAT modifie les adresses d'un paquet ; le NAPT, souvent appelé PAT, peut aussi modifier les ports. Le routage choisit le chemin et le filtrage autorise ou refuse le flux : ces trois mécanismes sont distincts.

| Mécanisme | Modification | Usage |
|---|---|---|
| SNAT | Adresse source | Sortie d'un réseau interne |
| PAT | Source et port | Partage d'une adresse par plusieurs connexions |
| DNAT | Destination, éventuellement port | Publication d'un service interne |

La capacité d'un PAT dépend de l'implémentation et des tuples de connexion. Elle ne se résume pas à une garantie de 65 536 sessions par adresse.

## Concevoir une règle

Décrire le protocole, les zones, les adresses et les ports avant et après traduction. Limiter la règle au besoin réel : une règle trop large peut modifier des flux d'administration ou des échanges internes.

Pour publier un serveur Web, définir la destination exposée, la cible en DMZ et le port attendu. Ajouter le filtrage adapté et vérifier la passerelle du serveur. Le retour doit traverser l'équipement qui maintient l'état de traduction.

## Vérification

1. Tester le service directement depuis une zone autorisée.
2. Tester l'adresse publiée depuis un client réellement externe.
3. Examiner les compteurs de règles et les journaux.
4. Comparer les adresses observées avant et après NAT.
5. Tester un port non autorisé : il doit rester inaccessible.

## Dépannage

Un ping réussi ne prouve pas la publication de HTTP ou FTP. Un test depuis le LAN vers l'adresse externe peut nécessiter un comportement de NAT de rebouclage différent. Pour FTP, distinguer canal de commande et canaux de données.

Les adresses CUB figurent dans la [table NAT](../../ressources/table-nat.md) ; la configuration du produit est décrite dans [Stormshield](../cybersecurite/stormshield.md).

## Situations associées

- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)
- [Situation 3 : Routage et NAT](../../situations/bloc3-cyber/situation3.md)

## Sources officielles

- [IETF — NAT traditionnel, RFC 3022](https://www.rfc-editor.org/rfc/rfc3022)
