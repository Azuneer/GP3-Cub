# MkDocs Material, Obsidian et GitHub Pages

## Rôle et fonctionnement

Les notes Obsidian constituent les sources de travail ; MkDocs transforme les fichiers Markdown du dossier `docs/` en site statique. Material fournit le thème et les composants de présentation. GitHub Pages héberge le résultat produit par le workflow du dépôt.

## Structure du projet

| Élément | Rôle |
|---|---|
| `docs/` | Pages, images et documents |
| `mkdocs.yml` | Navigation et configuration |
| `hooks/fiche_layout.py` | Gabarit commun des fiches |
| `docs/stylesheets/extra.css` | Présentation graphique |
| `site/` | Résultat du build, à régénérer |

## Conversion et publication

Convertir les images Obsidian en liens relatifs, les callouts en admonitions MkDocs et les emojis en shortcodes Material. Conserver des noms de fichiers explicites. Ajouter chaque nouvelle documentation à la navigation et à l'index de sa rubrique.

```bash
.venv/bin/mkdocs build
.venv/bin/mkdocs serve
```

Le serveur de développement permet un aperçu local. Après modification du code d'un hook, un redémarrage du serveur peut être nécessaire. Le build ne publie pas à lui seul sur GitHub Pages ; la publication dépend du workflow configuré et de son déclenchement.

## Vérification et dépannage

Contrôler les images depuis une page profonde, les liens internes, les tableaux et les en-têtes. Une page accessible localement peut encore produire un lien incorrect sous le préfixe d'un projet GitHub Pages : privilégier les liens Markdown relatifs traités par MkDocs.

Conserver la distinction entre documentation générale et compte rendu de situation. Ne pas modifier directement le HTML généré pour corriger une fiche : corriger sa source, son gabarit ou son style puis reconstruire.

## Situations associées

- [Situation 3 : Gestion des versions avec Git et Github](../../situations/bloc2-admin-sys/situation3-git.md)

## Sources officielles

- [MkDocs — déploiement](https://www.mkdocs.org/user-guide/deploying-your-docs/)
