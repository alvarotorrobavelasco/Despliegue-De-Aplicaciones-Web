# 🧩 Actividad #1 — Instalación de Apache en Ubuntu

## 🎯 Objetivo
Instalar y comprobar el funcionamiento del **servidor web Apache** en **Ubuntu**, como parte del entorno **LAMP (Linux, Apache, MySQL, PHP)**.

---

## 📘 Recursos de referencia
- Tutorial principal: [Instalar Apache en Ubuntu (DigitalOcean)](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04-es)
- Guía sobre `apt-get`: [Ubuntu Guía](http://www.ubuntu-guia.com/2011/01/comando-apt-get-en-ubuntu.html)
- Instalación de Ubuntu en VirtualBox (si estás en Windows): [Neoguias](https://www.neoguias.com/instalar-ubuntu-windows-virtual-box/)

---

## 🧠 Conceptos previos

El conjunto **LAMP** se compone de:
- **L**inux → Sistema operativo base.
- **A**pache → Servidor web.
- **M**ySQL → Base de datos.
- **P**HP → Lenguaje de servidor.

En esta actividad instalaremos únicamente **Apache**.

---

## ⚙️ Pasos de instalación en Ubuntu

### 1️⃣ Actualizar repositorios del sistema
```bash
sudo apt update
sudo apt upgrade -y
