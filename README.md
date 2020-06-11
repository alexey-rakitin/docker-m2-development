## Docker development environment
- Nginx
- PHP-FPM
- MySQL
- Mailhog
- PHPMyAdmin 

Domain name will be project dir name + .local (directory.local)
Add a local domain to hosts file:

`127.0.0.1 project.local`
`127.0.0.1 db-admin.project.local`
`127.0.0.1 mail.project.local`

Set `server_name` to your domain in conf/nginx.conf

Set `xdebug.remote_host` to your local network ip (192.168.0.x) in PHP/zphp.ini

Paste your credentials to PHP/auth.json for composer install

Run `bin/init` to initialize containers & traefik2

Run `bin/install` to install Magento 2