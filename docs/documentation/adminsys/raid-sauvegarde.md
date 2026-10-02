# RAID, sauvegardes et continuité

## Rôle et fonctionnement

Le RAID combine plusieurs disques pour répartir les données et, selon le niveau, tolérer des pannes. Il est annoncé dans la feuille de route réseau de CUB ; cette page en présente les bases sans supposer un déploiement déjà réalisé.

| Niveau | Minimum de disques | Capacité indicative, disques identiques | Tolérance |
|---|---:|---|---|
| RAID 0 | 2 | N × capacité d'un disque | Aucune |
| RAID 1 à deux disques | 2 | Capacité d'un disque | Un disque |
| RAID 5 | 3 | (N − 1) × capacité d'un disque | Un disque |
| RAID 6 | 4 | (N − 2) × capacité d'un disque | Deux disques |
| RAID 10 classique | 4 | N / 2 × capacité d'un disque | Dépend des miroirs touchés |

## Mise en œuvre et exploitation

Choisir le niveau à partir de la capacité, de la charge et de la disponibilité attendues. Documenter les disques, les alertes et la procédure de remplacement. La reconstruction sollicite les disques restants et peut durer longtemps.

Un RAID ne protège pas contre une suppression logique, un chiffrement malveillant ou la perte de tout le stockage. Prévoir des sauvegardes séparées et des tests de restauration. Un instantané de VM reste dépendant de son support.

## Vérification

Sur une machine utilisant Linux MD :

```bash
cat /proc/mdstat
sudo mdadm --detail /dev/md0
```

Ces commandes ne concernent pas tous les stockages Proxmox : ZFS ou un contrôleur matériel disposent d'autres outils.

## Recette de continuité

Définir le RPO (perte de données acceptable) et le RTO (durée de rétablissement attendue). Mesurer une restauration réelle et noter ses dépendances. Pour le bastion, prévoir un accès de secours contrôlé : la simple présence de WAC n'établit pas une continuité complète.

## Situations associées

- [Feuille de route des chapitres](../../situations/bloc2-reseaux/feuille-de-route.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [Linux — tableaux RAID MD](https://kernel.org/doc/html/v6.7/admin-guide/md.html)
- [Proxmox — guide d’administration](https://pve.proxmox.com/pve-docs/pve-admin-guide.pdf)
