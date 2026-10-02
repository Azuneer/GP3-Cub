# VPN : accès distant et interconnexion de sites

## Rôle et fonctionnement

Un VPN établit un tunnel protégé entre deux points à travers un réseau intermédiaire. Le mode site à site relie des réseaux ; le mode accès distant raccorde un poste utilisateur à un périmètre défini. Le sujet est annoncé dans la feuille de route CUB ; aucune recette de tunnel opérationnel n'est encore fournie.

IPsec protège les communications IP ; IKE négocie les paramètres et authentifie les pairs. D'autres solutions transportent le tunnel au-dessus de TLS. Le choix doit correspondre aux équipements et au besoin d'accès.

## Préparer un tunnel

Documenter les pairs, les réseaux locaux et distants, la méthode d'authentification, les propositions cryptographiques et les routes. Vérifier l'absence de chevauchement entre les réseaux à relier. Définir les flux autorisés dans le tunnel et l'éventuelle exemption NAT selon la solution.

Un tunnel établi ne signifie pas que tous les sous-réseaux sont accessibles : routes, sélecteurs et filtrage doivent être cohérents aux deux extrémités.

## Recette

| Contrôle | Attendu |
|---|---|
| Authentification des pairs | Identités connues et validées |
| Établissement | Associations actives des deux côtés |
| Flux utile | Service prévu accessible |
| Flux interdit | Accès refusé et journalisé |
| Reconnexion | Rétablissement mesuré après coupure |

## Dépannage

Distinguer négociation impossible, tunnel établi sans trafic et trafic à sens unique. Vérifier les propositions, les routes retour et le NAT avant de modifier le chiffrement. Le VPN ne remplace pas un bastion : le premier transporte les flux, le second encadre les sessions d'administration.

## Situations associées

- [Feuille de route des chapitres](../../situations/bloc2-reseaux/feuille-de-route.md)

## Sources officielles

- [strongSwan — principes IPsec/IKE](https://docs.strongswan.org/docs/latest/howtos/introduction.html)
