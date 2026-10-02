# Windows Admin Center, WinRM et administration distante

## Rôle et fonctionnement

Windows Admin Center (WAC) est une interface Web d'administration de Windows. Le navigateur rejoint une passerelle HTTPS ; cette passerelle dialogue avec les serveurs administrés, notamment via PowerShell et WinRM. L'accès au portail et les droits sur une machine cible sont deux contrôles différents.

Dans la situation CUB, le port HTTPS retenu pour WAC est **10443**. Ce choix de maquette ne constitue pas une valeur universelle. WinRM utilise habituellement TCP 5985 en HTTP ou 5986 en HTTPS selon le listener configuré.

## Mise en œuvre

1. Préparer une VM Windows à jour avec un nom résolu par DNS.
2. Installer WAC à partir de la distribution Microsoft.
3. Définir le port HTTPS, le certificat et le mode d'authentification.
4. Autoriser uniquement les flux nécessaires depuis le réseau d'administration.
5. Ajouter les serveurs cibles et tester avec un compte aux droits adaptés.

Le certificat doit correspondre au nom utilisé dans l'URL. Un certificat auto-signé convient à une maquette contrôlée ; une autorité de confiance facilite l'exploitation. Voir [TLS et certificats](../cybersecurite/tls.md).

## Vérification

```powershell
Test-NetConnection -ComputerName SERVEURWAC0 -Port 10443
Get-Service WinRM
winrm enumerate winrm/config/listener
Test-WSMan -ComputerName SERVEURWAC0
```

La dernière commande teste WinRM HTTP par défaut ; ajouter `-UseSSL` pour un listener HTTPS. Elle ne teste pas directement le portail WAC.

## Dépannage

| Symptôme | Contrôle |
|---|---|
| Portail inaccessible | Service WAC, DNS, port choisi et pare-feu |
| Erreur de certificat | Nom, validité et chaîne de confiance |
| Portail accessible, cible inaccessible | WinRM, droits et résolution de la cible |
| Groupe de travail | Identifiants locaux et configuration d'authentification |

Limiter `TrustedHosts` aux machines nécessaires si ce mécanisme est utilisé ; il n'accorde pas de droits sur la cible. WAC ne constitue pas automatiquement une relève haute disponibilité du bastion.

## Situations associées

- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)

## Sources officielles

- [Microsoft — présentation WAC](https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview)
- [Microsoft — installation](https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/deploy/install)
- [Microsoft — contrôle des accès](https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/configure/user-access-control)
