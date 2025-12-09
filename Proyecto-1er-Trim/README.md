🎓 README.md — Práctica Servidores Web (1º Trimestre)

DAW — Despliegue de Aplicaciones Web
Alumno: [Tu nombre]
Fecha de entrega: 28 de noviembre

📑 ÍNDICE

1. Introducción

2. Instalación del servidor Apache

3. Configuración de dominios internos

4. Activación de módulos PHP y MySQL

5. Instalación y configuración de WordPress

6. Activación de mod_wsgi y despliegue de aplicación Python

7. Protección por autenticación

8. Instalación y configuración de AWStats

9. Instalación de segundo servidor (Nginx) con PHP y phpMyAdmin

10. Capturas de pantalla

11. Conclusiones

## 1. Introducción

En esta práctica se ha configurado un entorno de servidores web para un instituto utilizando Apache como servidor principal y Nginx como servidor alternativo.

Se han implementado:

WordPress en el dominio centro.intranet

Aplicación Python mediante WSGI en departamentos.centro.intranet

Autenticación básica para proteger la aplicación Python

AWStats para estadísticas del servidor

Nginx en el dominio servidor2.centro.intranet, puerto 8080

phpMyAdmin sobre Nginx

## 2. Instalación del servidor Apache
2.1 Instalación
sudo apt update
sudo apt install apache2

2.2 Comprobación del servicio
systemctl status apache2


📸 Captura recomendada: estado activo del servicio Apache.

## 3. Configuración de dominios internos

Editar el archivo /etc/hosts:

sudo nano /etc/hosts


Añadir:

127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet
127.0.0.1   servidor2.centro.intranet


📸 Captura recomendada del archivo hosts.

## 4. Activación de módulos PHP y MySQL
Instalación:
sudo apt install php libapache2-mod-php php-mysql
sudo apt install mariadb-server mariadb-client


Reiniciar Apache:

sudo systemctl restart apache2

## 5. Instalación y configuración de WordPress (centro.intranet)
5.1 Descargar WordPress
cd /var/www
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress centro.intranet
sudo chown -R www-data:www-data centro.intranet

5.2 Crear base de datos en MariaDB
CREATE DATABASE wpdb;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'clave123';
GRANT ALL PRIVILEGES ON wpdb.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;

5.3 Configurar VirtualHost
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


📸 Captura recomendada: instalador de WordPress funcionando.

## 6. Activación de mod_wsgi y despliegue de aplicación Python
6.1 Instalar WSGI
sudo apt install libapache2-mod-wsgi-py3
sudo systemctl restart apache2

6.2 Crear aplicación Python
sudo mkdir -p /var/www/departamentos
sudo nano /var/www/departamentos/app.wsgi


Contenido:

def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicación Python funcionando correctamente"
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]

6.3 Crear VirtualHost
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf


Contenido:

<VirtualHost *:80>
    ServerName departamentos.centro.intranet

    WSGIScriptAlias / /var/www/departamentos/app.wsgi

    <Directory /var/www/departamentos>
        Require all granted
    </Directory>
</VirtualHost>


Activar:

sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl reload apache2


📸 Captura recomendada: mensaje “Aplicación Python funcionando correctamente” en navegador.

## 7. Protección por autenticación básica
Crear archivo de contraseñas:
sudo htpasswd -c /etc/apache2/.pythonauth usuario1

Añadir autenticación al VirtualHost:
<Directory /var/www/departamentos>
    AuthType Basic
    AuthName "Zona protegida"
    AuthUserFile /etc/apache2/.pythonauth
    Require valid-user
</Directory>


Reiniciar Apache:

sudo systemctl reload apache2


📸 Captura: diálogo de autenticación del navegador.

## 8. Instalación y configuración de AWStats
sudo apt install awstats
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo a2enconf awstats
sudo systemctl reload apache2


Acceso:

http://localhost/awstats/awstats.pl?config=centro.intranet


📸 Captura: panel de estadísticas.

## 9. Instalación de Nginx como segundo servidor (servidor2.centro.intranet)
9.1 Instalar Nginx y PHP-FPM
sudo apt install nginx php-fpm

9.2 Crear directorio del sitio
sudo mkdir /var/www/servidor2

9.3 Configurar VirtualHost en puerto 8080
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


Activar sitio:

sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
sudo systemctl restart nginx

9.4 Instalar phpMyAdmin
sudo apt install phpmyadmin


Acceso:

http://servidor2.centro.intranet:8080/phpmyadmin


📸 Captura: login de phpMyAdmin.

## 10. Capturas de pantalla

Incluir:

Estado de Apache

Archivo /etc/hosts

Instalación de WordPress

App Python funcionando

Ventana de autenticación

AWStats

Nginx en puerto 8080

phpMyAdmin

## 11. Conclusiones

En esta práctica se han configurado dos servidores web, múltiples dominios internos, WordPress, WSGI para Python, autenticación, estadísticas, y phpMyAdmin.
Se ha demostrado el uso avanzado de:

VirtualHosts

módulos Apache (rewrite, wsgi, php)

seguridad (AuthBasic)

integración con Nginx

El sistema queda totalmente funcional para un entorno educativo real.
