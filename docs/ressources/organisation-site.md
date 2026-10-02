# TODO — Structuration du site Cub

Mémo pour remettre le site au propre : arborescence, nav, et contenu à créer.

## Principe

Deux sections séparées mais sur le même site :

- **Documentation** → contenu général/technique, mutualisable, pas spécifique à une situation précise (ex: comment configurer un serveur DNS en général).
- **Situations** + **SP** → productions concrètes de mon groupe pour chaque situation professionnelle du référentiel.

Ne pas mélanger les deux dans les mêmes fichiers : un contenu générique va dans `documentation/`, une production liée à une situation va dans `situations/` ou `sp/`, quitte à faire un lien entre les deux (`documentation/` référencée depuis une situation, ou l'inverse).

## Arborescence cible

```
docs/
├── index.md
├── documentation/
│   ├── index.md
│   ├── adminsys/
│   │   ├── linux.md
│   │   └── windows.md
│   ├── cybersecurite/
│   │   └── stormshield.md
│   ├── devops/
│   │   └── versioning.md
│   ├── reseau/
│   │   └── cisco.md
│   └── services/
│       ├── dns.md
│       ├── services-web.md
│       └── supervision.md
├── situations/
│   ├── index.md
│   └── Situation1.md
├── sp/
│   └── index.md
├── ressources/
│   └── index.md
└── stylesheets/
    └── extra.css
```

## Nav mkdocs.yml cible

```yaml
nav:
  - Accueil: index.md
  - Documentation:
      - documentation/index.md
      - Adminsys:
          - documentation/adminsys/linux.md
          - documentation/adminsys/windows.md
      - Cybersécurité:
          - documentation/cybersecurite/stormshield.md
      - DevOps:
          - documentation/devops/versioning.md
      - Réseau:
          - documentation/reseau/cisco.md
      - Services:
          - documentation/services/dns.md
          - documentation/services/services-web.md
          - documentation/services/supervision.md
  - Situations:
      - situations/index.md
      - "Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025": situations/Situation1.md
  - Situation professionnelles:
      - sp/index.md
  - Ressources: ressources/index.md
```

## Checklist

- [ ] Créer tous les dossiers/fichiers de l'arborescence ci-dessus
- [ ] Noms de fichiers/dossiers **sans accents ni espaces** (ex: `cybersecurite`, pas `cybersécurité`) — évite les soucis d'URL sur GitHub Pages
- [ ] Chaque section a un `index.md` avec au moins un titre `#`, même minimal (nécessaire pour `navigation.indexes`)
- [ ] Mettre à jour le `nav:` du `mkdocs.yml` à chaque nouveau fichier ajouté (pas de scan automatique sans plugin dédié)
- [ ] Utiliser `"Titre : sous-titre"` avec guillemets dans le nav dès qu'un `:` apparaît dans le label
- [ ] Vérifier que `requirements.txt` contient bien toutes les deps utilisées dans `mkdocs.yml` (ex: `mkdocs-minify-plugin` si le plugin `minify` est actif)
- [ ] Tester en local avec `mkdocs serve` avant de push
- [ ] Une fois ok, `git push` sur `main` → l'action GitHub se charge du déploiement sur `gh-pages`

## Idées reprises du site de référence (Ludovic Mery)

- Page d'accueil qui explique comment naviguer + comment contribuer (PR)
- Mention explicite du déploiement auto via GitHub Actions sur la page d'accueil (transparence pédagogique)
- Sections dépliables par thématique dans "Documentation", cohérent avec la structure BTS SIO (Adminsys, Cybersécurité, DevOps, Réseau, Services)
