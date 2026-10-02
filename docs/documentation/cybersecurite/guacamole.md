# Bastion Apache Guacamole : architecture et accès

## Rôle et fonctionnement

Apache Guacamole fournit un accès distant depuis un navigateur. L'application Web dialogue avec **guacd**, qui établit une session vers la cible en SSH, RDP ou VNC. Le navigateur ne reçoit pas nécessairement un accès réseau direct à cette cible.

```mermaid
flowchart LR
    A[Navigateur administrateur] -->|HTTPS| B[Application Guacamole]
    B -->|Protocole Guacamole| C[guacd]
    C -->|SSH ou RDP| D[Serveurs cibles]
```

Le cloisonnement dépend aussi des règles réseau. La présence d'un bastion ne bloque pas automatiquement une connexion directe depuis un poste vers un serveur.

## Déploiement CUB

Le bastion documenté est `192.168.3.209/29`, dans le VLAN 51, avec la passerelle `.214`. L'URL de la maquette utilise HTTPS sur le port 8443. Ces valeurs décrivent le projet, pas une configuration par défaut du logiciel.

La procédure pédagogique utilise Debian 12 et un script Tomcat 9. Vérifier la compatibilité des versions Java, Tomcat et Guacamole avant toute réutilisation ; ne pas remplacer arbitrairement l'une de ces versions.

## Gestion des accès

Créer des comptes nominatifs et attribuer uniquement les connexions nécessaires. Distinguer administration de l'application, utilisation des connexions et consultation des traces. La séparation des rôles doit être testée avec les permissions réellement offertes par la version et le mécanisme d'authentification installés.

Retirer les accès initiaux après validation d'un compte d'administration. Les enregistrements de sessions et leur stockage doivent être configurés explicitement ; leur présence n'est pas automatique.

## Vérification et dépannage

Tester HTTPS, l'authentification, puis une connexion SSH et une connexion RDP autorisées. Vérifier aussi qu'un compte limité ne voit pas les autres cibles. Si le portail fonctionne mais pas la session, examiner guacd, les paramètres de connexion et le filtrage vers la cible.

Prévoir sauvegarde de la configuration, des comptes et de la base éventuelle, ainsi qu'un accès de secours contrôlé.

## Situations associées

- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [Apache — architecture Guacamole](https://guacamole.apache.org/doc/gug/guacamole-architecture.html)
- [Apache — configuration Guacamole](https://guacamole.apache.org/doc/gug/configuring-guacamole.html)
