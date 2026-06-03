# 03. Planificación del proyecto

## 1. Objetivo de la planificación

Organizar el trabajo de Guillermo y David durante las cuatro sesiones del proyecto, asegurando que se cumplen los requisitos técnicos y los requisitos de colaboración con GitHub.

## 2. Fases del proyecto

| Fase | Descripción | Resultado esperado |
|---|---|---|
| Fase 1 | Creación del repositorio y estructura inicial | Repositorio preparado. |
| Fase 2 | Redacción inicial por roles | Primeros documentos en ramas. |
| Fase 3 | Revisión cruzada y merge | PR revisados y fusionados. |
| Fase 4 | Conflictos Git | Conflictos resueltos con merge y rebase. |
| Fase 5 | Intercambio de roles | Ambos trabajan sobre documentación del compañero. |
| Fase 6 | Cambio de alcance con HAProxy | Diseño actualizado. |
| Fase 7 | Pulido final y release | Release v1.0 y revisión final. |

## 3. Calendario por sesiones

| Sesión | Trabajo principal | Guillermo | David |
|---|---|---|---|
| 1 | Repositorio, ramas y primeros documentos | Análisis y diseño | Monitorización y backups |
| 2 | Revisión y conflicto en diseño | Revisa operaciones | Revisa plataforma |
| 3 | Intercambio de roles y conflicto en firewall | Operación/recuperación | Servidor web/BD/SSH |
| 4 | HAProxy, revisión final y release | Diseño o configuración HAProxy | Configuración u operación HAProxy |

## 4. Plan de ramas

| Rama | Objetivo |
|---|---|
| `feature/plataforma-base` | Análisis y diseño inicial. |
| `feature/operaciones` | Monitorización y backups. |
| `feature/actualizar-versiones` | Actualizar tabla de versiones. |
| `feature/nuevas-tecnologias` | Añadir Certbot y resolver conflicto. |
| `feature/servidor-web-y-bd` | Servidor web, BD y SSH/firewall. |
| `feature/guia-operacion` | Operación, recuperación y CHANGELOG. |
| `feature/balanceador-diseno` | Diseño y planificación de HAProxy. |
| `feature/balanceador-config` | Configuración y mantenimiento de HAProxy. |

## 5. Plan de Pull Requests

Cada Pull Request debe incluir:

- Título claro.
- Descripción breve de los cambios.
- Archivos modificados.
- Captura o comentario si se resuelve conflicto.
- Revisión del compañero.

Plantilla recomendada:

```markdown
## Cambios realizados
- 
- 

## Archivos modificados
- 

## Revisión solicitada
@compañero

## Comprobaciones
- [ ] Markdown revisado
- [ ] Enlaces internos comprobados
- [ ] Comandos técnicos revisados
```

## 6. Diagrama Gantt simplificado

```text
Sesión 1: [Repositorio][Estructura][Primeras ramas]
Sesión 2:             [Revisión][Merge][Conflicto diseño]
Sesión 3:                         [Intercambio roles][Conflicto rebase]
Sesión 4:                                      [HAProxy][Pulido][Release]
```

## 7. Criterios de control

Antes de entregar se comprobará:

- No hay documentos vacíos.
- Todos los enlaces del README funcionan.
- `CHANGELOG.md` contiene entradas de todas las sesiones.
- `REVISION.md` responde a las preguntas finales.
- Se han usado ramas y Pull Requests.
- Existe el tag o release `v1.0`.
