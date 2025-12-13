# 📌 Actividad #2.1 – Configuración de Apache

## Objetivo
Configurar Apache modificando puertos, directivas básicas, dominios, redirecciones y módulos.

---

## 1️⃣ Arrancar Apache
Comprobamos que el servicio está activo.

```bash
sudo systemctl start apache2
sudo systemctl status apache2
```

---

## 2️⃣ Añadir el puerto 81
Apache escucha por defecto en el puerto 80. En este paso añadimos el puerto 81.

Editamos el archivo:

```bash
sudo nano /etc/apache2/ports.conf
```

Añadimos la línea:

```apache
Listen 81
```

Reiniciamos Apache:

```bash
sudo systemctl restart apache2
```

---

## 3️⃣ Añadir el dominio marisma.intranet
Para que el sistema reconozca el dominio, lo añadimos al archivo hosts.

```bash
sudo nano /etc/hosts
```

Añadimos:

```
127.0.0.1 marisma.intranet
```

---

## 4️⃣ Cambiar ServerTokens
Esta directiva controla la información que Apache muestra en las respuestas.

Archivo:

```
/etc/apache2/conf-enabled/security.conf
```

Configuramos:

```apache
ServerTokens Prod
```

---

## 5️⃣ Cambiar ServerSignature
Esta directiva controla si se muestra el pie de página en páginas de error.

En el mismo archivo:

```apache
ServerSignature On
```

Reiniciamos Apache y comprobamos accediendo a una página inexistente.

---

## 6️⃣ Crear directorios prueba y prueba2
Creamos dos carpetas dentro del directorio web.

```bash
sudo mkdir /var/www/html/prueba
sudo mkdir /var/www/html/prueba2
```

Creamos páginas de prueba:

```bash
echo "Pagina PRUEBA" | sudo tee /var/www/html/prueba/index.html
echo "Pagina PRUEBA2" | sudo tee /var/www/html/prueba2/index.html
```

---

## 7️⃣ Redireccionar la carpeta prueba a prueba2
Activamos el módulo necesario:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

Editamos el VirtualHost:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Añadimos:

```apache
Redirect /prueba /prueba2
```

---

## 8️⃣ Redireccionar una sola página
También es posible redireccionar únicamente un archivo concreto.

Ejemplo:

```apache
Redirect /prueba/pagina.html /prueba2/index.html
```

---

## 9️⃣ Usar la directiva UserDir
Permite a cada usuario publicar contenido web.

Activamos el módulo:

```bash
sudo a2enmod userdir
sudo systemctl restart apache2
```

Creamos el directorio del usuario:

```bash
mkdir ~/public_html
```

Acceso desde el navegador:

```
http://localhost/~usuario
```

---

## 🔟 Usar Alias
Alias permite mapear una URL a un directorio concreto.

En `000-default.conf`:

```apache
Alias /usuario /home/usuario/public_html
```

---

## 1️⃣1️⃣ Directiva Options e indexación
La directiva `Options` controla el comportamiento de los directorios.

Apache indexa los directorios cuando no existe un archivo `index.html`.

Para desactivar la indexación:

```apache
Options -Indexes
```

---

## 1️⃣2️⃣ Comprobar respuestas HTTP con curl
Utilizamos curl para ver la respuesta HTTP del servidor.

```bash
curl -I http://localhost
```

---

# 📌 Actividad #2.2 – Scripts en Bash

## Objetivo
Automatizar tareas comunes de configuración de Apache mediante scripts.

### Scripts realizados
- Script para añadir un puerto de escucha a Apache.
- Script para añadir un dominio al archivo hosts.
- Script para crear una página web HTML.

Todos los scripts comprueban:
- Número correcto de parámetros.
- Existencia previa de los valores.
- Muestran mensajes de error con la sintaxis correcta.

---

## ✅ Conclusión
Se ha instalado y configurado Apache en Ubuntu paso a paso, comprendiendo el funcionamiento de sus directivas principales y automatizando tareas mediante scripts en Bash.
