# 06. Plan de recuperación ante desastres

## 1. Objetivo

Definir los pasos para recuperar la infraestructura en caso de fallo grave, pérdida de datos o caída del servicio.

## 2. Escenarios de desastre

| Escenario | Impacto | Prioridad |
|---|---|---|
| Caída de Apache | Web no disponible | Alta |
| Caída de MySQL | Aplicaciones sin datos | Alta |
| Error en HAProxy | Web inaccesible desde el exterior | Alta |
| Pérdida de archivos web | Web incompleta o caída | Alta |
| Pérdida de base de datos | Pérdida crítica de información | Muy alta |
| Servidor completo inutilizable | Parada total | Muy alta |

## 3. Objetivos de recuperación

| Métrica | Valor propuesto |
|---|---|
| RTO | 4 horas para restaurar servicio básico. |
| RPO | Máximo 24 horas de pérdida de datos. |

RTO: tiempo máximo aceptable para recuperar el servicio.  
RPO: cantidad máxima de datos que se puede perder.

## 4. Recuperación de Apache

Comprobar estado:

```bash
sudo systemctl status apache2
```

Reiniciar:

```bash
sudo systemctl restart apache2
```

Validar configuración:

```bash
sudo apache2ctl configtest
```

Revisar logs:

```bash
sudo tail -n 100 /var/log/apache2/error.log
```

## 5. Recuperación de MySQL

Comprobar estado:

```bash
sudo systemctl status mysql
```

Reiniciar:

```bash
sudo systemctl restart mysql
```

Revisar logs:

```bash
sudo tail -n 100 /var/log/mysql/error.log
```

## 6. Restauración de base de datos

Localizar backup:

```bash
ls -lh /backups/mysql
```

Descomprimir:

```bash
gunzip empresa_web_FECHA.sql.gz
```

Restaurar:

```bash
mysql -u root empresa_web < empresa_web_FECHA.sql
```

Repetir para `empresa_gestion` si es necesario.

## 7. Restauración de archivos web

```bash
rsync -av /backups/web/empresa_FECHA/ /var/www/empresa/
sudo chown -R www-data:www-data /var/www/empresa
sudo systemctl reload apache2
```

## 8. Recuperación de HAProxy

Validar configuración:

```bash
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

Reiniciar:

```bash
sudo systemctl restart haproxy
```

Si HAProxy falla y Apache está operativo, se puede desviar temporalmente el tráfico directamente a Apache hasta corregir el problema.

## 9. Recuperación en servidor nuevo

Pasos generales:

1. Instalar Ubuntu Server.
2. Actualizar sistema.
3. Instalar Apache, PHP, MySQL, SSH, UFW, Netdata y HAProxy.
4. Restaurar archivos de configuración.
5. Restaurar archivos web.
6. Crear bases de datos y usuarios.
7. Restaurar dumps MySQL.
8. Aplicar reglas UFW.
9. Comprobar servicios.
10. Actualizar DNS si cambia la IP.

## 10. Orden recomendado de recuperación

1. Red y acceso SSH.
2. Firewall básico.
3. MySQL/MariaDB.
4. Apache y PHP.
5. Archivos web.
6. HAProxy.
7. Monitorización.
8. Backups automáticos.

## 11. Checklist de recuperación

- [ ] Se puede acceder por SSH.
- [ ] UFW está activo con reglas correctas.
- [ ] MySQL está activo.
- [ ] Bases de datos restauradas.
- [ ] Apache está activo.
- [ ] Web responde.
- [ ] HAProxy valida configuración.
- [ ] Backups vuelven a ejecutarse.
- [ ] Incidencia documentada.

## 12. Informe posterior

Tras una recuperación debe documentarse:

| Campo | Descripción |
|---|---|
| Fecha | Día y hora del incidente. |
| Causa | Motivo detectado. |
| Servicios afectados | Apache, MySQL, HAProxy, etc. |
| Tiempo de caída | Duración aproximada. |
| Datos perdidos | Si aplica. |
| Acción correctiva | Solución aplicada. |
| Medida preventiva | Cómo evitar repetición. |
