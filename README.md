## EJERCICIO 1 — Crear directorios, cambiar propietario y permisos
Código:
sudo mkdir /var/www/html/inf
sudo mkdir /var/www/html/adm

sudo chown www-data:www-data /var/www/html/inf
sudo chown www-data:www-data /var/www/html/adm

sudo chmod 764 /var/www/html/inf
sudo chmod 764 /var/www/html/adm

Explicación línea por línea:

sudo mkdir /var/www/html/inf → crea el directorio inf en el documentRoot.

sudo mkdir /var/www/html/adm → crea el directorio adm en el documentRoot.

sudo chown www-data:www-data /var/www/html/inf → Apache (www-data) pasa a ser dueño de inf.

sudo chown www-data:www-data /var/www/html/adm → Apache es dueño de adm.

sudo chmod 764 /var/www/html/inf → permisos: propietario rwx, grupo rw, otros r.

sudo chmod 764 /var/www/html/adm → mismos permisos para adm.

## EJERCICIO 2 — Redirigir /inf hacia /adm
Código:
Redirect /inf /adm

Explicación:

Redirect /inf /adm → cualquier petición a /inf se envía automáticamente a /adm.

## EJERCICIO 3 — Permitir acceso solo desde la red 10.9.0.0/16
Código:
<Directory "/var/www/html/inf">
    Require ip 10.9.0.0/16
    Require all denied
</Directory>

Explicación:

<Directory ...> → indica a qué carpeta se aplica la regla.

Require ip 10.9.0.0/16 → solo usuarios de la red 10.9.x.x pueden entrar.

Require all denied → el resto queda bloqueado.

</Directory> → cierra el bloque.

## EJERCICIO 4 — Activar userdir
Código:
sudo a2enmod userdir
sudo systemctl restart apache2

mkdir ~/public_html
chmod 711 ~
chmod 755 ~/public_html

Explicación:

a2enmod userdir → habilita el módulo que permite usar /~usuario.

systemctl restart apache2 → aplica los cambios.

mkdir ~/public_html → carpeta pública del usuario.

chmod 711 ~ → Apache puede entrar a tu home.

chmod 755 ~/public_html → Apache puede leer archivos dentro.

## EJERCICIO 5 — Crear VirtualHost inf.prueba.intranet
Código:
sudo mkdir -p /var/www/inf.prueba.intranet
sudo chown -R $USER:$USER /var/www/inf.prueba.intranet

127.0.0.1 inf.prueba.intranet

<VirtualHost *:80>
    ServerName inf.prueba.intranet
    DocumentRoot /var/www/inf.prueba.intranet
    <Directory /var/www/inf.prueba.intranet>
        Require all granted
    </Directory>
</VirtualHost>

sudo a2ensite inf.prueba.intranet.conf
sudo systemctl reload apache2

Explicación:

Se crea la carpeta del sitio.

Se apunta el dominio local en /etc/hosts.

VirtualHost define nombre, carpeta y permisos.

a2ensite lo activa.

reload aplica cambios.

## EJERCICIO 6 — Crear alias /img apuntando a /home/img
Código:
sudo mkdir -p /home/img
sudo chmod 755 /home/img

Alias /img /home/img

<Directory /home/img>
    Require all granted
</Directory>

sudo systemctl restart apache2

Explicación:

Se crea carpeta física /home/img.

Permisos 755 permiten que Apache acceda.

Alias /img /home/img → URL /img muestra archivos de /home/img.

Require all granted → acceso permitido.

Se reinicia Apache.

## EJERCICIO 7 — Denegar acceso a .txt y .pdf con FileMatch
Código:
<FileMatch "\.(txt|pdf)$">
    Require all denied
</FileMatch>

Explicación:

FileMatch selecciona archivos por patrón.

\.(txt|pdf)$ → coincide con .txt y .pdf.

Require all denied → bloquea acceso (403 Forbidden).

## EJERCICIO 8 — mod_rewrite noticia/236
Resultado:
ver.php?id=noticia&num=236

Explicación:

La regla captura:

texto → noticia

número → 236

Apache construye la página final:
ver.php?id=noticia&num=236.

## EJERCICIO 9 — Expresiones regulares
1. GIF con cuatro dígitos al final
^.*[0-9]{4}\.gif$

2. Número real
^[+-]?[0-9]+(\.[0-9]+)?$

3. Color hexadecimal RGB
^#[A-Fa-f0-9]{6}$

## EJERCICIO 10 — Crear directorio con index.html
Código:
sudo mkdir -p /var/www/html/ejemplo
sudo nano /var/www/html/ejemplo/index.html


Contenido:

<h1>Página de ejemplo</h1>

Explicación:

Se crea el directorio /ejemplo dentro del documentRoot.

Se añade página index.html para visualizar contenido.
