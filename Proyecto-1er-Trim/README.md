🎓 README.md — Práctica Servidores Web (1º Trimestre)
Despliegue de Aplicaciones Web – DAW
📑 Índice

Introducción

Instalación del servidor Apache

Configuración de dominios internos

Activación de módulos PHP y MySQL

Instalación y configuración de WordPress

Instalación y configuración de aplicación Python con WSGI

Protección de la app Python con autenticación

Instalación y configuración de AWStats

Instalación de Nginx en puerto 8080 y phpMyAdmin

Capturas de pantalla

Conclusiones

Introducción

En esta práctica se implementa un entorno realista de servidores web para un instituto usando:

Apache como servidor principal

WordPress en el dominio centro.intranet

Aplicación Python con WSGI en departamentos.centro.intranet

Autenticación básica para proteger la aplicación

AWStats para estadísticas

Nginx como segundo servidor en servidor2.centro.intranet:8080

phpMyAdmin funcionando en Nginx

Configuración mediante VirtualHosts, módulos y servicios internos

El objetivo es aprender a desplegar servicios web profesionales y multi-tecnología.

Instalación del servidor Apache
Instalación:
sudo apt update
sudo apt install apache2

Comprobación:
systemctl status apache2


📸 Captura recomendada: estado de Apache funcionando

<!-- INSERTAR IMAGEN AQUÍ -->
Configuración de dominios internos

Editar /etc/hosts:

sudo nano /etc/hosts


Añadir:

127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet


📸 Captura recomendada: edición del archivo hosts

<!-- INSERTAR IMAGEN AQUÍ -->
Activación de módulos PHP y MySQL
sudo apt install php libapache2-mod-php php-mysql
sudo apt install mariadb-server mariadb-client
sudo systemctl restart apache2


📸 Captura recomendada: salida del comando phpinfo()

<!-- INSERTAR IMAGEN AQUÍ -->
Instalación y configuración de WordPress
Dominio: centro.intranet
1. Descarga de WordPress
cd /var/www
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress centro.intranet
sudo chown -R www-data:www-data centro.intranet

2. Crear base de datos en MySQL/MariaDB
CREATE DATABASE wpdb;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'clave123';
GRANT ALL PRIVILEGES ON wpdb.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;

3. Crear VirtualHost

Crear archivo:

sudo nano /etc/apache2/sites-available/centro.intranet.conf


Contenido:

<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>


Activación:

sudo a2ensite centro.intranet.conf
sudo a2enmod rewrite
sudo systemctl reload apache2


📸 Captura recomendada: instalador de WordPress cargado

<!-- INSERTAR IMAGEN AQUÍ -->
Instalación y configuración de aplicación Python con WSGI
Dominio: departamentos.centro.intranet
1. Instalar módulo WSGI
sudo apt install libapache2-mod-wsgi-py3

2. Crear aplicación Python
sudo mkdir -p /var/www/departamentos
sudo nano /var/www/departamentos/app.wsgi


Contenido:

def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicación Python funcionando correctamente"
    headers = [('Content-Type', 'text/plain'),
               ('Content-Length', str(len(output)))]
    start_response(status, headers)
    return [output]

3. Crear VirtualHost
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf


Contenido:

<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    WSGIScriptAlias / /var/www/departamentos/app.wsgi

    <Directory /var/www/departamentos>
        Require all granted
    </Directory>
</VirtualHost>


Activarlo:

sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl reload apache2


📸 Captura sugerida: mensaje "Aplicación Python funcionando correctamente"

<!-- INSERTAR IMAGEN AQUÍ -->
Protección de la app Python con autenticación

Crear archivo htpasswd:

sudo htpasswd -c /etc/apache2/.pythonauth adminpy


Añadir autenticación en el VirtualHost:

<Directory /var/www/departamentos>
    AuthType Basic
    AuthName "Zona protegida"
    AuthUserFile /etc/apache2/.pythonauth
    Require valid-user
</Directory>


📸 Captura sugerida: ventana de autenticación del navegador

<!-- INSERTAR IMAGEN AQUÍ -->
Instalación y configuración de AWStats
sudo apt install awstats
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo a2enconf awstats
sudo systemctl reload apache2


Acceder:

http://localhost/awstats/awstats.pl?config=centro.intranet


📸 Captura sugerida: panel de estadísticas AWStats

<!-- INSERTAR IMAGEN AQUÍ -->
Instalación de Nginx en puerto 8080 y phpMyAdmin
Dominio: servidor2.centro.intranet
1. Instalar Nginx y PHP-FPM
sudo apt install nginx php-fpm

2. Crear VirtualHost en Nginx

Archivo:

sudo nano /etc/nginx/sites-available/servidor2.centro.intranet


Contenido:

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


Activación:

sudo mkdir /var/www/servidor2
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
sudo systemctl restart nginx

3. Instalar phpMyAdmin
sudo apt install phpmyadmin


Acceso:

http://servidor2.centro.intranet:8080/phpmyadmin


📸 Captura sugerida: phpMyAdmin cargando correctamente

<!-- INSERTAR IMAGEN AQUÍ -->
Capturas de pantalla

Aquí puedes insertar todas tus imágenes organizadas:

## Apache funcionando
![Apache](imagenes/apache.png)

## WordPress
![WordPress](imagenes/wp.png)

## App Python
![Python](imagenes/python.png)

Conclusiones

La práctica demuestra habilidades avanzadas en:

Administración de servidores web

Configuración de múltiples dominios internos

WordPress + PHP + MySQL

Python + WSGI + seguridad básica

Análisis de logs con AWStats

Nginx + PHP-FPM como servidor alternativo

phpMyAdmin bajo otro servidor distinto

El resultado es un entorno profesional totalmente funcional.
