# DNS : résolution, autorité et enregistrements

## Rôle et fonctionnement

Un client interroge un résolveur pour obtenir les données associées à un nom. Un serveur faisant autorité publie les données de ses zones. Un résolveur récursif recherche la réponse et peut la conserver en cache pendant sa durée de validité.

Dans CUB, `dns0` et `dns1` désignent les résolveurs ; `ns0`, `ns1` et `ns2` désignent les serveurs faisant autorité selon la convention de la fiche. Ces rôles doivent être distingués même lorsqu'un logiciel sait remplir plusieurs fonctions.

## Enregistrements

| Type | Usage |
|---|---|
| A / AAAA | Adresse IPv4 / IPv6 |
| CNAME | Alias vers un autre nom |
| NS | Serveur faisant autorité pour une zone |
| MX | Serveur de messagerie et priorité |
| PTR | Résolution inverse |
| SOA | Paramètres de la zone, numéro de série |
| SRV | Localisation d'un service, notamment pour AD |

## Outils et mise en œuvre

BIND peut publier des zones faisant autorité. Unbound est utilisé dans la situation pour la résolution récursive. Une zone interne peut être orientée vers les serveurs appropriés sans envoyer toutes les requêtes publiques vers eux.

```bash
dig @192.168.3.10 example.org A
dig @192.168.3.10 example.org AAAA
dig @192.168.3.10 example.org +tcp
dig @192.168.3.10 exemple-inexistant.invalid
```

## Vérification et dépannage

`NOERROR` n'implique pas nécessairement une réponse contenant le type demandé. `NXDOMAIN` signifie que le nom n'existe pas ; `SERVFAIL` indique un échec de traitement ; `REFUSED` indique un refus.

Comparer le serveur interrogé, le temps de réponse et le TTL. Tester UDP et TCP 53. Une panne DNSSEC peut provenir de l'heure ou de la chaîne de validation : désactiver la validation sans diagnostic ne résout pas sa cause.

La procédure de résolution utilisée par le projet figure dans [Unbound](unbound.md).

## Situations associées

- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)
- [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../../situations/bloc2-services/activite1-dns-resolveur.md)

## Sources officielles

- [IETF — concepts DNS](https://www.rfc-editor.org/rfc/rfc1034)
- [NLnet Labs — configuration Unbound](https://www.nlnetlabs.nl/documentation/unbound/unbound.conf/)
