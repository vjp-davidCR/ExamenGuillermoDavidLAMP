# Monitorización del sistema

## 1. Objetivo

Documentar una solución sencilla para monitorizar el estado del servidor LAMP y detectar problemas básicos de rendimiento o disponibilidad.

## 2. Elementos a monitorizar

| Elemento | Motivo |
|---|---|
| CPU | Detectar sobrecarga del servidor. |
| Memoria RAM | Evitar falta de memoria. |
| Disco | Prevenir caída por falta de espacio. |
| Apache | Comprobar que la web responde. |
| MySQL | Comprobar que la base de datos está activa. |
| HAProxy | Verificar el frontal de entrada. |
| Logs | Detectar errores y accesos sospechosos. |
| Backups | Confirmar que se generan correctamente. |

## 3. Instalación de Netdata

Netdata permite monitorizar el sistema de forma visual.

```bash
sudo apt update
sudo apt install netdata -y
```

Comprobar estado:

```bash
sudo systemctl status netdata
```

Acceso desde navegador:

```text
http://IP_DEL_SERVIDOR:19999
```

En producción, este puerto debe quedar restringido por firewall.

## 4. Revisión de servicios críticos

```bash
sudo systemctl status apache2
sudo systemctl status mysql
sudo systemctl status ssh
sudo systemctl status haproxy
sudo systemctl status netdata
```

## 5. Revisión de puertos

```bash
sudo ss -tulnp
```

Puertos esperados:

| Puerto | Servicio |
|---:|---|
| 22 | SSH |
| 80 | HTTP / HAProxy |
| 443 | HTTPS |
| 3306 | MySQL local o interno |
| 19999 | Netdata restringido |

## 6. Logs importantes

| Servicio | Archivo |
|---|---|
| Apache accesos | `/var/log/apache2/access.log` |
| Apache errores | `/var/log/apache2/error.log` |
| MySQL errores | `/var/log/mysql/error.log` |
| SSH/autenticación | `/var/log/auth.log` |
| Sistema | `/var/log/syslog` |
| HAProxy | `/var/log/haproxy.log` o syslog |

Comandos:

```bash
sudo tail -f /var/log/apache2/error.log
sudo tail -f /var/log/mysql/error.log
sudo tail -f /var/log/auth.log
```

## 7. Script sencillo de comprobación

Archivo propuesto:

```bash
sudo nano /usr/local/bin/check-lamp.sh
```

Contenido:

```bash
#!/bin/bash

FECHA=$(date '+%Y-%m-%d %H:%M:%S')
LOG="/var/log/check-lamp.log"

echo "[$FECHA] Comprobación LAMP" >> "$LOG"

for servicio in apache2 mysql ssh haproxy; do
    if systemctl is-active --quiet "$servicio"; then
        echo "[$FECHA] OK - $servicio activo" >> "$LOG"
    else
        echo "[$FECHA] ERROR - $servicio inactivo" >> "$LOG"
    fi
done

USO_DISCO=$(df / | awk 'NR==2 {print $5}')
echo "[$FECHA] Uso de disco raíz: $USO_DISCO" >> "$LOG"
```

Permisos:

```bash
sudo chmod +x /usr/local/bin/check-lamp.sh
```

Ejecución manual:

```bash
sudo /usr/local/bin/check-lamp.sh
sudo tail /var/log/check-lamp.log
```

## 8. Automatización con cron

Editar cron de root:

```bash
sudo crontab -e
```

Ejecutar cada 15 minutos:

```cron
*/15 * * * * /usr/local/bin/check-lamp.sh
```

## 9. Analizador de logs Apache

Como herramienta complementaria puede instalarse GoAccess:

```bash
sudo apt install goaccess -y
```

Ejemplo de uso:

```bash
sudo goaccess /var/log/apache2/access.log -c
```

## 10. Buenas prácticas

- Revisar alertas y logs de forma periódica.
- No exponer Netdata públicamente.
- Documentar incidencias detectadas.
- Comprobar backups junto con la monitorización.
- Revisar consumo de disco antes de que llegue al 90%.
