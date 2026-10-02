# Debian : réseau, paquets et services systemd

## Rôle et fonctionnement

Debian héberge les services Linux de CUB. APT gère les paquets ; systemd supervise les services ; les commandes `ip` et `ss` permettent de contrôler le réseau. La configuration persistante dépend du gestionnaire actif : ifupdown, NetworkManager ou systemd-networkd.

## État initial

```bash
cat /etc/os-release
ip -br address
ip route
cat /etc/resolv.conf
systemctl --failed
ss -lntup
```

Identifier l'interface réelle avant modification. Un nom comme `ens18` dépend du matériel virtuel et n'est pas garanti.

## Configuration avec ifupdown

Exemple pour **dns0**, `192.168.3.10/25`, dans le VLAN Production 53. Cette configuration concerne une machine utilisant `/etc/network/interfaces` :

```text
auto ens18
iface ens18 inet static
    address 192.168.3.10/25
    gateway 192.168.3.126
```

La configuration DNS doit être faite dans le mécanisme actif de la machine. La directive `dns-nameservers` dépend d'une intégration telle que resolvconf ; elle n'est pas une garantie de mise à jour du résolveur. Ne pas faire gérer la même interface par plusieurs outils.

## Paquets et services

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
systemctl status unbound
sudo journalctl -u unbound -b --no-pager
```

`apt update` actualise l'index ; `apt upgrade` installe les mises à jour proposées. Prévoir les conséquences d'un redémarrage de service.

## Vérification et dépannage

Après une modification réseau, tester la passerelle, la résolution DNS et le service depuis un autre VLAN autorisé. Une modification à distance peut interrompre SSH : conserver la console de la VM disponible.

`htop` aide à examiner la charge ; `tmux` conserve un terminal de travail ; `journalctl` donne les événements. La persistance et la centralisation des journaux relèvent de la [supervision](../services/supervision.md).

## Situations associées

- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)
- [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../../situations/bloc2-services/activite1-dns-resolveur.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [Debian — configuration réseau](https://www.debian.org/doc/manuals/debian-reference/ch05.en.html)
- [Debian — référence](https://www.debian.org/doc/manuals/debian-reference/)
