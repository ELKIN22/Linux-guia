# Guía de estudio para Apache

Esta guía te ayudará a comprender desde los conceptos básicos hasta temas intermedios y avanzados relacionados con Apache, uno de los servidores web más utilizados en el mundo.

---

## Introducción a Apache
Apache HTTP Server, conocido simplemente como Apache, es un servidor web de código abierto que permite a los desarrolladores y administradores de sistemas alojar páginas web y aplicaciones.

### Características principales:
- Compatible con múltiples sistemas operativos (Linux, Windows, macOS).
- Extensible mediante módulos (mod_rewrite, mod_ssl, etc.).
- Amplia comunidad y documentación.
- Compatible con protocolos como HTTP/1.1, HTTP/2.

### Instalación en Linux:
```bash
sudo apt update
sudo apt install apache2
```

Verifica que el servidor está activo:
```bash
sudo systemctl status apache2
```

Accede al servidor local:
```bash
http://localhost
```

---

## Configuración básica
Apache utiliza archivos de configuración para controlar su comportamiento. El principal archivo de configuración es `apache2.conf` (en Debian/Ubuntu) o `httpd.conf` (en CentOS/Red Hat).

### Estructura de directorios de Apache:
- `/etc/apache2/` o `/etc/httpd/`: Contiene archivos de configuración principales.
- `/var/www/html/`: Directorio raíz de documentos donde se alojan las páginas web.
- `/var/log/apache2/`: Archivos de registro (logs).

### Configurar un host virtual (Virtual Host):
Los hosts virtuales permiten alojar varios sitios en un solo servidor.

Ejemplo de configuración de un host virtual:
```apache
<VirtualHost *:80>
    ServerName ejemplo.com
    DocumentRoot /var/www/ejemplo

    <Directory /var/www/ejemplo>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/ejemplo_error.log
    CustomLog ${APACHE_LOG_DIR}/ejemplo_access.log combined
</VirtualHost>
```
Habilitar el nuevo host virtual:
```bash
sudo a2ensite ejemplo.conf
sudo systemctl reload apache2
```

---

## Módulos en Apache
Los módulos extienden la funcionalidad del servidor Apache.

### Activar o desactivar módulos:
- Activar: `sudo a2enmod nombre_modulo`
- Desactivar: `sudo a2dismod nombre_modulo`
- Reiniciar Apache: `sudo systemctl restart apache2`

### Ejemplos de módulos comunes:
1. **mod_rewrite**: Permite reescritura de URLs.
   Ejemplo de regla:
   ```apache
   RewriteEngine On
   RewriteRule ^about$ about.html [L]
   ```
2. **mod_ssl**: Habilita HTTPS.
   - Crear un certificado SSL autogenerado:
     ```bash
     sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
     -keyout /etc/ssl/private/apache-selfsigned.key \
     -out /etc/ssl/certs/apache-selfsigned.crt
     ```
   - Configurar el host virtual:
     ```apache
     <VirtualHost *:443>
         SSLEngine on
         SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
         SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
     </VirtualHost>
     ```

---

## Autenticación y autorización
Apache permite proteger directorios y recursos mediante autenticación.

### Protección con .htaccess:
Archivo `.htaccess` para habilitar autenticación:
```apache
AuthType Basic
AuthName "Acceso restringido"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```
Crear el archivo `.htpasswd`:
```bash
sudo htpasswd -c /etc/apache2/.htpasswd usuario
```

---

## Configuraciones intermedias

### Configurar limitación de ancho de banda
Instalar el módulo `mod_ratelimit` y configurarlo:
```apache
<Directory /var/www/ejemplo>
    SetOutputFilter RATE_LIMIT
    SetEnv rate-limit 500
</Directory>
```
Esto limita la velocidad de descarga a 500 KB/s.

### Configuración de logs personalizados
Puedes personalizar el formato de los logs:
```apache
LogFormat "%h %l %u %t \"%r\" %>s %b" custom
CustomLog ${APACHE_LOG_DIR}/custom.log custom
```

---

## Técnicas avanzadas

### Balanceo de carga
Apache puede actuar como balanceador de carga con el módulo `mod_proxy_balancer`.

Configuración básica:
```apache
<Proxy balancer://mi_cluster>
    BalancerMember http://192.168.1.101
    BalancerMember http://192.168.1.102
</Proxy>

ProxyPass / balancer://mi_cluster/
ProxyPassReverse / balancer://mi_cluster/
```

### Configurar caché
El módulo `mod_cache` permite almacenar en caché recursos estáticos.
```apache
<IfModule mod_cache.c>
    CacheEnable disk /
    CacheRoot /var/cache/apache2
</IfModule>
```

---

## Herramientas para monitoreo

### Mod_status
Este módulo permite supervisar el estado de Apache en tiempo real.
Activar el módulo:
```bash
sudo a2enmod status
```
Configurar acceso restringido:
```apache
<Location "/server-status">
    Require ip 192.168.1.0/24
</Location>
```
Acceder a:
```text
http://localhost/server-status
```

---

## Resolución de problemas

### Comandos útiles:
- Verificar configuración: `sudo apachectl configtest`
- Ver errores en los logs: `tail -f /var/log/apache2/error.log`

### Errores comunes:
1. **"Address already in use"**:
   - Solución: Detener el proceso que ocupa el puerto o cambiar el puerto de Apache.

2. **403 Forbidden**:
   - Solución: Verifica permisos del directorio y configuración de `Require`.

---

Con esta guía, tienes una base para comenzar a trabajar con Apache y explorar sus funcionalidades. Si tienes dudas específicas, recuerda consultar la documentación oficial: [Apache HTTP Server Documentation](https://httpd.apache.org/docs/).

