# Git et GitHub : versions, branches et collaboration

## Rôle et fonctionnement

Git conserve l'historique local d'un projet. GitHub héberge des dépôts et fournit des outils de collaboration. Le répertoire de travail contient les fichiers modifiés ; l'index prépare le prochain commit ; le dépôt conserve les versions enregistrées.

## Cycle de travail

```bash
git status
git diff
git switch -c documentation/dns
git add docs/documentation/services/dns.md
git diff --staged
git commit -m "Documenter le fonctionnement du DNS"
```

L'identité se configure avec `git config user.name` et `git config user.email`, au niveau du dépôt ou avec `--global`. Un commit reste local jusqu'à sa publication.

## Dépôt distant et Pull Request

```bash
git remote -v
git fetch origin
git log --oneline --graph --all
git push -u origin documentation/dns
```

Une Pull Request permet de comparer une branche, discuter les changements et les fusionner après validation. `fetch` récupère l'historique distant sans fusionner ; `pull` récupère puis intègre selon la configuration. Une divergence n'entraîne pas systématiquement un conflit : celui-ci dépend des modifications concurrentes.

## Annulation et conflits

`git restore --staged fichier` retire un fichier de l'index sans effacer son contenu de travail. `git restore fichier` rétablit le fichier depuis l'index et peut faire perdre les modifications non enregistrées : examiner `git diff` avant utilisation.

En cas de conflit, comprendre les deux versions, éditer le résultat, supprimer les marqueurs, tester puis enregistrer la résolution. `git revert` crée un commit inverse et convient souvent mieux qu'une réécriture de l'historique partagé.

## Vérification

Avant publication, vérifier le statut, les différences préparées, la branche et le dépôt distant. Ne jamais versionner de secret, même dans un fichier supprimé au commit suivant : il resterait dans l'historique.

Le dépôt utilisé dans la situation est `Azuneer/CUB-Scripts-Administration`. Les commandes ci-dessus sont des exemples, pas une publication automatique du travail.

## Situations associées

- [Situation 3 : Gestion des versions avec Git et Github](../../situations/bloc2-admin-sys/situation3-git.md)

## Sources officielles

- [Git — branches et fusion](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [Git — restore](https://git-scm.com/docs/git-restore)
- [GitHub — Pull Requests](https://docs.github.com/en/pull-requests/reference/pull-requests)
