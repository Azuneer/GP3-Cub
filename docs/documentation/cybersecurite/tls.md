# HTTPS, TLS et certificats

## Rôle et fonctionnement

TLS protège la confidentialité et l'intégrité d'une connexion et permet d'authentifier le serveur à l'aide de son certificat. HTTPS associe HTTP à TLS. Dans CUB, ces mécanismes concernent WAC, Guacamole et les services Web.

Un certificat contient notamment une identité, une période de validité et une clé publique. La clé privée correspondante reste confidentielle. Le navigateur vérifie le nom demandé, les dates et la chaîne de confiance.

## Mise en œuvre

Choisir le nom DNS d'accès avant d'émettre le certificat. Définir les noms dans les extensions SAN, installer la chaîne nécessaire et protéger la clé privée. Un certificat utilisé via une IP doit couvrir cette IP ; couvrir uniquement un nom DNS ne suffit pas.

Un certificat auto-signé peut être employé dans une maquette avec une confiance explicitement maîtrisée. Le chiffrement ne garantit pas, à lui seul, que l'identité du serveur a été correctement vérifiée.

## Vérification

```bash
openssl s_client -connect bastion.example.org:8443 -servername bastion.example.org -verify_return_error
curl -I https://bastion.example.org:8443/guacamole/
```

Les noms `example.org` sont des exemples à remplacer par les noms réellement configurés. Contrôler l'émetteur, les dates, les SAN et le résultat de validation.

## Dépannage

| Erreur | Cause possible |
|---|---|
| Nom incompatible | URL différente des SAN |
| Autorité inconnue | Racine non approuvée ou chaîne incomplète |
| Certificat expiré | Renouvellement absent |
| Pas encore valide | Horloge incorrecte |

Une option désactivant la vérification TLS peut aider à isoler un problème, mais ne constitue pas un critère de recette réussi. Vérifier le renouvellement et les dépendances à [NTP](../services/ntp.md).

## Situations associées

- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [IETF — TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446)
- [Microsoft — certificat WAC](https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/deploy/install)
