# Virtualisation : Proxmox, VirtualBox et VirtIO

## Rôle et fonctionnement

La virtualisation permet d'exécuter plusieurs systèmes invités sur un hôte physique. Chaque VM dispose de processeurs virtuels, de mémoire, de disques et de cartes réseau. Proxmox VE héberge les VM de la maquette CUB ; VirtualBox apparaît dans les exercices de cybersécurité.

Un bridge relie une carte virtuelle à un réseau. Un VLAN précise le segment transporté. Le mode NAT d'un hyperviseur ajoute une traduction d'adresses : il ne remplace pas un raccordement direct au VLAN attendu par la maquette.

## Préparation d'une VM

1. Relever l'OS, le rôle, le VLAN et les ressources nécessaires.
2. Créer le disque et raccorder la carte réseau au bridge prévu.
3. Monter l'ISO du système. Pour Windows avec matériel VirtIO, prévoir aussi l'ISO des pilotes.
4. Installer les pilotes correspondant au périphérique et à la version du système.
5. Vérifier l'adresse MAC, l'IP, le masque et la passerelle avant de dupliquer la machine.

Les pilotes VirtIO permettent au système invité d'utiliser les périphériques paravirtualisés de Proxmox. Une carte absente dans Windows peut donc provenir d'un pilote manquant, sans panne du réseau physique.

## Clone, modèle et instantané

| Élément | Usage | Limite |
|---|---|---|
| Modèle | Base de déploiement reproductible | À préparer et maintenir |
| Clone | Nouvelle VM issue d'une base | Identité et adressage à personnaliser |
| Instantané | Retour à un état antérieur | Dépend du stockage et ne remplace pas une sauvegarde |
| Sauvegarde | Restauration après perte de VM ou de stockage | Restauration à tester |

## Vérification et dépannage

Contrôler le démarrage, les pilotes, le lien virtuel puis la connectivité à la passerelle. Après clonage, rechercher les doublons d'IP et de nom. Pour Windows, préparer le modèle avec [Sysprep](windows.md), avant l'intégration à l'annuaire.

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)
- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)
- [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../../situations/bloc3-cyber/situation2.md)

## Sources officielles

- [Proxmox — migration et pilotes VirtIO](https://pve.proxmox.com/wiki/Migrate_to_Proxmox_VE)
- [Proxmox — guide d’administration](https://pve.proxmox.com/pve-docs/pve-admin-guide.pdf)
