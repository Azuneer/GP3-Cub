# Windows

Documentation générique d'administration système **Windows Server**, utilisée dans le contexte CUB pour les services Active Directory et DNS.

## Installation d'un serveur Windows Server

L'installation s'effectue sur une machine virtuelle (ferme Proxmox) :

1. Créer la VM avec les ressources adaptées
2. Monter l'ISO Windows Server
3. Démarrer et suivre l'assistant d'installation
4. Définir une adresse IP fixe

!!! note "Contexte CUB"
    Les contrôleurs de domaine sont installés sur la ferme de serveurs Proxmox et administrés à distance (RDP) via un bastion.

## Configuration réseau (IP fixe)

```powershell
# Liste des interfaces
Get-NetIPAddress

# Attribuer une IP statique
New-NetIPAddress -InterfaceAlias "Ethernet0" `
  -IPAddress 172.16.X.X -PrefixLength 24 -DefaultGateway 172.16.X.254

# Configurer le DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" `
  -ServerAddresses 172.16.X.1
```

## Promotion en contrôleur de domaine (AD + DNS)

Installation du rôle Active Directory et du DNS :

```powershell
# Installer les rôles
Install-WindowsFeature AD-Domain-Services, DNS

# Promouvoir en contrôleur de domaine
Install-ADDSForest `
  -DomainName "local.cubX.fr" `
  -InstallDns:$true `
  -SafeModeAdministratorPassword (ConvertTo-SecureString "..." -AsPlainText -Force)
```

## Administration de l'annuaire

```powershell
# Créer une unité d'organisation
New-ADOrganizationalUnit -Name "Utilisateurs"

# Créer un utilisateur
New-ADUser -Name "Jean Dupont" `
  -SamAccountName "jdupont" `
  -AccountPassword (ConvertTo-SecureString "..." -AsPlainText -Force) `
  -Enabled $true
```

## Connexion à distance (RDP)

```bash
# Depuis un poste Windows
mstsc /v:172.16.X.1
```

## Voir aussi

- [Linux](linux.md)
- [DNS](../services/dns.md)
