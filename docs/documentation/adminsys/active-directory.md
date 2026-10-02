# Active Directory, DNS et comptes

## Rôle et fonctionnement

Active Directory Domain Services (AD DS) centralise les objets d'un domaine : comptes, groupes et ordinateurs. Un contrôleur de domaine assure notamment l'authentification et la réplication avec les autres contrôleurs. DNS permet aux clients de localiser les services du domaine.

Dans l'architecture évoquée par CUB, les serveurs AD sont des cibles d'administration. Leur présence dans un schéma ne prouve pas que la promotion et la réplication ont déjà été réalisées.

## Préparation

1. Fixer le nom, l'adressage et le DNS du serveur.
2. Définir le domaine et le rôle du serveur : nouveau domaine ou contrôleur supplémentaire.
3. Vérifier l'heure et la connectivité.
4. Installer le rôle AD DS puis réaliser la promotion adaptée à l'architecture.
5. Tester le DNS, la connexion d'un client et, s'il existe plusieurs contrôleurs, la réplication.

Un second contrôleur doit rejoindre le domaine existant ; créer une nouvelle forêt ne produit pas de redondance du premier domaine.

## Comptes et privilèges

Un compte local appartient à une machine ; un compte de domaine appartient à l'annuaire. Les groupes attribuent des permissions. Les unités d'organisation servent à organiser les objets et à déléguer l'administration ou appliquer des stratégies de groupe.

## Vérification et dépannage

Sur un contrôleur disposant des outils AD :

```powershell
Get-Service NTDS,DNS,Netlogon
Get-ADDomain
dcdiag /test:dns
repadmin /replsummary
```

Un échec d'ouverture de session peut provenir du DNS ou de l'heure, pas seulement du mot de passe. Vérifier ces dépendances avant de modifier les comptes. Les sauvegardes des contrôleurs doivent permettre une restauration adaptée à l'annuaire.

## Situations associées

- [Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025](../../situations/bloc2-admin-sys/situation1-windows.md)

## Sources officielles

- [Microsoft — présentation AD DS](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
