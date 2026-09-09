# Supervision

Documentation générique de la **supervision** des systèmes et services, utilisée dans le contexte CUB pour surveiller l'infrastructure (réseau, serveurs, disponibilité des services).

## Principe

La supervision consiste à **collecter**, **analyser** et **alerter** sur l'état des équipements et services afin de détecter les dysfonctionnements au plus tôt.

## Outils courants

| Type d'outil | Exemple | Rôle |
|--------------|---------|------|
| Supervision réseau | Zabbix, Nagios | Surveillance globale des équipements |
| Analyse de trafic | tcpdump, Wireshark | Capture et analyse des paquets |
| Journalisation | rsyslog | Centralisation des logs |

## Supervision réseau (Zabbix)

### Installation serveur (Debian)

```bash
sudo apt install zabbix-server-mysql zabbix-frontend-php
sudo systemctl enable zabbix-server
sudo systemctl start zabbix-server
```

### Ajout d'un agent

```bash
sudo apt install zabbix-agent
sudo systemctl enable zabbix-agent
sudo systemctl start zabbix-agent
```

## Analyse de trafic avec tcpdump

```bash
# Capturer les paquets d'une interface
sudo tcpdump -i ens18

# Filtrer par port
sudo tcpdump -i ens18 port 53

# Filtrer par hôte
sudo tcpdump -i ens18 host 192.168.3.126
```

## Centralisation des journaux (rsyslog)

Les logs sont centralisés dans `/var/log` et gérés par **rsyslog** :

```bash
sudo systemctl status rsyslog.service
ls -l /var/log
```

## Bonnes pratiques

- Superviser la **disponibilité** et les **performances** des services critiques
- Configurer des **alertes** (Seuils critiques)
- **Centraliser** les journaux pour faciliter l'investigation
- Vérifier régulièrement l'intégrité des sauvegardes

## Voir aussi

- [Linux](../adminsys/linux.md)
- [Services Web](services-web.md)
