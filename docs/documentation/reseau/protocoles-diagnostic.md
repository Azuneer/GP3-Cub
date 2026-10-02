# TCP, UDP, ICMP et diagnostic réseau

## Rôle et fonctionnement

TCP fournit un flux d'octets fiable et ordonné avec établissement de connexion. UDP transporte des datagrammes sans garantir leur livraison ni leur ordre. ICMP sert notamment aux messages de diagnostic ; il ne possède pas de ports TCP ou UDP.

| Service | Transport habituel |
|---|---|
| DNS | UDP et TCP 53 |
| SSH | TCP 22 |
| HTTP / HTTPS | TCP 80 / 443 dans les exemples CUB |
| NTP | UDP 123 |
| RDP | TCP 3389, UDP possible |
| WAC de la maquette | TCP 10443 |

## Méthode de diagnostic

Procéder du plus proche au plus éloigné : interface locale, VLAN, passerelle, route, filtrage, service, puis DNS. Tester par IP aide à isoler un problème de résolution de noms.

```bash
ip -br address
ip route
ping -c 4 192.168.3.126
ss -lntup
dig @192.168.3.10 example.org
curl -I http://192.168.3.10
```

La dernière commande n'est pertinente que si un serveur HTTP est attendu sur cette adresse. Pour tester un port précis depuis Windows :

```powershell
Test-NetConnection -ComputerName 192.168.3.209 -Port 8443
tracert 192.168.3.209
```

## Capture ciblée

```bash
sudo tcpdump -ni ens18 -c 30 'host 192.168.3.10 and port 53'
```

Une capture contient potentiellement des données sensibles ; choisir un filtre et une durée adaptés. Observer requête et réponse sur le bon lien avant de conclure.

## Interpréter les résultats

Un ping bloqué ne prouve pas que le service est arrêté. Un port TCP accessible ne prouve pas que l'authentification fonctionne. Une recette doit préciser source, destination, protocole, résultat attendu et observation, avec un test autorisé et un test de refus.

## Situations associées

- [Feuille de route des chapitres](../../situations/bloc2-reseaux/feuille-de-route.md)
- [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../../situations/bloc3-cyber/situation2.md)
- [Situation 3 : Routage et NAT](../../situations/bloc3-cyber/situation3.md)
- [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../../situations/bloc2-services/activite1-dns-resolveur.md)

## Sources officielles

- [IETF — TCP](https://www.rfc-editor.org/rfc/rfc9293)
- [IETF — UDP](https://www.rfc-editor.org/rfc/rfc768)
- [Debian — tcpdump](https://manpages.debian.org/bookworm/tcpdump/tcpdump.8.en.html)
