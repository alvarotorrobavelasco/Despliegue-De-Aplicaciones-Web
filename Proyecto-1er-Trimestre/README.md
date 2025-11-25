# 📘 Práctica – Servidores Web (1º Trimestre)
**Módulo:** Despliegue de Aplicaciones Web (DAW)**  
**Alumno:** *Tu nombre*  
**Fecha:** 28 de noviembre  

# 🏫 Objetivo de la práctica
Configurar Apache+WordPress, Python WSGI, autenticación, AWStats y Nginx+phpMyAdmin en una intranet.

# 1. Configuración inicial
## 1.1 Actualización
```
sudo apt update && sudo apt upgrade -y
```

## 1.2 hosts
```
127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet
```

# 2. Apache
```
sudo apt install apache2 -y
```

# 3. PHP + MySQL
```
sudo apt install php libapache2-mod-php php-mysql mysql-server -y
```

# 4. WordPress – centro.intranet
## 4.1 Descargar
```
cd /var/www/
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress centro
sudo chown -R www-data:www-data centro
```

## 4.2 VirtualHost
```
<VirtualHost *:80>
 ServerName centro.intranet
 DocumentRoot /var/www/centro
</VirtualHost>
```

## 4.3 BD
```
CREATE DATABASE wp;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'clave123';
GRANT ALL PRIVILEGES ON wp.* TO 'wpuser'@'localhost';
```

# 5. Python WSGI – departamentos.centro.intranet
## 5.1 Instalar
```
sudo apt install libapache2-mod-wsgi-py3 -y
```

## 5.2 app.wsgi
```
def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicacion Python funcionando"
    start_response(status, [('Content-Type','text/plain')])
    return [output]
```

## 5.3 VirtualHost
```
<VirtualHost *:80>
 ServerName departamentos.centro.intranet
 WSGIScriptAlias / /var/www/pythonapp/app.wsgi
 <Directory /var/www/pythonapp>
   Require all granted
 </Directory>
</VirtualHost>
```

# 6. Autenticación
```
sudo htpasswd -c /etc/apache2/.pythonpass admin
```

```
AuthType Basic
AuthUserFile /etc/apache2/.pythonpass
Require valid-user
```

# 7. AWStats
```
sudo apt install awstats -y
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
```

URL:
```
http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet
```

# 8. Nginx + PHP + phpMyAdmin
## 8.1 Instalar
```
sudo apt install nginx php-fpm php-mysql -y
```

## 8.2 VirtualHost 8080
```
server {
 listen 8080;
 server_name servidor2.centro.intranet;
 root /var/www/servidor2;
 index index.php index.html;
 location ~ \.php$ {
   include snippets/fastcgi-php.conf;
   fastcgi_pass unix:/run/php/php8.1-fpm.sock;
 }
}
```

## 8.3 phpMyAdmin
```
sudo ln -s /usr/share/phpmyadmin /var/www/servidor2/phpmyadmin
```
