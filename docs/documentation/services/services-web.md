# Services Web

Documentation générique des **services web** (HTTP/HTTPS) utilisée dans le contexte CUB pour l'hébergement des sites vitrines de l'entreprise.

## Environnement

Dans le contexte CUB, les serveurs web sont déployés sur **Debian (Apache)** et exposent les sites de l'entreprise sur l'ensemble des agences.

## Installation d'Apache sur Debian

```bash
sudo apt update
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2
```

## Configuration d'un site virtuel (VirtualHost)

Créer `/etc/apache2/sites-available/cubX.conf` :

```apache
<VirtualHost *:80>
    ServerName cubX.fr
    ServerAlias www.cubX.fr

    DocumentRoot /var/www/cubX

    <Directory /var/www/cubX>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Activer le site :

```bash
sudo a2ensite cubX.conf
sudo systemctl reload apache2
```

## HTTPS avec Let's Encrypt

```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d cubX.fr -d www.cubX.fr
```

L'installation de Let's Encrypt active automatiquement le HTTPS et gère le renouvellement des certificats.

## Vérification

```bash
# Version du serveur
apache2 -v

# Statut du service
sudo systemctl status apache2

# Tester la réponse
curl -I https://cubX.fr
```

## Déploiement

Les fichiers du site sont déployés dans `/var/www/cubX` et les droits adaptés pour l'utilisateur du service.

## Voir aussi

- [Linux](../adminsys/linux.md)
- [DNS](dns.md)
- [Supervision](supervision.md)
