# Linux

Documentation générique d'administration système **Linux (Debian)**, utilisée dans le contexte CUB pour les serveurs de services (DNS, Web, DHCP, supervision).

## Connexion et prises de main

```bash
# Connexion SSH
ssh utilisateur@adresse_ip

# Passage en droit administrateur
sudo -i

# Mise à jour du système
sudo apt update
sudo apt upgrade -y
```

## Outils d'administration courants

| Outil | Rôle |
|-------|------|
| `htop` | Surveillance des processus et de la charge |
| `tcpdump` | Analyse du trafic réseau |
| `tmux` | Multiplexeur de terminaux |
| `systemctl` | Gestion des services (démarrage, arrêt, statut) |
| `journalctl` | Consultation des journaux système |

## Gestion des services

```bash
# Vérifier l'état d'un service
sudo systemctl status <service>

# Activer au démarrage
sudo systemctl enable <service>

# Voir les journaux en temps réel
sudo journalctl -u <service> -f
```

## Configuration réseau

L'interface réseau se configure dans `/etc/network/interfaces` :

```bash
auto ens18
iface ens18 inet static
    address 172.16.53.X
    netmask 255.255.255.0
    gateway 172.16.53.254
    dns-nameservers 8.8.8.8
```

## Journalisation (rsyslog)

Les journaux sont centralisés dans `/var/log` et gérés par **rsyslog** :

```bash
sudo apt install rsyslog
sudo systemctl status rsyslog.service
```

## Versionnement de la configuration (etckeeper)

**etckeeper** permet de versionner automatiquement les fichiers de `/etc` (git) :

```bash
sudo apt install etckeeper
# Reporter COMMIT_AFTER_INSTALL="yes" dans /etc/etckeeper/etckeeper.conf
sudo git log --oneline   # journal des commits
```

## Voir aussi

- [Windows](windows.md)
- [DNS](../services/dns.md)
- [Services Web](../services/services-web.md)
