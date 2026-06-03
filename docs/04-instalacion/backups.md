# Estrategia de copias de seguridad

## 1. Objetivo

Definir una estrategia de backups automática para proteger las bases de datos y los archivos principales de la web.

## 2. Elementos incluidos en backup

| Elemento | Ruta o método | Frecuencia |
|---|---|---|
| Base de datos web | `mysqldump empresa_web` | Diaria |
| Base de datos gestión | `mysqldump empresa_gestion` | Diaria |
| Archivos web | `/var/www/empresa` | Diaria |
| Configuración Apache | `/etc/apache2` | Semanal |
| Configuración HAProxy | `/etc/haproxy` | Semanal |
| Scripts propios | `/usr/local/bin` | Semanal |

## 3. Directorios de backup

```bash
sudo mkdir -p /backups/mysql
sudo mkdir -p /backups/web
sudo mkdir -p /backups/config
sudo chmod 700 /backups
```

## 4. Script de backup MySQL

Crear archivo:

```bash
sudo nano /usr/local/bin/backup-mysql.sh
```

Contenido:

```bash
#!/bin/bash

FECHA=$(date '+%Y-%m-%d_%H-%M')
DESTINO="/backups/mysql"

mysqldump -u root empresa_web > "$DESTINO/empresa_web_$FECHA.sql"
mysqldump -u root empresa_gestion > "$DESTINO/empresa_gestion_$FECHA.sql"

gzip "$DESTINO/empresa_web_$FECHA.sql"
gzip "$DESTINO/empresa_gestion_$FECHA.sql"

find "$DESTINO" -type f -name "*.gz" -mtime +7 -delete
```

Permisos:

```bash
sudo chmod +x /usr/local/bin/backup-mysql.sh
```

## 5. Script de backup de archivos

Crear archivo:

```bash
sudo nano /usr/local/bin/backup-archivos.sh
```

Contenido:

```bash
#!/bin/bash

FECHA=$(date '+%Y-%m-%d_%H-%M')
DESTINO_WEB="/backups/web"
DESTINO_CONFIG="/backups/config"

rsync -av --delete /var/www/empresa/ "$DESTINO_WEB/empresa_$FECHA/"
tar -czf "$DESTINO_CONFIG/config_$FECHA.tar.gz" /etc/apache2 /etc/haproxy /usr/local/bin

find "$DESTINO_WEB" -mindepth 1 -maxdepth 1 -type d -mtime +7 -exec rm -rf {} \;
find "$DESTINO_CONFIG" -type f -name "*.tar.gz" -mtime +30 -delete
```

Permisos:

```bash
sudo chmod +x /usr/local/bin/backup-archivos.sh
```

## 6. Programación con cron

Editar cron:

```bash
sudo crontab -e
```

Añadir:

```cron
0 2 * * * /usr/local/bin/backup-mysql.sh
30 2 * * * /usr/local/bin/backup-archivos.sh
```

## 7. Copia remota con rsync

Si existe un servidor externo de backups:

```bash
rsync -avz /backups/ usuario@SERVIDOR_BACKUP:/datos/backups/empresa/
```

Puede automatizarse con clave SSH y cron.

## 8. Política de retención

| Tipo de copia | Retención |
|---|---|
| Backups diarios de MySQL | 7 días |
| Backups diarios de archivos web | 7 días |
| Backups de configuración | 30 días |
| Copia mensual externa | 6 meses |

## 9. Prueba de restauración

Restaurar base de datos en entorno de prueba:

```bash
gunzip empresa_web_FECHA.sql.gz
mysql -u root empresa_web < empresa_web_FECHA.sql
```

Restaurar archivos:

```bash
rsync -av /backups/web/empresa_FECHA/ /var/www/empresa/
```

## 10. Buenas prácticas

- No considerar válido un backup hasta haber probado su restauración.
- Proteger `/backups` con permisos restrictivos.
- Mantener una copia fuera del servidor principal.
- Registrar errores de los scripts.
- Revisar espacio en disco periódicamente.
