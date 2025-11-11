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
| **Apache** | Servidor web que muestra las páginas. |
| **MySQL** | Base de datos. |
| **PHP** | Lenguaje de servidor. |

En esta primera actividad instalaremos **Apache**, el servidor web.

---

## ⚙️ Pasos para instalar Apache en Ubuntu

### 1️⃣ Actualizar repositorios del sistema
```bash
sudo apt update
sudo apt upgrade -y
