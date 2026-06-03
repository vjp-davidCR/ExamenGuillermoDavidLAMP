# Instalación y configuración del servidor web

He hecho una editación

## 1. Objetivo

Documentar la instalación del servidor web Apache, el soporte para PHP y la incorporación final de HAProxy como balanceador de carga.

## 2. Actualización del sistema

Antes de instalar paquetes, se actualiza la lista de repositorios:

```bash
sudo apt update
sudo apt upgrade -y
```

## 3. Instalación de Apache

```bash
sudo apt install apache2 -y
```

Comprobar estado del servicio:

```bash
sudo systemctl status apache2
```

Iniciar, parar o reiniciar Apache:

```bash
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl reload apache2
```

## 4. Directorios principales de Apache

| Ruta | Función |
|---|---|
| `/etc/apache2/apache2.conf` | Archivo principal de configuración. |
| `/etc/apache2/ports.conf` | Puertos en los que escucha Apache. |
| `/etc/apache2/sites-available/` | Hosts virtuales disponibles. |
| `/etc/apache2/sites-enabled/` | Hosts virtuales activos. |
| `/etc/apache2/mods-available/` | Módulos disponibles. |
| `/etc/apache2/mods-enabled/` | Módulos activos. |
| `/var/www/html/` | Directorio web por defecto. |

## 5. Logs de Apache

| Archivo | Descripción |
|---|---|
| `/var/log/apache2/access.log` | Registro de peticiones recibidas. |
| `/var/log/apache2/error.log` | Registro de errores del servidor. |

Comandos útiles:

```bash
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/apache2/error.log
```

## 6. Instalación de PHP

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

El paquete `libapache2-mod-php` permite servir páginas PHP desde Apache. El paquete `php-mysql` permite que PHP se conecte a bases de datos MySQL/MariaDB.

## 7. Prueba de PHP

Crear archivo de prueba:

```bash
sudo nano /var/www/html/info.php
```

Contenido:

```php
<?php
phpinfo();
?>
```

Después acceder desde navegador:

```text
http://IP_DEL_SERVIDOR/info.php
```

Por seguridad, una vez comprobado PHP, se elimina el archivo:

```bash
sudo rm /var/www/html/info.php
```

## 8. Host virtual básico

Crear archivo:

```bash
sudo nano /etc/apache2/sites-available/empresa.conf
```

Contenido de ejemplo:

```apache
<VirtualHost *:80>
    ServerName empresa.local
    DocumentRoot /var/www/empresa

    <Directory /var/www/empresa>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/empresa_error.log
    CustomLog ${APACHE_LOG_DIR}/empresa_access.log combined
</VirtualHost>
```

Crear directorio y activar sitio:

```bash
sudo mkdir -p /var/www/empresa
sudo chown -R www-data:www-data /var/www/empresa
sudo a2ensite empresa.conf
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

## 9. Instalación de HAProxy

HAProxy se añade como cambio de alcance final para actuar como balanceador delante de Apache.

```bash
sudo apt install haproxy -y
```

Comprobar estado:

```bash
sudo systemctl status haproxy
```

## 10. Configuración básica de HAProxy

Editar archivo:

```bash
sudo nano /etc/haproxy/haproxy.cfg
```

Ejemplo básico:

```haproxy
frontend http_front
    bind *:80
    default_backend apache_back

backend apache_back
    balance roundrobin
    server apache1 127.0.0.1:8080 check
```

Si HAProxy escucha en el puerto 80, Apache puede configurarse para escuchar en el puerto 8080 modificando `/etc/apache2/ports.conf`:

```apache
Listen 8080
```

Y el VirtualHost:

```apache
<VirtualHost *:8080>
    DocumentRoot /var/www/empresa
</VirtualHost>
```

Reiniciar servicios:

```bash
sudo systemctl restart apache2
sudo systemctl restart haproxy
```

## 11. Comprobaciones finales

```bash
curl -I http://localhost
sudo systemctl status apache2
sudo systemctl status haproxy
sudo apache2ctl configtest
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

## 12. Buenas prácticas

- No dejar archivos `info.php` públicos.
- Revisar logs tras cambios de configuración.
- Usar HTTPS en producción.
- Separar configuración disponible y configuración activa.
- Documentar cada cambio realizado.
