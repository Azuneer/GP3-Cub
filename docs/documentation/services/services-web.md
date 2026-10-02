# Services Web : Apache, HTTP et publication en DMZ

## Rôle et fonctionnement

Un serveur HTTP répond aux requêtes d'un client Web. Apache peut héberger plusieurs sites avec des VirtualHost, sélectionnés notamment à partir du nom demandé. HTTPS ajoute TLS ; la publication depuis le WAN implique aussi le routage, le filtrage et éventuellement le NAT.

## Exemple de site de maquette

Sur Debian :

```bash
sudo apt update
sudo apt install apache2
sudo install -d -m 0755 /var/www/cub
```

Créer un fichier `index.html` dans ce répertoire, puis définir un site dans `/etc/apache2/sites-available/cub.conf` :

```apache
<VirtualHost *:80>
    ServerName cub.example.org
    DocumentRoot /var/www/cub
    <Directory /var/www/cub>
        Options -Indexes
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

Le nom est un exemple documentaire. Il doit correspondre au nom choisi dans la maquette et à sa résolution DNS.

```bash
sudo a2ensite cub.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
curl -I -H 'Host: cub.example.org' http://127.0.0.1
```

## HTTPS et exposition

Installer un certificat correspondant au nom d'accès et vérifier son renouvellement. L'obtention auprès d'une autorité publique exige de prouver le contrôle du domaine avec une méthode adaptée ; une simple adresse privée ou un nom fictif ne suffit pas.

Ne publier que les ports nécessaires. L'administration du serveur doit rester sur le chemin d'administration prévu, séparée du trafic des visiteurs.

## Vérification et dépannage

Tester successivement depuis le serveur, depuis une zone interne autorisée, puis depuis le WAN. Contrôler le code HTTP, le bon VirtualHost, les journaux et le certificat. Une réponse HTTP 200 depuis le LAN ne valide pas encore la règle [NAT](../reseau/nat.md).

## Situations associées

- [Situation 3 : Routage et NAT](../../situations/bloc3-cyber/situation3.md)

## Sources officielles

- [Apache — VirtualHost par nom](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)
- [IETF — TLS](https://www.rfc-editor.org/rfc/rfc8446)
