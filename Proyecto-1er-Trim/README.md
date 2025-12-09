# 🌐 Práctica de Servidores Web – 1º Trimestre  
### 🏫 Módulo: Despliegue de Aplicaciones Web (DAW)

---

## 📘 Introducción

En esta práctica se despliega un entorno profesional de servidores web para un instituto, utilizando:

- Servidor **Apache** como servidor principal  
- **WordPress** funcionando bajo el dominio `centro.intranet`  
- **Aplicación Python con mod_wsgi** bajo `departamentos.centro.intranet`  
- **Autenticación básica** mediante htpasswd para proteger la app Python  
- **AWStats** como sistema de estadísticas  
- **Nginx en puerto 8080** bajo el dominio `servidor2.centro.intranet`  
- **PHP-FPM** para ejecutar PHP bajo Nginx  
- **phpMyAdmin** ejecutándose desde Nginx  

El propósito es comprender cómo desplegar servicios web reales con distintos servidores, tecnologías y módulos de integración.

---

# 📑 Índice

1. [Instalación del servidor Apache](#1-instalación-del-servidor-apache)  
2. [Configuración de dominios internos](#2-configuración-de-dominios-internos)  
3. [Instalación de PHP y MySQL](#3-instalación-de-php-y-mysql)  
4. [Instalación y configuración de WordPress](#4-instalación-y-configuración-de-wordpress)  
5. [Aplicación Python con WSGI](#5-aplicación-python-con-wsgi)  
6. [Protección con autenticación básica](#6-protección-con-autenticación-básica)  
7. [Instalación y configuración de AWStats](#7-instalación-y-configuración-de-awstats)  
8. [Servidor Nginx en puerto 8080](#8-servidor-nginx-en-puerto-8080)  
9. [phpMyAdmin funcionado en Nginx](#9-phpmyadmin-funcionando-en-nginx)  
10. [Capturas de pantalla](#10-capturas-de-pantalla)  
11. [Conclusiones](#11-conclusiones)

---

# 1️⃣ Instalación del servidor Apache

```bash
sudo apt update
sudo apt install apache2
```
# 2️⃣ Configuración de dominios internos

Para trabajar con varios sitios web en la misma máquina, necesitamos crear dominios internos que apunten a `localhost`.  
Esto se hace editando el archivo **/etc/hosts**, que actúa como un pequeño DNS local.

### 📝 Añadir los dominios al sistema

Editamos el archivo con permisos de administrador:

```bash
sudo nano /etc/hosts

127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet
127.0.0.1   servidor2.centro.intranet
```

