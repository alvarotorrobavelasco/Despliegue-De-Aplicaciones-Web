#!/bin/bash
#############################################################
# PRACTICA COMPLETA SERVIDORES WEB - DAW
# Script ÚNICO que instala todo + genera README PREMIUM
# Fecha límite: 28 Nov
#############################################################

clear
echo "==========================================="
echo " INSTALACIÓN COMPLETA SERVIDORES WEB (DAW) "
echo "==========================================="

#############################################
# 1. ACTUALIZAR SISTEMA
#############################################
echo "📌 Actualizando sistema..."
apt update -y && apt upgrade -y


#############################################
# 2. INSTALAR APACHE
#############################################
echo "📌 Instalando Apache..."
apt install apache2 -y
systemctl enable apache2
systemctl restart apache2


#############################################
# 3. CONFIGURAR DOMINIOS INTERNOS
#############################################
echo "📌 Configurando /etc/hosts..."
cat >> /etc/hosts <<EOF
127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet
EOF


#############################################
# 4. INSTALAR PHP + MYSQL/MARIADB
#############################################
echo "📌 Instalando PHP + MySQL..."
apt install php libapache2-mod-php php-mysql -y
apt install mariadb-server mariadb-client -y


#############################################
# 5. INSTALAR WORDPRESS EN centro.intranet
#############################################
echo "📌 Instalando WordPress..."
cd /var/www
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
mv wordpress centro.intranet
chown -R www-data:www-data /var/www/centro.intranet

#############################################
# CREAR VIRTUALHOST WORDPRESS
#############################################
echo "📌 Creando VirtualHost centro.intranet..."
cat > /etc/apache2/sites-available/centro.intranet.conf <<EOF
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
EOF

a2ensite centro.intranet.conf
a2enmod rewrite
systemctl reload apache2


#############################################
# 6. INSTALAR WSGI + PYTHON APP
#############################################
echo "📌 Instalando mod_wsgi..."
apt install libapache2-mod-wsgi-py3 -y

mkdir -p /var/www/departamentos

cat > /var/www/departamentos/app.wsgi <<EOF
def application(environ, start_response):
    status = "200 OK"
    output = b"Aplicación Python funcionando correctamente"
    headers = [("Content-Type", "text/plain"),
               ("Content-Length", str(len(output)))]
    start_response(status, headers)
    return [output]
EOF

#############################################
# VIRTUALHOST PYTHON
#############################################
echo "📌 Creando VirtualHost departamentos.centro.intranet..."
cat > /etc/apache2/sites-available/departamentos.centro.intranet.conf <<EOF
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    WSGIScriptAlias / /var/www/departamentos/app.wsgi

    <Directory /var/www/departamentos>
        Require all granted
    </Directory>
</VirtualHost>
EOF

a2ensite departamentos.centro.intranet.conf
systemctl reload apache2


#############################################
# 7. PROTEGER APP PYTHON (AUTENTICACIÓN)
#############################################
echo "📌 Protegiendo aplicación Python..."
echo "Introduce contraseña para usuario adminpy:"
htpasswd -c /etc/apache2/.pythonauth adminpy

cat >> /etc/apache2/sites-available/departamentos.centro.intranet.conf <<EOF

<Directory /var/www/departamentos>
    AuthType Basic
    AuthName "Zona protegida"
    AuthUserFile /etc/apache2/.pythonauth
    Require valid-user
</Directory>
EOF

systemctl reload apache2


#############################################
# 8. INSTALAR AWSTATS
#############################################
echo "📌 Instalando AWStats..."
apt install awstats -y
cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
a2enconf awstats
systemctl reload apache2


#############################################
# 9. INSTALAR NGINX + PHP-FPM EN PUERTO 8080
#############################################
echo "📌 Instalando Nginx + PHP-FPM..."
apt install nginx php-fpm -y

mkdir -p /var/www/servidor2

cat > /etc/nginx/sites-available/servidor2.centro.intranet <<EOF
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
EOF

ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
systemctl restart nginx


#############################################
# 10. INSTALAR PHPMYADMIN
#############################################
echo "📌 Instalando phpMyAdmin..."
apt install phpmyadmin -y


#############################################
# 11. GENERAR README.md PREMIUM
#############################################
echo "📌 Generando README.md PREMIUM..."

cat > README.md <<'EOF'
# 🌐 Práctica Servidores Web — PREMIUM  
## Despliegue de Aplicaciones Web (DAW)

---

# 📘 1. Introducción  
Este proyecto despliega un entorno web completo con Apache, WordPress, Python WSGI, autenticación, AWStats, Nginx, PHP-FPM y phpMyAdmin.  
Se utilizan dominios internos:  
- centro.intranet  
- departamentos.centro.intranet  
- servidor2.centro.intranet  

---

# 📘 2. Instalación del servidor Apache  

```bash
sudo apt update
sudo apt install apache2
systemctl status apache2
```

<!-- INSERTAR IMAGEN APACHE AQUI -->

---

# 📘 3. Configuración de dominios internos  

Editar `/etc/hosts`:

```
127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet
127.0.0.1 servidor2.centro.intranet
```

<!-- INSERTAR IMAGEN ETCHOSTS AQUI -->

---

# 📘 4. Activación de módulos PHP y MySQL  

```bash
sudo apt install php libapache2-mod-php php-mysql
sudo apt install mariadb-server mariadb-client
sudo systemctl restart apache2
```

<!-- INSERTAR IMAGEN PHPINFO AQUI -->

---

# 📘 5. Instalación de WordPress (centro.intranet)

```bash
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
mv wordpress centro.intranet
```

VirtualHost:

```apache
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet
</VirtualHost>
```

<!-- INSERTAR IMAGEN WORDPRESS AQUI -->

---

# 📘 6. Instalación WSGI y aplicación Python  

Archivo WSGI:

```python
def application(...):
    return [b"Aplicación Python funcionando correctamente"]
```

<!-- INSERTAR IMAGEN APP PYTHON AQUI -->

---

# 📘 7. Autenticación Basic  

```apache
AuthType Basic
AuthUserFile /etc/apache2/.pythonauth
Require valid-user
```

<!-- INSERTAR IMAGEN AUTENTICACION AQUI -->

---

# 📘 8. AWStats  

Acceso:
```
http://localhost/awstats/awstats.pl?config=centro.intranet
```

<!-- INSERTAR IMAGEN AWSTATS AQUI -->

---

# 📘 9. Nginx como segundo servidor (8080)

VirtualHost:

```nginx
server {
    listen 8080;
    server_name servidor2.centro.intranet;
}
```

<!-- INSERTAR IMAGEN NGINX AQUI -->

---

# 📘 10. phpMyAdmin bajo Nginx  

Acceso:
```
http://servidor2.centro.intranet:8080/phpmyadmin
```

<!-- INSERTAR IMAGEN PHPMYADMIN AQUI -->

---

# 🎯 Conclusiones  
Se ha desplegado un sistema profesional con:

- Apache + WordPress  
- Python con WSGI  
- Autenticación Basic  
- AWStats  
- Nginx + PHP-FPM  
- phpMyAdmin  

EOF

echo "🎉 SCRIPT COMPLETADO — README PREMIUM GENERADO"
