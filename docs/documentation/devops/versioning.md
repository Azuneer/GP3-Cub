# Versioning

Documentation générique des pratiques **DevOps** et de **versioning** utilisées dans le contexte CUB.

## Pourquoi versionner ?

Le versioning permet de :

- conserver l'**historique** des modifications ;
- **revenir** à une version antérieure en cas de problème ;
- **collaborer** à plusieurs sur les mêmes fichiers ;
- **documenter** par des messages de commit explicites.

## Git — commandes essentielles

```bash
# Initialiser un dépôt
git init

# Préparer les modifications
git add .

# Créer un commit descriptif
git commit -m "docs: ajout de la procédure de configuration DNS"

# Pousser sur le dépôt distant
git push origin main

# Récupérer les dernières modifications
git pull
```

## Stratégie de branche

| Branche | Rôle |
|---------|------|
| `main` | Version stable / production |
| `feature/*` | Développement d'une fonctionnalité |

```bash
# Créer une branche de fonctionnalité
git checkout -b feature/maquette-reseau

# Fusionner dans main
git checkout main
git merge feature/maquette-reseau
```

## Versionner la configuration : etckeeper

**etckeeper** versionne le contenu de `/etc` sur les serveurs Debian, créant un commit à chaque modification de configuration :

```bash
sudo apt install etckeeper
sudo git -C /etc log --oneline
```

## Intégration continue (GitHub Actions)

Le site de documentation CUB est déployé automatiquement via **GitHub Actions** : à chaque push sur `main`, la documentation est compilée (MkDocs) puis publiée sur GitHub Pages.

## Voir aussi

- [Linux](../adminsys/linux.md)
