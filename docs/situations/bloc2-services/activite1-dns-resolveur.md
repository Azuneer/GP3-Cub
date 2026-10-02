# Activité 1 : Mise en place du service DNS résolveur (Unbound)

![Logo CUB](../../assets/logo_cub.png){ width="150" }

> :bust_in_silhouette: **Fiche rédigée par** : GADONNAUD Ewen & Rayan BOINA BOINA
> :mortar_board: **Formation** : BTS SIO 2ème année - Option SISR  
> :school: **Établissement** : Lycée Paul-Louis Courier, Tours  
> :calendar: **Date** : Septembre 2026

---
## Étape 1. Conception de l'architecture DNS de l'entreprise

### a) Schéma logique incluant le fonctionnement du service DNS

À partir du schéma logique initial, nous proposons une architecture DNS respectant les contraintes suivantes :

* Une **continuité de service** pour le **DNS récursif**, hébergé dans le **VLAN Production** (serveurs `dns0` et `dns1`).
* Une **continuité de service** pour le **DNS faisant autorité** sur le domaine `californie.cub.sioplc.fr`, hébergé dans la **DMZ** (serveurs de noms `ns0`, `ns1`).

> **Convention de nommage**
> * Serveurs **faisant autorité** sur un domaine : `ns0`, `ns1`, `ns2` (ex : `ns0.gadonnaud.eu`).
> * Serveurs **récursifs** : `dns0`, `dns1` (ex : `dns0.google.com`, `dns1.google.com`).

**Schéma (voir sur le site GP3)** : un résolveur `dns0` (192.168.3.10) et `dns1` (192.168.3.11) dans le VLAN Production, un DNS autoritaire `ns0`/`ns1` pour `californie.cub.sioplc.fr` dans la DMZ :
- Poste client (VLAN Clients) → requête récursive → `dns0` → (continuité `dns1`) → résolution récursive → serveurs racines → TLD `.fr` → autorité `ns0`/`ns1` (DMZ).

### b) Fonctionnement du service DNS, étape par étape

Prenons l'exemple d'une machine cliente du VLAN Clients qui souhaite obtenir l'adresse IP correspondant au nom `ns0.californie.cub.sioplc.fr` :

1. Le poste client envoie une **requête récursive** à son résolveur configuré, le serveur `dns0` (192.168.3.10). En cas d'indisponibilité de `dns0`, la requête est automatiquement renvoyée à `dns1` (192.168.3.11) : c'est la **continuité de service**.
2. `dns0` ne connaît pas la réponse et son cache est vide : il amorce une **résolution récursive**. Il interroge d'abord un **serveur racine** (liste fournie par le fichier `root.hints`).
3. Le serveur racine ne connaît pas le domaine mais oriente `dns0` vers les serveurs autoritaires de la **TLD `.fr`**.
4. Les serveurs de la TLD `.fr` orientent ensuite `dns0` vers les serveurs **faisant autorité** pour `californie.cub.sioplc.fr` : `ns0` et `ns1`, situés dans la DMZ.
5. `dns0` interroge `ns0` (ou `ns1` en continuité) qui possède les enregistrements de la zone et répond avec l'enregistrement `A` demandé.
6. `dns0` met la réponse en **cache** (TTL), puis la transmet au poste client. La prochaine résolution du même nom sera instantanée, sans re-parcourir l'arborescence DNS.

## Étape 2. Mise en place du service DNS résolveur dans le VLAN Production

Afin de mettre en place le service DNS résolveur `dns0` (avec **Unbound**) dans le VLAN Production, nous nous appuyons sur la documentation du contexte disponible à l'adresse <https://cubdocumentation.sioplc.fr>.

### a) Préparation du serveur

Sur le serveur `BLOC2-ExploitServ-DNSR0` présent sur Proxmox :

1. Affecter son interface réseau virtuelle au **Projet C** correspondant à notre **VLAN Production**.
2. Configurer l'interface réseau avec l'adressage suivant :

| Paramètre | Valeur |
| --- | --- |
| IP | `192.168.3.10` |
| Masque | `255.255.255.128` (/25) |
| Passerelle | `192.168.3.126` |
| DNS | `192.168.3.11` (dns1, continuité), `127.0.0.1` (le serveur lui-même) |

Vérification de l'adressage :

```bash
etudiant@dns0:~$ ip a
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    ...
    inet 192.168.3.10/25 brd 192.168.3.127 scope global ens18
    ...
```

### Installation d'Unbound

```bash
sudo apt update
sudo apt install unbound
```

### Validation de la configuration et téléchargement des « root hints »

Unbound a besoin du fichier `root.hints` listant les serveurs racines. Il n'est pas présent par défaut, ce que la vérification de configuration fait apparaître :

