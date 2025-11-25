# 📘 Práctica – Servidores Web (1º Trimestre)

**Módulo:** Despliegue de Aplicaciones Web (DAW)**  
**Alumno:** *Tu nombre*  
**Fecha:** 28 de noviembre  

---

# 🏫 Objetivo de la práctica

El objetivo de esta práctica es desplegar varios servicios web dentro de una intranet simulada, utilizando distintas tecnologías:

- 🌐 Apache + WordPress → **centro.intranet**  
- 🐍 Python + WSGI → **departamentos.centro.intranet**  
- 🔐 Autenticación HTTP básica  
- 📊 AWStats  
- 🚀 Nginx + PHP + phpMyAdmin → **servidor2.centro.intranet:8080**

Esta documentación recoge todos los pasos realizados y las capturas correspondientes.

---

# ✨ 1. Configuración inicial

## 🔧 1.1 Actualización del sistema
```
sudo apt update && sudo apt upgrade -y
```

---

## 📝 1.2 Configuración de dominios internos (hosts)

```
127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet
```

📷 **Captura del archivo hosts:**  
![hosts](docs/imagenes/hosts.png)

---

# 🔥 2. Instalación de Apache

```
sudo apt install apache2 -y
```

📷 **Página "It Works" de Apache:**  
![apache-it-works](docs/imagenes/apache-it-works.png)

📷 **Versión de Apache:**  
![apache-version](docs/imagenes/apache-version.png)

---

# 🧩 3. PHP + MySQL

```
sudo apt install php libapache2-mod-php php-mysql mysql-server -y
```

📷 **Versión de PHP:**  
![php-version](docs/imagenes/php-version.png)

📷 **MySQL funcionando:**  
![mysql-status](docs/imagenes/mysql-status.png)

---

# 🌐 4. WordPress – centro.intranet

## 📥 4.1 Descarga e instalación

```
cd /var/www/
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress centro
sudo chown -R www-data:www-data centro
```

📷 **Carpeta /var/www/centro:**  
![wp-folder](docs/imagenes/wp-folder.png)

---

## ⚙️ 4.2 VirtualHost

```
<VirtualHost *:80>
 ServerName centro.intranet
 DocumentRoot /var/www/centro
</VirtualHost>
```

📷 **a2ensite centro.intranet:**  
![wp-a2ensite](docs/imagenes/wp-a2ensite.png)

---

## 🛢 4.3 Base de datos WordPress

```
CREATE DATABASE wp;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'clave123';
GRANT ALL PRIVILEGES ON wp.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
```

📷 **MySQL creando BD:**  
![wp-mysql](docs/imagenes/wp-mysql.png)

---

## 🌟 4.4 Instalación desde navegador

```
http://centro.intranet
```

📷 **Instalador de WordPress:**  
![wp-setup](docs/imagenes/wp-setup.png)

📷 **Panel admin WordPress:**  
![wp-admin](docs/imagenes/wp-admin.png)

---

# 🐍 5. Python + WSGI – departamentos.centro.intranet

## ⚙️ 5.1 Instalar WSGI

```
sudo apt install libapache2-mod-wsgi-py3 -y
sudo a2enmod wsgi
sudo systemctl restart apache2
```

📷 **Módulo WSGI activado:**  
![wsgi-enabled](docs/imagenes/wsgi-enabled.png)

---

## 📄 5.2 app.wsgi

```
def application(environ, start_response):
    status = '200 OK'
    output = b"Aplicacion Python funcionando"
    start_response(status, [('Content-Type','text/plain')])
    return [output]
```

📷 **Archivo app.wsgi:**  
![python-app](docs/imagenes/python-app.png)

---

## 🌐 5.3 VirtualHost

```
<VirtualHost *:80>
 ServerName departamentos.centro.intranet
 WSGIScriptAlias / /var/www/pythonapp/app.wsgi

 <Directory /var/www/pythonapp>
   Require all granted
 </Directory>
</VirtualHost>
```

📷 **VirtualHost Python:**  
![python-virtualhost](docs/imagenes/python-virtualhost.png)

📷 **Python funcionando:**  
![python-running](docs/imagenes/python-running.png)

---

# 🔐 6. Autenticación básica

```
sudo htpasswd -c /etc/apache2/.pythonpass admin
```

📷 **htpasswd ejecutado:**  
![htpasswd](docs/imagenes/htpasswd.png)

Ventana del navegador:

📷 **Popup de autenticación:**  
![auth-popup](docs/imagenes/auth-popup.png)

---

# 📊 7. AWStats

## 📦 Instalación

```
sudo apt install awstats -y
```

## ⚙️ Configuración

```
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
```

📷 **Config AWStats:**  
![awstats-config](docs/imagenes/awstats-config.png)

Acceso:

```
http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet
```

📷 **Panel de AWStats:**  
![awstats-panel](docs/imagenes/awstats-panel.png)

---

# 🚀 8. Nginx + PHP + phpMyAdmin – servidor2.centro.intranet

## ⚙️ 8.1 Instalación

```
sudo apt install nginx php-fpm php-mysql -y
```

📷 **Página por defecto de Nginx:**  
![nginx-default](docs/imagenes/nginx-default.png)

---

## 🛠 8.2 VirtualHost en puerto 8080

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

📷 **Config de Nginx:**  
![nginx-conf](docs/imagenes/nginx-conf.png)

---

## 🛢 8.3 phpMyAdmin

```
sudo apt install phpmyadmin -y
sudo ln -s /usr/share/phpmyadmin /var/www/servidor2/phpmyadmin
```

📷 **phpMyAdmin funcionando:**  
![phpmyadmin](docs/imagenes/phpmyadmin.png)

---

# 🎯 9. Conclusión

Esta práctica recrea una intranet profesional con varios servidores funcionando simultáneamente: Apache, Nginx, WordPress, Python WSGI, AWStats y autenticación HTTP básica.

Se ha configurado todo desde cero siguiendo buenas prácticas de despliegue.

---

# ✔️ FIN  
