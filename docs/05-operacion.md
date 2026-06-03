# 05. Guía de operación y mantenimiento

He hecho otra editación, o no

## 1. Objetivo

Ofrecer al cliente una guía clara para mantener la infraestructura LAMP operativa, segura y revisada.

## 2. Tareas diarias

| Tarea | Comando o acción |
|---|---|
| Comprobar servicios | `systemctl status apache2 mysql haproxy ssh` |
| Revisar errores Apache | `tail -n 50 /var/log/apache2/error.log` |
| Revisar errores MySQL | `tail -n 50 /var/log/mysql/error.log` |
| Revisar disco | `df -h` |
| Confirmar backups | `ls -lh /backups/mysql` |
| Revisar monitorización | Acceder a Netdata desde red autorizada. |

## 3. Tareas semanales

- Revisar actualizaciones disponibles.
- Comprobar reglas UFW.
- Revisar logs de autenticación.
- Verificar que las copias se están rotando.
- Probar acceso a la web pública.
- Revisar funcionamiento de HAProxy.

Comandos:

```bash
sudo apt update
apt list --upgradable
sudo ufw status verbose
sudo tail -n 100 /var/log/auth.log
```

## 4. Tareas mensuales

- Aplicar actualizaciones planificadas.
- Probar restauración de una copia en entorno de pruebas.
- Revisar usuarios de MySQL.
- Revisar usuarios del sistema.
- Revisar espacio de almacenamiento.
- Documentar incidencias.

## 5. Reinicio controlado de servicios

Apache:

```bash
sudo systemctl restart apache2
```

MySQL:

```bash
sudo systemctl restart mysql
```

HAProxy:

```bash
sudo systemctl restart haproxy
```

Antes de reiniciar, revisar configuración:

```bash
sudo apache2ctl configtest
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

## 6. Mantenimiento de HAProxy

Tareas específicas:

- Confirmar que HAProxy está activo.
- Validar su configuración antes de reiniciar.
- Revisar logs si hay errores 503 o caídas.
- Verificar que Apache responde detrás del balanceador.

Comandos:

```bash
sudo systemctl status haproxy
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
curl -I http://localhost
```

## 7. Procedimiento de actualización

1. Avisar al cliente si puede haber interrupción.
2. Realizar backup previo.
3. Actualizar lista de paquetes.
4. Aplicar actualizaciones.
5. Reiniciar servicios si es necesario.
6. Comprobar funcionamiento.
7. Registrar la actuación.

Comandos:

```bash
sudo /usr/local/bin/backup-mysql.sh
sudo apt update
sudo apt upgrade -y
sudo systemctl status apache2 mysql haproxy
```

## 8. Registro de incidencias

Ejemplo de tabla para documentar incidencias:

| Fecha | Incidencia | Causa | Solución | Responsable |
|---|---|---|---|---|
| 2026-01-10 | Web no responde | Apache detenido | Reinicio de servicio | Técnico |

## 9. Checklist rápido

- [ ] Apache activo.
- [ ] MySQL activo.
- [ ] HAProxy activo.
- [ ] SSH accesible solo por administradores.
- [ ] UFW activo.
- [ ] Backups recientes disponibles.
- [ ] Disco con espacio suficiente.
- [ ] Logs sin errores críticos.
