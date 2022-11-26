## Installer Nginx

```bash
sudo apt install nginx
```

## Configuration d'un site statique

Les fichiers de configurations des sites sont stockés dans `/etc/nginx/sites-available`. Une version minimale se présente sous la forme suivante :
```nginx
server {
    root /var/www/site.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Configuration d'un site PHP

Il faut d'abord installer les dépendances :
```bash
sudo apt install php mysql-default-client mysql-default-server php8.2-fpm
```

Le fichier de configuration Nginx est légèrement différent :
```nginx
server {
	root /var/www/site.com;
    index index.php;
	
    location / {
        try_files $uri $uri/ =404;
    }
    
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    }
	
    location ~ /\.ht {
        deny all;
    }
}
```

Enfin pour importer une base de données :
```bash
sudo gunzip data.sql.gz | mysql -u root -p <nom de la bdd>
sudo systemctl restart mysql
```

## Activer le site

Pour que le site soit actif, il suffit de créer un lien symbolique et de redémarrer Nginx :
```bash
sudo ln -s /etc/nginx/sites-available/site.com /etc/nginx/sites-enabled/site.com
sudo systemctl restart nginx
```

## Lier un nom de domaine

Deux redirections à faire au niveau du DNS :
- Une entrée A (`site.com`) vers l'IPv4 du VPS
- Une entrée AAAA (`www.site.com`) vers l'IPv6 du VPS

`certbot` permet de configurer automatiquement la navigation en HTTPS :
```bash
sudo apt install certbot
sudo certbot --nginx
# Puis suivre les indications
```

Pour charger plus rapidement le site, il est possible d'utiliser le protocol HTTP/2 au lieu du protocol HTTP/1.1. La plupart des navigateurs modernes ont fait le choix d'implémenter uniquement la version d'HTTP/2 avec chiffrement donc un certificat SSL est obligatoire.

Une fois le certificat valide et déployé, il suffit de changer une ligne dans le fichier de configuration Nginx du site (`/etc/sites-available/site.com`) :
```bash
# Il suffit de rajouter "http2"
listen 443 ssl http2;
```
