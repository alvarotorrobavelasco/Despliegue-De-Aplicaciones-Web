# 🧩 Actividad #1 — Instalación de Apache en Ubuntu

## 🎯 Objetivo
Instalar y comprobar el funcionamiento del **servidor web Apache** en **Ubuntu**, como parte del entorno **LAMP (Linux, Apache, MySQL, PHP)** del módulo **Desarrollo Web en Entorno Servidor (DWES)**.

---

## 📘 Recursos de referencia
- 📝 [Tutorial DigitalOcean: Instalar LAMP en Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04-es)
- 📗 [Guía sobre apt-get (Ubuntu Guía)](http://www.ubuntu-guia.com/2011/01/comando-apt-get-en-ubuntu.html)
- 💽 [Cómo instalar Ubuntu en VirtualBox (Neoguias)](https://www.neoguias.com/instalar-ubuntu-windows-virtual-box/)

---

## 🧠 Conceptos previos

El conjunto **LAMP** está formado por:

| Componente | Función |
|-------------|----------|
| **Linux** | Sistema operativo base. |
| **Apache** | Servidor web que muestra las páginas web. |
| **MySQL** | Sistema gestor de bases de datos. |
| **PHP** | Lenguaje de programación del lado del servidor. |

En esta primera actividad instalaremos **Apache**, el servidor web.

---

## ⚙️ Pasos para instalar Apache en Ubuntu

A continuación se explica el proceso completo para instalar Apache desde la terminal de Ubuntu.  
Copia o ejecuta los comandos uno por uno.


# 🔹 1. Actualizar el sistema
# Lo primero es asegurarse de que los repositorios y paquetes del sistema están actualizados.
```bash
sudo apt update
sudo apt upgrade -y
```

# 📸 Inserta aquí una captura del terminal tras ejecutar los comandos:
# ![Actualización del sistema](imagenes/update.png)

# 🔹 2. Instalar Apache
# Instalamos el servidor web con el siguiente comando:
```
sudo apt install apache2 -y
```
# Esto descargará e instalará Apache automáticamente.
# 📸 Captura recomendada:
# ![Instalación de Apache](imagenes/instalacion_apache.png)

# 🔹 3. Comprobar que Apache está funcionando
# Para verificar que el servicio se ha iniciado correctamente:
```
sudo systemctl status apache2
```
# Deberías ver el mensaje “active (running)” en verde.
# 📸 Captura recomendada:
# ![Apache activo](imagenes/apache_activo.png)

# 🔹 4. Probar Apache en el navegador
# Abre el navegador web y escribe:
```
# http://localhost
```
# Si ves una página con el texto “It works!”, Apache está funcionando correctamente.
# 📸 Captura recomendada:
# ![Página It works / Apache](imagenes/it_works.png)
