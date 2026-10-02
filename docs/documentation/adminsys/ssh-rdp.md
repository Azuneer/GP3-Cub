# SSH, SCP et RDP

## Rôle et fonctionnement

| Protocole | Usage | Port habituel |
|---|---|---|
| SSH | Administration en ligne de commande chiffrée | TCP 22 |
| SCP / SFTP | Transfert de fichiers via SSH | TCP 22 |
| RDP | Bureau distant Windows | TCP 3389, avec UDP possible |

Dans CUB, les sessions d'administration doivent suivre le chemin prévu par le [bastion](../cybersecurite/guacamole.md). L'ouverture d'un port ne remplace ni l'authentification ni le contrôle des droits.

## Connexion et transfert

```bash
ssh etudiant@192.168.3.209
scp deploiement_tomcat9.sh etudiant@192.168.3.209:/tmp/
```

Après transfert, le fichier se trouve dans `/tmp/`. Son lancement doit donc préciser ce chemin ou être précédé d'un changement de répertoire. Vérifier sa provenance et son contenu avant exécution. Les versions récentes d'OpenSSH utilisent SFTP pour `scp` par défaut.

## OpenSSH sur Windows

```powershell
Get-Service sshd
Start-Service sshd
Set-Service sshd -StartupType Automatic
Get-NetFirewallRule -Name OpenSSH-Server-In-TCP
```

Contrôler d'abord la présence du composant. Pour RDP, vérifier le service, les utilisateurs autorisés et l'authentification au niveau réseau ; éviter l'exposition directe depuis Internet.

## Vérification et dépannage

```bash
sudo sshd -t
ssh -v etudiant@192.168.3.209
```

`sshd -t` valide la configuration côté serveur. Une erreur de clé d'hôte après réinstallation doit conduire à vérifier l'empreinte par un autre canal avant d'actualiser `known_hosts`.

Distinguer un délai dépassé (chemin réseau ou filtrage), un refus de connexion (service absent) et un refus d'authentification (compte, clé ou méthode). Pour un accès renforcé, consulter [TOTP et gestion des secrets](../cybersecurite/authentification.md).

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)
- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)

## Sources officielles

- [OpenSSH — scp](https://man.openbsd.org/scp)
- [OpenSSH — configuration serveur](https://man.openbsd.org/sshd_config)
- [Microsoft — OpenSSH Windows](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
