# PowerShell : commandes, comptes et scripts

## Rôle et fonctionnement

PowerShell manipule des objets. Le pipeline transmet ces objets entre commandes, ce qui permet de filtrer des services ou des comptes puis d'afficher leurs propriétés. Les verbes `Get`, `New`, `Set` et `Remove` indiquent généralement la nature de l'action.

## Inspection avant modification

```powershell
Get-Help Get-Service -Examples
Get-Command *LocalUser*
Get-Service | Where-Object Status -eq 'Running'
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
Get-LocalUser | Select-Object Name, Enabled
```

`Select-Object` choisit les propriétés ; `Where-Object` filtre les objets. Une session élevée est nécessaire pour de nombreuses modifications système, mais pas pour toute consultation.

## Compte local d'administration

Exemple à adapter au compte attendu, après vérification de son absence :

```powershell
$CredentialPassword = Read-Host 'Mot de passe du compte' -AsSecureString
New-LocalUser -Name 'ADM-SRV' -Password $CredentialPassword
```

L'appartenance à un groupe doit découler du rôle demandé. Un utilisateur de passerelle WAC ne doit pas recevoir automatiquement tous les droits locaux du serveur.

## Scripts et mises à jour

Enregistrer les scripts en `.ps1`, relire les modifications avec Git puis exécuter une version identifiée. Utiliser `-WhatIf` lorsqu'une commande le propose. Une stratégie d'exécution PowerShell encadre le lancement des scripts ; elle ne remplace pas l'analyse de leur contenu.

Le module **PSWindowsUpdate**, utilisé dans la maquette, est un module tiers distribué via PowerShell Gallery. Avant installation, vérifier son éditeur, sa version et les dépendances ; ne pas le présenter comme une commande native de Windows.

## Vérification et dépannage

Relever la version avec `$PSVersionTable`, consulter `Get-Help` et identifier la commande ayant échoué. Conserver les sorties utiles à la recette, sans secret ni transcription de saisie confidentielle. Un script modifié doit être relu et testé avant son utilisation sur plusieurs serveurs.

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)
- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)
- [Situation 3 : Gestion des versions avec Git et Github](../../situations/bloc2-admin-sys/situation3-git.md)

## Sources officielles

- [Microsoft — documentation PowerShell](https://learn.microsoft.com/en-us/powershell/)
- [PowerShell Gallery — PSWindowsUpdate](https://www.powershellgallery.com/packages/PSWindowsUpdate)
