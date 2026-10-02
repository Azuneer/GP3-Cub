# Authentification : mots de passe, Bitwarden et TOTP

## Rôle et fonctionnement

L'authentification établit une identité ; l'autorisation détermine les actions permises. Un secret long, unique et conservé dans un coffre limite les risques de réutilisation. Une authentification multifacteur combine des facteurs distincts, par exemple un secret connu et un dispositif possédé.

Bitwarden organise le partage dans une organisation et des collections, avec des accès attribués aux membres. Un coffre personnel et une collection partagée ne remplissent pas le même rôle.

## TOTP et SSH

Un code TOTP dépend d'un secret partagé et de l'heure. Une dérive importante peut entraîner un refus. Le module PAM Google Authenticator permet une intégration à l'authentification Linux ; `qrencode` produit un QR code mais ne configure pas, à lui seul, le second facteur.

La chaîne d'authentification doit être définie précisément : configuration PAM, méthodes SSH autorisées et mécanismes de secours. Un simple ajout de TOTP ne prouve pas qu'aucun autre chemin de connexion ne contourne ce contrôle.

## Mise en œuvre

1. Définir les comptes concernés et les facteurs attendus.
2. Vérifier la synchronisation horaire.
3. Enrôler un compte de test sans publier son secret ni son QR code.
4. Contrôler les méthodes effectives avec `sshd -T` et valider la syntaxe avec `sshd -t`.
5. Tester une nouvelle session avant de fermer l'accès de secours.

## Recette et dépannage

Tester un code correct, incorrect et expiré, ainsi que l'accès d'un compte non autorisé. Vérifier qu'une autre méthode SSH ne permet pas de contourner le dispositif prévu.

Les captures et fiches ne doivent contenir ni secret TOTP, ni codes de récupération, ni mot de passe réel. Les exemples utilisent `<mot de passe>`. Les codes de secours doivent être conservés selon une procédure distincte et contrôlée.

## Situations associées

- [Activité 0 : Mise en place du contexte CUB](../../situations/bloc2-services/activite0.md)
- [Situation 2 : Premiers paramétrages d'un pare-feu sur un site de l'entreprise](../../situations/bloc3-cyber/situation2.md)
- [Situation 4 : Déploiement et sécurisation d'un bastion](../../situations/bloc3-cyber/situation4.md)

## Sources officielles

- [Bitwarden — organisations et collections](https://bitwarden.com/en-gb/help/getting-started-organizations/)
- [Google — module PAM Authenticator](https://github.com/google/google-authenticator-libpam)
- [OpenSSH — méthodes d’authentification](https://man.openbsd.org/sshd_config)
