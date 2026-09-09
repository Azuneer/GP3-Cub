# DNS

Documentation générique de configuration et de gestion des services **DNS** (Domain Name System), utilisée dans le contexte CUB pour la résolution des noms de domaine des agences.

## Principe

Le DNS traduit des **noms de domaine** en **adresses IP**. Une zone DNS décrit un domaine et ses enregistrements.

## Types d'enregistrements

| Type | Rôle | Exemple |
|------|------|---------|
| `A` | Adresse IPv4 d'un hôte | `www.cub.fr. A 192.36.253.30` |
| `AAAA` | Adresse IPv6 | `www.cub.fr. AAAA fe80::1` |
| `CNAME` | Alias d'un hôte | `doc CNAME www` |
| `MX` | Serveur de messagerie | `cub.fr. MX 10 mail` |
| `NS` | Serveur de noms | `cub.fr. NS ns1.cub.fr.` |
| `PTR` | Résolution inverse | `30.253.36.192 PTR www` |

## Mise en place sur Debian (BIND)

### Installation

```bash
sudo apt update
sudo apt install bind9
```

### Configuration d'une zone

Fichier `/etc/bind/named.conf.local` :

```bind
zone "cubX.fr" {
    type master;
    file "/etc/bind/db.cubX.fr";
};
```

Fichier de zone `/etc/bind/db.cubX.fr` :

```bind
$TTL 604800
@   IN  SOA ns1.cubX.fr. admin.cubX.fr. (
        2026090901 ; Serial
        604800     ; Refresh
        86400      ; Retry
        2419200    ; Expire
        604800 )   ; Negative Cache TTL
;
    IN  NS  ns1.cubX.fr.
ns1 IN  A   <ip_dns>
www IN  A   <ip_web>
```

### Activer et vérifier

```bash
sudo systemctl restart bind9
sudo named-checkzone cubX.fr /etc/bind/db.cubX.fr
nslookup www.cubX.fr
dig www.cubX.fr
```

## Réseau serveurs Debian (DMZ)

Dans l'architecture CUB, les serveurs **DNS maîtres et esclaves** sont déployés en **DMZ** pour assurer la résolution du domaine de chaque agence.

## Voir aussi

- [Linux](../adminsys/linux.md)
- [Services Web](services-web.md)
- [Cisco](../reseau/cisco.md)
