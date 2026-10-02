# etckeeper : historique des configurations Linux

## Rôle et fonctionnement

etckeeper associe un gestionnaire de versions, souvent Git, au répertoire `/etc`. Il conserve les modifications de configuration et des métadonnées complémentaires. L'intégration aux outils de paquets facilite les commits autour des installations ou mises à jour.

Il ne crée pas nécessairement un commit à chaque édition de fichier. Les commits automatiques dépendent des hooks et de la planification configurés.

## Installation et contrôle

```bash
sudo apt update
sudo apt install etckeeper
sudo etckeeper vcs status
sudo etckeeper vcs log --oneline -n 10
```

Vérifier le gestionnaire choisi dans `/etc/etckeeper/etckeeper.conf` et l'initialisation effective du dépôt. Le paquet peut réaliser cette initialisation ; ne pas recréer aveuglément un dépôt existant.

## Enregistrer une modification

Après modification d'un service et validation de sa configuration :

```bash
sudo etckeeper vcs diff
sudo etckeeper commit "Configurer la journalisation Unbound"
```

L'historique permet de rapprocher une panne d'une modification et de retrouver une version antérieure. La restauration d'un fichier doit être suivie d'une validation et, si nécessaire, d'un rechargement du service.

## Vérification et limites

Comparer le fichier attendu au diff, vérifier le message du commit et contrôler le service. Une erreur de permission peut provenir du dépôt protégé par root.

`/etc` contient potentiellement des secrets. Ce dépôt ne doit pas être publié sur GitHub ni rendu accessible aux comptes ordinaires. etckeeper complète les sauvegardes ; il ne protège pas contre la perte du disque ni ne sauvegarde automatiquement toutes les données applicatives.

## Situations associées

- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)

## Sources officielles

- [etckeeper — fonctionnement et utilisation](https://etckeeper.branchable.com/README/)
