# 🌐 Práctica de Servidores Web – 1º Trimestre  
## 🏫 Despliegue de Aplicaciones Web (DAW)

---

## 📘 Introducción

En esta práctica se despliega un entorno profesional de servidores web para un instituto, utilizando:

- **Apache** como servidor principal  
- **WordPress** en `centro.intranet`  
- **Aplicación Python con WSGI** en `departamentos.centro.intranet`  
- **Autenticación básica** sobre la aplicación Python  
- **AWStats** para estadísticas del servidor  
- **Nginx** como segundo servidor web en `servidor2.centro.intranet:8080`  
- **phpMyAdmin** funcionando bajo Nginx  

El objetivo es aprender a instalar, configurar y gestionar múltiples servicios web en Ubuntu de forma profesional.

---

# 📑 Índice

1. [Instalación del servidor Apache](#instalación-del-servidor-apache)  
2. [Configuración de dominios internos](#configuración-de-dominios-internos)  
3. [Instalación de PHP y MySQL](#instalación-de-php-y-mysql)  
4. [Instalación y configuración de WordPress](#instalación-y-configuración-de-wordpress)  
5. [Aplicación Python con WSGI](#aplicación-python-con-wsgi)  
6. [Protección de la aplicación Python](#protección-de-la-aplicación-python)  
7. [Instalación y configuración de AWStats](#instalación-y-configuración-de-awstats)  
8. [Instalación de Nginx en puerto 8080](#instalación-de-nginx-en-puerto-8080)  
9. [Instalación de phpMyAdmin](#instalación-de-phpmyadmin)  
10. [Capturas de pantalla](#capturas-de-pantalla)  
11. [Conclusiones](#conclusiones)

---

# 🔹 Instalación del servidor Apache

### 🛠️ Instalación:
```bash
sudo apt update
sudo apt install apache2
🔍 Comprobación:
bash
Copiar código
systemctl status apache2
📸 Inserte aquí la captura del estado de Apache

<!-- EJEMPLO → ![Apache activo](imagenes/apache_status.png) -->
🔹 Configuración de dominios internos
Editar el archivo /etc/hosts:

Copiar código
127.0.0.1 centro.intranet  
127.0.0.1 departamentos.centro.intranet  
127.0.0.1 servidor2.centro.intranet  
📸 Captura del archivo hosts correctamente configurado

<!-- INSERTAR IMAGEN -->
🔹 Instalación de PHP y MySQL
bash
Copiar código
sudo apt install php libapache2-mod-php php-mysql
sudo apt install mariadb-server mariadb-client
sudo systemctl restart apache2
📸 Captura recomendada: phpinfo() funcionando

<!-- INSERTAR IMAGEN -->
🔹 Instalación y configuración de WordPress
🌍 Dominio: centro.intranet
📥 Descarga e instalación
bash
Copiar código
cd /var/www
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress centro.intranet
sudo chown -R www-data:www-data centro.intranet
🗄️ Creación de la base de datos
sql
Copiar código
CREATE DATABASE wpdb;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'clave123';
GRANT ALL PRIVILEGES ON wpdb.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
🧩 VirtualHost de WordPress
apache
Copiar código
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
Activación:

bash
Copiar código
sudo a2ensite centro.intranet.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
📸 Captura del instalador de WordPress

<!-- INSERTAR IMAGEN -->
🔹 Aplicación Python con WSGI
🌍 Dominio: departamentos.centro.intranet
📄 Archivo WSGI
python
Copiar código
def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicación Python funcionando correctamente"
    headers = [('Content-Type', 'text/plain'),
               ('Content-Length', str(len(output)))]
    start_response(status, headers)
    return [output]
🧩 VirtualHost de Python + WSGI
apache
Copiar código
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    WSGIScriptAlias / /var/www/departamentos/app.wsgi

    <Directory /var/www/departamentos>
        Require all granted
    </Directory>
</VirtualHost>
📸 Captura: aplicación Python respondiendo en navegador

<!-- INSERTAR IMAGEN -->
🔹 Protección de la aplicación Python
🔐 Autenticación básica (htpasswd)
Crear usuario:

bash
Copiar código
sudo htpasswd -c /etc/apache2/.pythonauth adminpy
Añadir al VirtualHost:

apache
Copiar código
<Directory /var/www/departamentos>
    AuthType Basic
    AuthName "Zona protegida"
    AuthUserFile /etc/apache2/.pythonauth
    Require valid-user
</Directory>
📸 Captura: ventana de autenticación solicitada por Apache

<!-- INSERTAR IMAGEN -->
🔹 Instalación y configuración de AWStats
bash
Copiar código
sudo apt install awstats
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo a2enconf awstats
sudo systemctl reload apache2
Acceso web:

arduino
Copiar código
http://localhost/awstats/awstats.pl?config=centro.intranet
📸 Captura: panel de estadísticas AWStats funcionando

<!-- INSERTAR IMAGEN -->
🔹 Instalación de Nginx en puerto 8080
🌍 Dominio: servidor2.centro.intranet
🛠️ Instalación
bash
Copiar código
sudo apt install nginx php-fpm
🧩 VirtualHost de Nginx
nginx
Copiar código
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
📸 Captura: servidor Nginx activo en puerto 8080

<!-- INSERTAR IMAGEN -->
🔹 Instalación de phpMyAdmin
bash
Copiar código
sudo apt install phpmyadmin
Acceso:

bash
Copiar código
http://servidor2.centro.intranet:8080/phpmyadmin
📸 Captura: phpMyAdmin cargando correctamente

<!-- INSERTAR IMAGEN -->
📸 Capturas de pantalla
Aquí introduces todas tus imágenes:
bash
Copiar código
## Apache funcionando
![Apache](imagenes/apache.png)

## WordPress
![WordPress](imagenes/wordpress.png)

## App Python
![Python](imagenes/python_app.png)

## AWStats
![AWStats](imagenes/awstats.png)
🧾 Conclusiones
Esta práctica permite dominar:

Administración de Apache y Nginx

Configuración de múltiples dominios virtuales

Integración de PHP, WordPress y MySQL

Ejecución de aplicaciones Python con WSGI

Seguridad mediante autenticación HTTP

Análisis de logs con AWStats

Gestión simultánea de dos servidores web

El resultado es un entorno profesional totalmente funcional para despliegue web real.

