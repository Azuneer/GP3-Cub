# NTP : synchronisation horaire et fuseaux

## Rôle et fonctionnement

Une heure cohérente permet de rapprocher les journaux, valider les certificats et utiliser les mécanismes d'authentification sensibles au temps. NTP synchronise l'horloge ; le fuseau détermine son affichage local. Régler Europe/Paris ne prouve pas que la synchronisation fonctionne.

## Windows : contrôler avant de modifier

```powershell
w32tm /query /status
w32tm /query /source
w32tm /query /configuration
```

Pour un serveur autonome, un exemple de sources explicites est :

```powershell
w32tm /config /manualpeerlist:"0.fr.pool.ntp.org,0x8 1.fr.pool.ntp.org,0x8" /syncfromflags:manual /update
Restart-Service w32time
w32tm /resync
```

Dans un domaine Active Directory, respecter la hiérarchie de temps du domaine. Ne pas imposer les mêmes sources manuelles à tous les membres sans tenir compte du rôle PDC Emulator et de la stratégie retenue.

## Debian et pare-feu

```bash
timedatectl status
timedatectl show -p NTPSynchronized
```

Identifier ensuite le service utilisé : chrony, systemd-timesyncd ou autre. Sur Stormshield, vérifier les serveurs configurés, la zone horaire et les événements de synchronisation. Autoriser les échanges NTP nécessaires selon le réseau.

## Recette et dépannage

Relever la source, la dernière synchronisation et l'écart observé. Comparer un même événement sur deux équipements. En cas d'échec, examiner UDP 123, la résolution des sources et la disponibilité du serveur de temps.

Une horloge correcte à un instant donné peut dériver ensuite : intégrer la synchronisation à la supervision. Les exemples de sources sont ceux de la maquette, à adapter à la politique de l'entreprise.

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)
- [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../../situations/bloc2-admin-sys/situation2-wac.md)
- [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../../situations/bloc3-cyber/situation2.md)

## Sources officielles

- [Microsoft — outils et réglages Windows Time](https://learn.microsoft.com/en-us/windows-server/networking/windows-time-service/windows-time-service-tools-and-settings)