```bash
etudiant@dns0:~$ sudo unbound-checkconf /etc/unbound/unbound.conf
/var/lib/unbound/root.hints: No such file or directory
[1790151742] unbound-checkconf[1205:0] fatal error: file with root-hints: "/var/lib/unbound/root.hints" does not exist
```

Nous téléchargeons donc la liste officielle des serveurs racines depuis l'ICANN/Internic, puis nous en donnons la propriété à l'utilisateur `unbound` :

```bash
sudo curl --output /var/lib/unbound/root.hints https://www.internic.net/domain/named.cache
sudo chown -R unbound:unbound /var/lib/unbound/
```

### Mise en place de la journalisation

```bash
sudo touch /var/log/unbound.log
sudo chown unbound:unbound /var/log/unbound.log
```

> Note : la commande `chown` doit être exécutée avec `sudo`, sinon elle échoue (`Opération non permise`). C'est `unbound` qui écrit dans le fichier, il faut donc qu'il en soit le propriétaire.

### Redémarrage du service et constat du problème AppArmor

```bash
sudo systemctl restart unbound
sudo systemctl status unbound
```

Le service démarre, mais le journal signale une erreur d'ouverture du fichier de log :

```
● unbound.service - Unbound DNS server
     ...
   Main PID: 1233 (unbound)
     ...
sept. 23 10:23:02 dns0 unbound[1233]: [1790151782] unbound[1233:0] error: Could not open logfile /var/log/unbound.log: Permission denied
```

Même si le propriétaire du fichier est correct, **AppArmor** bloque l'écriture dans `/var/log/unbound.log`. Il faut ajouter cette écriture dans le profil de sécurité d'Unbound :

```bash
sudo vim /etc/apparmor.d/usr.sbin.unbound
sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.unbound
sudo systemctl restart apparmor
```

Nous rechargons alors le service et vérifions que le journal se remplit :

```bash
sudo systemctl restart unbound
sudo cat /var/log/unbound.log
```

### Configuration finale d'Unbound (`/etc/unbound/unbound.conf`)

Fichier de configuration de notre résolveur :

```conf
# Unbound configuration file for Debian.
#
# See the unbound.conf(5) man page.
#
# See /usr/share/doc/unbound/examples/unbound.conf for a commented
# reference config file.

# Inclusion des fichiers de configuration supplémentaires
include-toplevel: "/etc/unbound/unbound.conf.d/*.conf"

server:

    # Interface d'écoute IPv4 sur le réseau
    interface: 192.168.3.10
    interface: 127.0.0.1

    # Quels réseaux ont le droit de se servir du serveur DNS récursif
    # Ne jamais laisser son serveur récursif ouvert à tous
    #
    # allow_snoop autorise le traçage des requêtes DNS avec
    # la commande dig +trace

    access-control: 192.168.3.0/24 allow_snoop
    access-control: 192.168.33.248/29 allow_snoop
    access-control: 127.0.0.0/8 allow_snoop

    # Fichier indiquant les serveurs DNS racines
    root-hints: "/var/lib/unbound/root.hints"

    # On cache la version de Unbound
    # et on augmente la sécurité

    hide-version: yes
    hide-identity: yes
    qname-minimisation: yes

    # On autorise l'IPv4
    do-ip4: yes

    # Domaine interne
    domain-insecure: "sio.lan."
    private-domain: "sio.lan."

    # Journalisation
    logfile: "/var/log/unbound.log"
    verbosity: 1
    log-queries: yes

# Serveurs DNS faisant autorité pour sio.lan.
# Cette section doit être en dehors de "server:"

stub-zone:
    name: "sio.lan."
    stub-addr: 172.16.20.10
    stub-addr: 172.16.20.11
```

Points clés de cette configuration :

