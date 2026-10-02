# Unbound : résolveur récursif, cache et DNSSEC

## Rôle et fonctionnement

Unbound fournit une résolution récursive avec cache et validation DNSSEC. Les root hints indiquent les serveurs racine ; l'ancre de confiance sert à la validation DNSSEC. Ce sont deux éléments distincts.

Dans la situation CUB, `dns0` utilise `192.168.3.10/25`. Les réseaux autorisés à interroger le résolveur doivent être définis explicitement afin d'éviter un service récursif ouvert à tous.

## Configuration minimale de maquette

Installer le paquet Debian puis ajouter un fichier dans le répertoire de configuration inclus par le paquet, après vérification des fichiers déjà présents :

```bash
sudo apt update
sudo apt install unbound dnsutils
```

```text
server:
    interface: 127.0.0.1
    interface: 192.168.3.10
    access-control: 127.0.0.0/8 allow
    access-control: 192.168.3.0/25 allow
    access-control: 192.168.3.128/26 allow
    access-control: 192.168.3.192/28 allow
    access-control: 192.168.3.208/29 allow
```

Cet extrait ne remplace pas toute la configuration du paquet. Examiner notamment les réglages DNSSEC déjà fournis pour éviter des définitions contradictoires. Unbound possède des informations racine intégrées ; un fichier root hints externe n'est pas systématiquement nécessaire.

## Vérification

```bash
sudo unbound-checkconf
sudo systemctl restart unbound
systemctl status unbound
dig @192.168.3.10 example.org
dig @192.168.3.10 example.org +tcp
sudo journalctl -u unbound -b --no-pager
```

Comparer plusieurs requêtes pour observer le cache. Tester un nom inexistant et un client situé hors des réseaux autorisés.

## Zones internes et dépannage

Une `stub-zone` dirige la résolution d'une zone vers ses serveurs faisant autorité ; une `forward-zone` s'appuie sur un autre résolveur. Choisir le mécanisme selon le rôle réel de la cible.

Pour les erreurs de journalisation, vérifier les permissions puis [AppArmor](../cybersecurite/apparmor.md). Pour `SERVFAIL`, contrôler aussi l'heure, la connectivité sortante et DNSSEC avant de modifier la politique de sécurité.

## Situations associées

- [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../../situations/bloc2-services/activite1-dns-resolveur.md)

## Sources officielles

- [NLnet Labs — manuel unbound.conf](https://www.nlnetlabs.nl/documentation/unbound/unbound.conf/)
