# IPv4, CIDR et calcul VLSM

## Rôle et fonctionnement

Une adresse IPv4 comporte 32 bits. Le préfixe `/n` indique le nombre de bits réseau ; les bits restants définissent les adresses du sous-réseau. Le VLSM adapte la taille de chaque bloc au besoin, au lieu d'imposer le même masque à tous les VLAN.

Pour un sous-réseau IPv4 classique, hors cas particuliers `/31` et `/32`, le nombre d'hôtes utilisables vaut `2^(32 − n) − 2`. L'adresse réseau et le broadcast ne sont pas attribués aux machines.

## Méthode

1. Compter les machines, les interfaces de passerelle et la réserve souhaitée.
2. Classer les besoins du plus grand au plus petit.
3. Choisir le plus petit bloc suffisant pour chaque besoin.
4. Respecter l'alignement du bloc et vérifier l'absence de chevauchement.
5. Documenter réseau, masque, plage, passerelle et broadcast.

## Application au VLAN Bastion

Le réseau `192.168.3.208/29` contient huit adresses : `.208` à `.215`. Le masque est `255.255.255.248`, les hôtes vont de `.209` à `.214` et le broadcast est `.215`. La convention CUB réserve la dernière adresse utilisable, `.214`, à la passerelle.

Les quatre blocs de CUB occupent `128 + 64 + 16 + 8 = 216` adresses du `/24`. Les 40 adresses restantes commencent à `.216`. Elles ne constituent pas un unique bloc CIDR : une décomposition possible est `.216/29` puis `.224/27`.

## Vérification et dépannage

Comparer l'IP et le masque du client à ceux de sa passerelle. Un masque trop large peut conduire le client à chercher une destination localement par ARP au lieu de passer par le routeur.

Le [plan d'adressage de référence](../../situations/bloc3-cyber/situation1.md) reste la source des affectations ; les exemples ci-dessus expliquent les calculs.

## Situations associées

- [Situation 1 : Phase d'analyse préalable](../../situations/bloc3-cyber/situation1.md)
- [Activité 0 : Mise en place de l'infrastructure réseau des agences de l'entreprise CUB](../../situations/bloc2-reseaux/activite0.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [IETF — CIDR, RFC 4632](https://www.rfc-editor.org/rfc/rfc4632)
