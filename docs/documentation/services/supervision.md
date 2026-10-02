# Supervision, journaux et rsyslog

## Rôle et fonctionnement

La supervision mesure la disponibilité et les performances, puis déclenche des alertes. La journalisation enregistre les événements. Les deux se complètent : un service peut répondre tout en produisant des erreurs, et un fichier de journal peut exister sans être correctement alimenté.

`journald` collecte les événements systemd ; rsyslog peut les traiter, écrire des fichiers et transmettre des messages à un collecteur. Installer rsyslog ne configure pas automatiquement une centralisation distante.

## Contrôle local

```bash
systemctl --failed
sudo journalctl -u unbound -b --no-pager
sudo systemctl status rsyslog
sudo rsyslogd -N1
logger -t recette-cub 'Test de journalisation CUB'
```

`rsyslogd -N1` valide la configuration. Rechercher le message de test dans la destination définie par les règles ; le nom du fichier dépend de la distribution et des réglages.

## Concevoir la collecte

Définir les équipements émetteurs, le collecteur, le transport, la rétention et les accès. Prévoir la rotation et l'espace disque. Une collecte distante sur un réseau non maîtrisé nécessite une protection appropriée, par exemple TLS, et une authentification du collecteur.

Zabbix ou Nagios peuvent superviser des services, mais leur déploiement complet exige une configuration propre, éventuellement une base de données et un serveur Web. Leur seule installation ne suffit pas à créer une supervision opérationnelle.

## Recette et dépannage

| Contrôle | Attendu |
|---|---|
| Service arrêté volontairement en maquette | Alerte reçue |
| Service rétabli | Retour à l'état normal |
| Message de test | Présent sur le collecteur |
| Horodatage | Cohérent avec les autres équipements |
| Rotation | Espace disque maîtrisé |

En cas d'absence d'événements, vérifier émission, filtrage, transport, règles du collecteur et permissions de destination. Utiliser une [capture ciblée](../reseau/protocoles-diagnostic.md) pour localiser la rupture.

## Situations associées

- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)
- [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../../situations/bloc2-services/activite1-dns-resolveur.md)
- [Feuille de route des chapitres](../../situations/bloc2-reseaux/feuille-de-route.md)

## Sources officielles

- [Debian — rsyslogd](https://manpages.debian.org/bookworm/rsyslog/rsyslogd.8.en.html)
- [Debian — tcpdump](https://manpages.debian.org/bookworm/tcpdump/tcpdump.8.en.html)
