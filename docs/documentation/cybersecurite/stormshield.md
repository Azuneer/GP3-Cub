# Stormshield

Documentation générique du pare-feu **UTM Stormshield** utilisée dans le contexte CUB pour sécuriser et interconnecter les agences de l'entreprise.

## Présentation

Un pare-feu **UTM** (Unified Threat Management) combine plusieurs fonctions de sécurité : pare-feu réseau, antivirus, anti-espion, antispam, prévention/détection d'intrusions, filtrage de contenus et prévention des fuites. Dans le contexte CUB, une solution **Stormshield** a été retenue en raison de :

- sa **souveraineté** (entreprise française, filiale d'Airbus Defence and Space, hors de portée du Cloud Act) ;
- sa **conformité** aux exigences de l'**ANSSI** et des marchés publics français.

## Configuration du NAT dynamique (NAPT / masquerading)

Le NAT permet aux machines des réseaux internes d'accéder à Internet en traduisant les adresses privées vers une adresse publique.

### Principe du PAT

Le **PAT** (Port Address Translation) est un NAT dynamique qui utilise le numéro de port pour multiplexer plusieurs connexions sur une seule adresse IP (jusqu'à **65 536** traductions, port codé sur 16 bits).

### Étapes de configuration

1. Ouvrir la **Politique** (ex. Politique 10) → onglet **NAT**
2. Créer une nouvelle règle de **partage d'adresse source (masquerading)**
3. **Source originale** : `Network_internals` (tous les réseaux internes protégés)
4. **Destination originale** : `Internet`
5. **Interface de sortie** : `out`
6. **Source traduite** : `Firewall_Out` avec port `ephemeral_fw` (choix aléatoire du port)
7. **Activer** la règle puis **Appliquer** la politique

!!! warning "Attention"
    Si la destination originale est laissée à `Any` au lieu de `Internet`, les flux d'administration (SSH/HTTPS) seront eux aussi traduits et interprétés comme une tentative d'intrusion, puis bloqués.

## Bonnes pratiques de sécurité

- Segmenter les réseaux (VLAN Production / Clients / Administration)
- Appliquer le filtrage au niveau applicatif (UTM)
- Utiliser des numéros de port source aléatoires pour complexifier les attaques
- Respecter les recommandations de l'**ANSSI**

## Voir aussi

- [Documents liés à la situation Cyber](../../situations/bloc3-cyber/situation1.md)
- [Cisco](../reseau/cisco.md)
