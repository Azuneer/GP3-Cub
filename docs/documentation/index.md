# Documentation technique CUB

Notions, technologies et outils abordés dans les situations professionnelles. Chaque fiche présente le fonctionnement, la mise en œuvre, les vérifications et les pistes de dépannage. Les exemples sont à adapter à la machine et à la version utilisées.

<div class="grid cards" markdown>

- :material-server:{ .lg .middle } __Administration système__

    ---

    [Virtualisation : Proxmox, VirtualBox et VirtIO](adminsys/virtualisation.md)  
    [Windows Server : préparation, SConfig et Sysprep](adminsys/windows.md)  
    [PowerShell : commandes, comptes et scripts](adminsys/powershell.md)  
    [Windows Admin Center, WinRM et administration distante](adminsys/wac.md)  
    [Debian : réseau, paquets et services systemd](adminsys/linux.md)  
    [SSH, SCP et RDP](adminsys/ssh-rdp.md)  
    [Active Directory, DNS et comptes](adminsys/active-directory.md)  
    [RAID, sauvegardes et continuité](adminsys/raid-sauvegarde.md)  

- :material-lan:{ .lg .middle } __Réseaux__

    ---

    [IPv4, CIDR et calcul VLSM](reseau/adressage-vlsm.md)  
    [VLAN, trunks et routage inter-VLAN](reseau/vlan-routage.md)  
    [Cisco IOS : VLAN, SVI et routes](reseau/cisco.md)  
    [NAT, PAT et publication de services](reseau/nat.md)  
    [TCP, UDP, ICMP et diagnostic réseau](reseau/protocoles-diagnostic.md)  
    [VPN : accès distant et interconnexion de sites](reseau/vpn.md)  
    [Draw.io, plans de câblage et Packet Tracer](reseau/schemas-maquette.md)  

- :material-shield-lock:{ .lg .middle } __Cybersécurité__

    ---

    [Stormshield : UTM, zones et politiques de sécurité](cybersecurite/stormshield.md)  
    [Bastion Apache Guacamole : architecture et accès](cybersecurite/guacamole.md)  
    [Authentification : mots de passe, Bitwarden et TOTP](cybersecurite/authentification.md)  
    [AppArmor : confinement et diagnostic](cybersecurite/apparmor.md)  
    [HTTPS, TLS et certificats](cybersecurite/tls.md)  

- :material-server-network:{ .lg .middle } __Services et supervision__

    ---

    [DNS : résolution, autorité et enregistrements](services/dns.md)  
    [Unbound : résolveur récursif, cache et DNSSEC](services/unbound.md)  
    [NTP : synchronisation horaire et fuseaux](services/ntp.md)  
    [Supervision, journaux et rsyslog](services/supervision.md)  
    [Services Web : Apache, HTTP et publication en DMZ](services/services-web.md)  
    [FTP, FTPS et SFTP](services/ftp.md)  

- :material-source-branch:{ .lg .middle } __Git et documentation__

    ---

    [Git et GitHub : versions, branches et collaboration](devops/versioning.md)  
    [etckeeper : historique des configurations Linux](devops/etckeeper.md)  
    [MkDocs Material, Obsidian et GitHub Pages](devops/mkdocs.md)  

</div>

## Repères par situation

