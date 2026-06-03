# Instalación y configuración de base de datos

## 1. Objetivo

Documentar la instalación y configuración de MySQL Server o MariaDB para disponer de una base de datos para la web y otra para la gestión interna.

## 2. Instalación de MySQL Server

```bash
sudo apt update
sudo apt install mysql-server -y
```

Comprobar estado:

```bash
sudo systemctl status mysql
```

Comandos de servicio:

```bash
sudo systemctl start mysql
sudo systemctl stop mysql
sudo systemctl restart mysql
sudo systemctl status mysql
```

## 3. Archivos principales

| Ruta | Función |
|---|---|
| `/etc/mysql/mysql.conf.d/` | Configuración principal de MySQL. |
| `/etc/mysql/my.cnf` | Archivo general de configuración. |
| `/var/log/mysql/error.log` | Registro de errores. |

## 4. Acceso a consola MySQL

```bash
sudo mysql -u root
```

También puede accederse como root del sistema:

```bash
sudo su
mysql -u root
```

## 5. Seguridad inicial

Ejecutar el asistente de seguridad:

```bash
sudo mysql_secure_installation
```

Recomendaciones:

- Establecer contraseña segura.
- Eliminar usuarios anónimos.
- Deshabilitar acceso remoto de root.
- Eliminar base de datos de prueba.
- Recargar privilegios.

## 6. Creación de bases de datos

Acceder a MySQL:

```bash
sudo mysql -u root
```

Crear bases de datos:

```sql
CREATE DATABASE empresa_web CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE empresa_gestion CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 7. Creación de usuarios

```sql
CREATE USER 'web_user'@'localhost' IDENTIFIED BY 'Cambiar_Esta_Clave_Web_2026!';
CREATE USER 'gestion_user'@'localhost' IDENTIFIED BY 'Cambiar_Esta_Clave_Gestion_2026!';
```

Asignar permisos:

```sql
GRANT ALL PRIVILEGES ON empresa_web.* TO 'web_user'@'localhost';
GRANT ALL PRIVILEGES ON empresa_gestion.* TO 'gestion_user'@'localhost';
FLUSH PRIVILEGES;
```

## 8. Comandos útiles

```sql
SHOW DATABASES;
USE empresa_web;
SHOW TABLES;
DESCRIBE nombre_tabla;
DROP DATABASE nombre_base_datos;
EXIT;
```

## 9. Comprobación desde consola

```bash
mysql -u web_user -p empresa_web
mysql -u gestion_user -p empresa_gestion
```

## 10. Copia manual de prueba

```bash
mysqldump -u root -p empresa_web > empresa_web.sql
mysqldump -u root -p empresa_gestion > empresa_gestion.sql
```

## 11. Restauración manual de prueba

```bash
mysql -u root -p empresa_web < empresa_web.sql
mysql -u root -p empresa_gestion < empresa_gestion.sql
```

## 12. Buenas prácticas

- No usar el usuario root desde aplicaciones.
- Crear un usuario por aplicación.
- Aplicar privilegios mínimos.
- No abrir el puerto 3306 a Internet.
- Probar periódicamente las copias de seguridad.
- Guardar credenciales en archivos protegidos por permisos.
