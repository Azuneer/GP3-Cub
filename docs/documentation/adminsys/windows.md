# Windows Server : préparation, SConfig et Sysprep

## Rôle et fonctionnement

Windows Server héberge les rôles d'infrastructure. Server Core privilégie une administration sans bureau complet ; Desktop Experience fournit une interface graphique. Dans CUB, la préparation de la VM précède la configuration des rôles et de Windows Admin Center.

SConfig rassemble les réglages initiaux : nom, réseau, mises à jour et administration distante. Sysprep prépare une installation destinée à être réutilisée en supprimant des informations propres à la machine.

## Préparation

1. Installer le système et les pilotes [VirtIO](virtualisation.md).
2. Choisir un nom unique et une IP conforme au plan d'adressage de la situation.
3. Configurer le DNS et la [synchronisation horaire](../services/ntp.md).
4. Installer les mises à jour, puis vérifier le redémarrage.
5. Créer les comptes d'administration et vérifier les protections locales.

```powershell
SConfig
Get-NetAdapter
Get-NetIPConfiguration
Get-DnsClientServerAddress
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion
```

## Préparer un modèle avec Sysprep

Sur une VM de référence prévue pour être clonée :

```powershell
C:\Windows\System32\Sysprep\Sysprep.exe /generalize /oobe /shutdown
```

Créer le modèle après l'arrêt, puis personnaliser chaque clone. Cette procédure ne constitue pas une méthode de clonage d'un contrôleur de domaine déjà en service.

## Sécurité et vérification

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled
Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System' -Name EnableLUA
Get-LocalUser | Select-Object Name, Enabled, SID
```

Le renommage du compte intégré ne modifie pas son SID et ne suffit pas à le sécuriser. Conserver l'UAC, limiter les administrateurs et contrôler les accès distants. Un changement de réseau doit être vérifié depuis la console avant de fermer la session.

## Dépannage

Une interface absente oriente vers les pilotes ; une interface présente sans connectivité vers le VLAN ou l'adressage. Un échec Sysprep demande l'examen des journaux de préparation, sans répéter les clones à partir d'une image incomplète.

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)
- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)

## Sources officielles

- [Microsoft — SConfig](https://learn.microsoft.com/en-us/windows-server/windows-server-2022/get-started/sconfig-on-ws2022)
- [Microsoft — Sysprep](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)