* **`interface`** : le résolveur écoute sur l'adresse du VLAN Production et en localhost.
* **`access-control`** : seuls les réseaux autorisés (VLAN Production, réseau d'administration, loopback) peuvent utiliser le résolveur. Il n'est volontairement **pas ouvert** à l'ensemble d'Internet (résolveur ouvert = risque d'amplification DDoS).
* **`root-hints`** : pointe vers le fichier des serveurs racines téléchargé précédemment.
* **`hide-version` / `hide-identity`** : durcissement (ne pas divulguer la version du serveur).
* **`qname-minimisation`** : préserve la confidentialité en minimisant le nom envoyé aux serveurs parents (RFC 7816).
* **`domain-insecure` / `private-domain`** : le domaine interne `sio.lan.` est traité comme un domaine privé, sans validation DNSSEC en amont.
* **`logfile` / `log-queries`** : journalisation des requêtes dans `/var/log/unbound.log`.
* **`stub-zone`** : le domaine interne `sio.lan.` est délégué aux serveurs faisant autorité `172.16.20.10` et `172.16.20.11`.

## Recette de validation de la situation

Nous validons le bon fonctionnement du résolveur depuis le serveur lui-même en interrogeant directement `192.168.3.10`.

### Test 1 — Résolution récursive d'un nom public

```bash
dig A google.fr @192.168.3.10
```

Ce que renvoie le serveur :

```
; <<>> DiG 9.20.26-1~deb13u1-Debian <<>> A google.fr @192.168.3.10
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32612
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;google.fr.                     IN      A

;; ANSWER SECTION:
google.fr.              299     IN      A       172.217.20.35

;; Query time: 0 msec
;; SERVER: 192.168.3.10#53(192.168.3.10) (UDP)
;; WHEN: Wed Sep 23 10:28:31 CEST 2026
;; MSG SIZE  rcvd: 54
```

**Analyse** : le statut est `NOERROR` et la zone « ANSWER » contient l'enregistrement `A` de `google.fr` (`172.217.20.35`). Le résolveur a pu, seul, parcourir toute l'arborescence DNS (racine → `.fr` → `google.fr`) et répondre : la **résolution récursive fonctionne**.

### Test 2 — Requête sur un nom inexistant (réponse négative)

```bash
dig A 8.8.8.8 @192.168.3.10
```

Ce que renvoie le serveur :

```
; <<>> DiG 9.20.26-1~deb13u1-Debian <<>> A 8.8.8.8 @192.168.3.10
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 19338
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;8.8.8.8.                       IN      A

;; AUTHORITY SECTION:
.                       3569    IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2026092300 1800 900 604800 86400

;; Query time: 64 msec
;; SERVER: 192.168.3.10#53(192.168.3.10) (UDP)
;; WHEN: Wed Sep 23 10:28:58 CEST 2026
;; MSG SIZE  rcvd: 111
```

**Analyse** : la requête porte sur l'enregistrement `A` du *nom* `8.8.8.8.`, qui n'existe pas. La réponse est **`NXDOMAIN`** (domaine inexistant) : c'est un comportement **normal et attendu**, qui prouve que le résolveur interroge bien les serveurs racines (la section « AUTHORITY » référencée par la SOA `a.root-servers.net`) et gère correctement les réponses négatives en définissant le bit `ad` (authenticité DNSSEC).

### Test 3 — Traçabilité des requêtes dans le journal

```bash
sudo cat /var/log/unbound.log
```

Ce que renvoie le journal :

```
[1790152072] unbound[1418:0] notice: init module 0: subnetcache
[1790152072] unbound[1418:0] notice: init module 1: validator
[1790152072] unbound[1418:0] notice: init module 2: iterator
[1790152072] unbound[1418:0] info: start of service (unbound 1.22.0).
[1790152106] unbound[1418:0] info: 192.168.3.10 google.fr. A IN
[1790152107] unbound[1418:0] info: generate keytag query _ta-4f66-9728. NULL IN
[1790152111] unbound[1418:0] info: 192.168.3.10 google.fr. A IN
[1790152138] unbound[1418:0] info: 192.168.3.10 8.8.8.8. A IN
```

**Analyse** : les requêtes issues de `192.168.3.10` (`google.fr. A`, `8.8.8.8. A`) sont bien journalisées. La journalisation est opérationnelle, ce qui permet une supervision et une analyse des requêtes reçues par le résolveur.

### Conclusion de la recette

| Test | Résultat attendu | Résultat constaté | Statut |
| --- | --- | --- | --- |
| Résolution récursive `google.fr` | `NOERROR` + enregistrement `A` | `172.217.20.35` | :white_check_mark: |
| Requête sur un nom inexistant | `NXDOMAIN` | `NXDOMAIN` + SOA racine | :white_check_mark: |
| Journalisation des requêtes | Entrées dans `/var/log/unbound.log` | `google.fr. A IN` / `8.8.8.8. A IN` | :white_check_mark: |

Le serveur DNS résolveur `dns0` (192.168.3.10) est **opérationnel** : il assure la résolution récursive des noms publics pour le VLAN Production, avec une continuité prévue par le second résolveur `dns1` (192.168.3.11) configuré dans la table DNS des clients.

## Documentation technique associée

- [Debian : réseau, paquets et services systemd](../../documentation/adminsys/linux.md)
- [TCP, UDP, ICMP et diagnostic réseau](../../documentation/reseau/protocoles-diagnostic.md)
- [AppArmor : confinement et diagnostic](../../documentation/cybersecurite/apparmor.md)
- [DNS : résolution, autorité et enregistrements](../../documentation/services/dns.md)
- [Unbound : résolveur récursif, cache et DNSSEC](../../documentation/services/unbound.md)
- [Supervision, journaux et rsyslog](../../documentation/services/supervision.md)
