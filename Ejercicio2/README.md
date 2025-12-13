## 📌 Actividad #2.1 – Configuración de Apache

### Arranque del servidor
```bash
sudo systemctl start apache2
sudo systemctl status apache2
```

### Puerto 81
Archivo:
```
/etc/apache2/ports.conf
```

```apache
Listen 81
```

### Dominio marisma.intranet
Archivo:
```
/etc/hosts
```

```
127.0.0.1 marisma.intranet
```

### ServerTokens
Archivo:
```
/etc/apache2/conf-enabled/security.conf
```

```apache
ServerTokens Prod
```

### ServerSignature
```apache
ServerSignature On
```

### Directorios prueba y prueba2
```bash
sudo mkdir /var/www/html/prueba
sudo mkdir /var/www/html/prueba2
```

### Redirecciones
```apache
Redirect /prueba /prueba2
```

### UserDir
```bash
sudo a2enmod userdir
```

Acceso:
```
http://localhost/~usuario
```

### Alias
```apache
Alias /usuario /home/usuario/public_html
```

### Options
Apache indexa directorios si no existe index.html.  
Para desactivar:
```apache
Options -Indexes
```

### curl
```bash
curl -I http://localhost
```

---

## 📌 Actividad #2.2 – Scripts en Bash

- Script para añadir puertos a Apache
- Script para añadir dominios al fichero hosts
- Script para crear páginas web

Todos los scripts validan los parámetros antes de ejecutarse.

---

## ✅ Conclusión
Apache ha sido instalado y configurado correctamente en Ubuntu y se han automatizado tareas mediante scripts en Bash.
