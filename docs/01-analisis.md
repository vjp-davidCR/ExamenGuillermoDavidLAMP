# 01. Análisis de requisitos

## 1. Contexto del cliente

El cliente es una pequeña empresa que necesita una infraestructura web básica, segura y mantenible. La solución debe permitir publicar una página web corporativa y disponer de bases de datos diferenciadas para la web pública y para una aplicación interna de gestión.

El equipo actúa como departamento de sistemas de una consultora. El objetivo es documentar todo el proceso de implantación, no ejecutarlo realmente.

## 2. Necesidades principales

La empresa solicita:

- Un servidor Linux con pila LAMP.
- Servidor web Apache con soporte para PHP.
- Sistema gestor de base de datos MySQL o MariaDB.
- Una base de datos para la web pública.
- Una base de datos para gestión interna.
- Acceso remoto seguro mediante SSH.
- Firewall básico mediante UFW.
- Monitorización sencilla del sistema.
- Copias de seguridad automáticas con rotación.
- Documentación de mantenimiento y recuperación ante desastres.
- Incorporación final de un balanceador HAProxy delante de Apache.

## 3. Alcance del proyecto

Este proyecto incluye la documentación de:

1. Análisis de requisitos.
2. Diseño de arquitectura.
3. Planificación del despliegue.
4. Instalación y configuración de Apache, PHP, MySQL/MariaDB, SSH, UFW, Netdata, backups y HAProxy.
5. Guía de operación y mantenimiento.
6. Plan de recuperación ante desastres.
7. Flujo colaborativo con Git y GitHub.

No incluye la ejecución real del despliegue en un servidor físico o virtual.

## 4. Requisitos funcionales

| Código | Requisito | Prioridad |
|---|---|---|
| RF-01 | Publicar una web mediante Apache | Alta |
| RF-02 | Ejecutar páginas PHP | Alta |
| RF-03 | Crear base de datos para la web | Alta |
| RF-04 | Crear base de datos para gestión interna | Alta |
| RF-05 | Permitir administración remota por SSH | Alta |
| RF-06 | Consultar estado del sistema mediante monitorización | Media |
| RF-07 | Generar copias automáticas de bases de datos | Alta |
| RF-08 | Restaurar servicio en caso de fallo grave | Alta |
| RF-09 | Incorporar balanceo de carga con HAProxy | Media |

## 5. Requisitos no funcionales

| Código | Requisito | Descripción |
|---|---|---|
| RNF-01 | Seguridad | Solo se abrirán los puertos necesarios. |
| RNF-02 | Mantenibilidad | La documentación debe permitir repetir el proceso. |
| RNF-03 | Claridad | Todos los comandos estarán explicados. |
| RNF-04 | Trazabilidad | Los cambios quedarán registrados mediante Git. |
| RNF-05 | Disponibilidad | Se documentará recuperación ante fallos. |
| RNF-06 | Escalabilidad | HAProxy permitirá preparar la infraestructura para crecimiento futuro. |

## 6. Usuarios implicados

| Usuario | Necesidad |
|---|---|
| Administrador de sistemas | Instalar, configurar y mantener el servidor. |
| Responsable de la empresa | Conocer el estado general del servicio. |
| Usuario interno | Usar la aplicación de gestión. |
| Visitante externo | Acceder a la web pública. |

## 7. Riesgos identificados

| Riesgo | Impacto | Medida preventiva |
|---|---|---|
| Contraseñas débiles | Alto | Política de contraseñas fuertes y usuarios separados. |
| Acceso SSH expuesto | Alto | Restringir IPs, usar claves y desactivar root. |
| Pérdida de datos | Alto | Backups automáticos y pruebas de restauración. |
| Caída de Apache | Medio | Monitorización y procedimiento de reinicio. |
| Error en actualización | Medio | Realizar copias antes de actualizar. |
| Conflictos Git | Bajo | Pull frecuente, ramas claras y revisión cruzada. |

## 8. Criterios de aceptación

El proyecto se considerará correcto si:

- Todos los documentos Markdown están completos.
- El README enlaza correctamente a toda la documentación.
- El diseño incluye componentes, versiones y diagrama.
- La instalación contiene comandos realistas.
- La operación y recuperación son comprensibles para el cliente.
- Existen Pull Requests revisados por el compañero.
- Se han resuelto al menos dos conflictos Git.
- Existe un release final `v1.0`.
