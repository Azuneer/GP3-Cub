# Bienvenue

![Logo CUB](./assets/logo_cub.png){ width="200" }

Documentation **BTS SIO — GP3 Contexte Cub**.

---

## Contexte

Le projet **CUB** concerne une entreprise spécialisée dans l'incubation de startups, disposant d'un siège social à Paris et de multiples agences internationales. Face aux évolutions de son infrastructure et aux menaces de cybersécurité, la DSI déploie une refonte de son architecture réseau : segmentation stricte (VLAN, DMZ), remplacement des pare-feux par des solutions **UTM Stormshield** et application des recommandations de l'ANSSI.

Ce site compile produits de notre groupe : documentation technique, situations professionnelles et ressources de référence.

## Navigation

<div class="grid cards" markdown>

- :material-book-open-variant:{ .lg .middle } __Documentation__

    ---

    Contenu général et technique, mutualisable entre les situations : adminsys, cybersécurité, DevOps, réseau et services.

    [:octicons-arrow-right-24: Explorer](documentation/index.md)

- :material-briefcase:{ .lg .middle } __Situations__

    ---

    Productions concrètes de notre groupe pour chaque situation professionnelle du référentiel, organisées par bloc.

    [:octicons-arrow-right-24: Découvrir](situations/index.md)

- :material-clipboard-check-outline:{ .lg .middle } __Situations professionnelles__

    ---

    Synthèses et bilans des situations professionnelles réalisées.

    [:octicons-arrow-right-24: Consulter](sp/index.md)

- :material-folder-download:{ .lg .middle } __Ressources__

    ---

    Base documentaire commune : schémas, tables NAT et tables de routage.

    [:octicons-arrow-right-24: Télécharger](ressources/index.md)

</div>

## Comment contribuer

Ce site est propulsé par [MkDocs Material](https://squidfunnel.github.io/mkdocs-material/). Pour contribuer :

1. Forkez le dépôt
2. Créez une branche (`git checkout -b feature/nom`)
3. Modifiez les fichiers dans `docs/`
4. Poussez et ouvrez une **Pull Request**

## Déploiement

À chaque push sur `main`, l'action **GitHub Actions** compile et déploie automatiquement le site sur `gh-pages`. Aucune intervention manuelle n'est nécessaire.