| Situation | Documentation principale |
|---|---|
| [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../situations/bloc2-admin-sys/situation1-windows.md) | [Virtualisation : Proxmox, VirtualBox et VirtIO](adminsys/virtualisation.md), [Windows Server : préparation, SConfig et Sysprep](adminsys/windows.md), [PowerShell : commandes, comptes et scripts](adminsys/powershell.md), [SSH, SCP et RDP](adminsys/ssh-rdp.md), [Active Directory, DNS et comptes](adminsys/active-directory.md), [NTP : synchronisation horaire et fuseaux](services/ntp.md) |
| [Situation 2 : Installation d'un centre d'administration des serveurs windows (WAC - Windows Admin Center)](../situations/bloc2-admin-sys/situation2-wac.md) | [Virtualisation : Proxmox, VirtualBox et VirtIO](adminsys/virtualisation.md), [Windows Server : préparation, SConfig et Sysprep](adminsys/windows.md), [PowerShell : commandes, comptes et scripts](adminsys/powershell.md), [Windows Admin Center, WinRM et administration distante](adminsys/wac.md), [HTTPS, TLS et certificats](cybersecurite/tls.md), [NTP : synchronisation horaire et fuseaux](services/ntp.md) |
| [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../situations/bloc3-cyber/situation2.md) | [Virtualisation : Proxmox, VirtualBox et VirtIO](adminsys/virtualisation.md), [TCP, UDP, ICMP et diagnostic réseau](reseau/protocoles-diagnostic.md), [Stormshield : UTM, zones et politiques de sécurité](cybersecurite/stormshield.md), [Authentification : mots de passe, Bitwarden et TOTP](cybersecurite/authentification.md), [NTP : synchronisation horaire et fuseaux](services/ntp.md) |
| [Situation 3 : Gestion des versions avec Git et Github](../situations/bloc2-admin-sys/situation3-git.md) | [PowerShell : commandes, comptes et scripts](adminsys/powershell.md), [Git et GitHub : versions, branches et collaboration](devops/versioning.md), [MkDocs Material, Obsidian et GitHub Pages](devops/mkdocs.md) |
| [Activité 0 : Mise en place du contexte CUB](../situations/bloc2-services/activite0.md) | [Debian : réseau, paquets et services systemd](adminsys/linux.md), [SSH, SCP et RDP](adminsys/ssh-rdp.md), [Authentification : mots de passe, Bitwarden et TOTP](cybersecurite/authentification.md), [DNS : résolution, autorité et enregistrements](services/dns.md), [Supervision, journaux et rsyslog](services/supervision.md), [etckeeper : historique des configurations Linux](devops/etckeeper.md) |
| [Activité 1 : Mise en place du service DNS résolveur (Unbound)](../situations/bloc2-services/activite1-dns-resolveur.md) | [Debian : réseau, paquets et services systemd](adminsys/linux.md), [TCP, UDP, ICMP et diagnostic réseau](reseau/protocoles-diagnostic.md), [AppArmor : confinement et diagnostic](cybersecurite/apparmor.md), [DNS : résolution, autorité et enregistrements](services/dns.md), [Unbound : résolveur récursif, cache et DNSSEC](services/unbound.md), [Supervision, journaux et rsyslog](services/supervision.md) |
| [Situation 4 : Déploiement et sécurisation d'un bastion](../situations/bloc3-cyber/situation4.md) | [Debian : réseau, paquets et services systemd](adminsys/linux.md), [SSH, SCP et RDP](adminsys/ssh-rdp.md), [RAID, sauvegardes et continuité](adminsys/raid-sauvegarde.md), [IPv4, CIDR et calcul VLSM](reseau/adressage-vlsm.md), [VLAN, trunks et routage inter-VLAN](reseau/vlan-routage.md), [Bastion Apache Guacamole : architecture et accès](cybersecurite/guacamole.md), [Authentification : mots de passe, Bitwarden et TOTP](cybersecurite/authentification.md), [HTTPS, TLS et certificats](cybersecurite/tls.md) |
| [Feuille de route des chapitres](../situations/bloc2-reseaux/feuille-de-route.md) | [RAID, sauvegardes et continuité](adminsys/raid-sauvegarde.md), [TCP, UDP, ICMP et diagnostic réseau](reseau/protocoles-diagnostic.md), [VPN : accès distant et interconnexion de sites](reseau/vpn.md), [Supervision, journaux et rsyslog](services/supervision.md) |
| [Situation 1 : Phase d'analyse préalable](../situations/bloc3-cyber/situation1.md) | [IPv4, CIDR et calcul VLSM](reseau/adressage-vlsm.md), [VLAN, trunks et routage inter-VLAN](reseau/vlan-routage.md), [Cisco IOS : VLAN, SVI et routes](reseau/cisco.md), [NAT, PAT et publication de services](reseau/nat.md), [Draw.io, plans de câblage et Packet Tracer](reseau/schemas-maquette.md), [Stormshield : UTM, zones et politiques de sécurité](cybersecurite/stormshield.md) |
| [Activité 0 : Mise en place de l'infrastructure réseau des agences de l'entreprise CUB](../situations/bloc2-reseaux/activite0.md) | [IPv4, CIDR et calcul VLSM](reseau/adressage-vlsm.md), [VLAN, trunks et routage inter-VLAN](reseau/vlan-routage.md), [Cisco IOS : VLAN, SVI et routes](reseau/cisco.md), [Draw.io, plans de câblage et Packet Tracer](reseau/schemas-maquette.md) |
| [Situation 3 : Routage et NAT](../situations/bloc3-cyber/situation3.md) | [Cisco IOS : VLAN, SVI et routes](reseau/cisco.md), [NAT, PAT et publication de services](reseau/nat.md), [TCP, UDP, ICMP et diagnostic réseau](reseau/protocoles-diagnostic.md), [Stormshield : UTM, zones et politiques de sécurité](cybersecurite/stormshield.md), [Services Web : Apache, HTTP et publication en DMZ](services/services-web.md), [FTP, FTPS et SFTP](services/ftp.md) |
