```bash
sudo apt install nginx
```

## Héberger un site
```
server {
    server_name site.com www.site.com;

    root /var/www/site.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/site.com /etc/nginx/sites-enabled/site.com
sudo systemctl restart nginx
```

## Configuration DNS

Deux redirections à faire :
- Entrée A (`site.com`) vers l'IPv4 du VPS
- Entrée AAAA (`www.site.com`) vers l'IPv6 du VPS

## Certificat SSL
```bash
sudo apt install snapd
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot

sudo certbot --nginx
```

## HTTP/2

Pour charger plus rapidement le site, il est possible d'utiliser le protocol HTTP/2 au lieu du protocol HTTP/1.1. La plupart des navigateurs modernes ont fait le choix d'implémenter uniquement la version d'HTTP/2 avec chiffrement donc un certificat SSL est obligatoire.

Une fois le certificat valide et déployé, il suffit de changer une ligne dans le fichier de configuration Nginx du site (`/etc/sites-available/site.com`) :
```bash
# Il suffit de rajouter "http2"
listen 443 ssl http2;

# Une fois la modification faite, il est nécessaire de redémarrer Nginx
# avec la commande "sudo systemctl restart nginx"
```
