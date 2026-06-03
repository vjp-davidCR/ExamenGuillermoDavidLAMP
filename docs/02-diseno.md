# 02. Diseño de la infraestructura

## 1. Arquitectura propuesta

La infraestructura propuesta se basa en una pila LAMP sobre Ubuntu Server. Como mejora final del proyecto, se añade HAProxy delante de Apache para actuar como balanceador de carga y punto de entrada del tráfico HTTP/HTTPS.

## 2. Diagrama lógico

```text
                Internet
                   |
                   v
            +--------------+
            |   HAProxy    |
            |  Puertos 80  |
            |  y 443       |
            +--------------+
                   |
                   v
        +---------------------+
        | Servidor Apache     |
        | PHP                 |
        | /var/www/html       |
        +---------------------+
                   |
                   v
        +---------------------+
        | MySQL / MariaDB     |
        | BD web              |
        | BD gestión interna  |
        +---------------------+
                   |
                   v
        +---------------------+
        | Backups             |
        | mysqldump + rsync   |
        +---------------------+
```

## 3. Componentes principales

| Componente | Versión recomendada | Función |
|---|---:|---|
| Ubuntu Server | 22.04 LTS | Sistema operativo base. |
| Apache | 2.4.60 | Servidor web. |
| PHP | 8.x | Ejecución de páginas dinámicas. |
| MySQL Server | 8.0 | Sistema gestor de bases de datos. |
| OpenSSH Server | Actual del repositorio | Administración remota segura. |
| UFW | Actual del repositorio | Firewall básico. |
| Netdata | Actual estable | Monitorización del sistema. |
| rsync | Actual del repositorio | Sincronización de copias. |
| HAProxy | 2.x | Balanceo de carga HTTP/HTTPS. |
| Certbot | 2.9 | Gestión de certificados SSL/TLS. |

## 4. Puertos utilizados

| Puerto | Protocolo | Servicio | Exposición |
|---:|---|---|---|
| 22 | TCP | SSH | Solo administración autorizada. |
| 80 | TCP | HTTP | Público. |
| 443 | TCP | HTTPS | Público. |
| 3306 | TCP | MySQL/MariaDB | No público; solo local o red interna. |
| 19999 | TCP | Netdata | Restringido a administración. |

## 5. Diseño de bases de datos

Se proponen dos bases de datos separadas:

| Base de datos | Uso | Usuario recomendado |
|---|---|---|
| `empresa_web` | Web pública corporativa | `web_user` |
| `empresa_gestion` | Aplicación interna | `gestion_user` |

La separación permite aplicar permisos diferentes y reducir riesgos si una aplicación queda comprometida.

## 6. Seguridad básica

Medidas incluidas en el diseño:

- Acceso SSH restringido.
- Firewall con política de denegar tráfico entrante por defecto.
- Apertura solo de puertos necesarios.
- Usuarios de base de datos sin privilegios globales.
- Copias de seguridad cifrables o protegidas por permisos.
- Revisión periódica de logs.
- Actualizaciones controladas del sistema.

## 7. Diseño de HAProxy

HAProxy funcionará como frontal de entrada. Recibirá las peticiones externas y las reenviará al servidor Apache.

Ejemplo conceptual:

```text
Cliente -> HAProxy:80/443 -> Apache:80 -> PHP -> MySQL
```

En una fase futura podrían añadirse más servidores Apache detrás de HAProxy.

## 8. Justificación técnica

La pila LAMP es adecuada porque utiliza herramientas conocidas, documentadas y disponibles en repositorios de Ubuntu. Apache permite servir la web, PHP añade capacidad dinámica y MySQL/MariaDB permite almacenar datos de forma estructurada. UFW simplifica la gestión del firewall y Netdata facilita una monitorización visual rápida.
