# Revisión final del proyecto

## 1. Participantes

- Guillermo
- David

## 2. ¿Qué conflictos tuvimos y cómo los resolvimos?

Durante el proyecto se provocaron dos conflictos principales para practicar situaciones reales de trabajo colaborativo.

### Conflicto 1: `02-diseno.md`

Ambos modificamos la misma tabla de versiones de software. Una rama actualizó Apache a la versión `2.4.60`, mientras que la otra rama añadió una versión diferente y una nueva tecnología, Certbot.

La solución fue conservar la versión más actualizada de Apache y añadir también la fila de Certbot. El conflicto se resolvió mediante edición manual, `git add`, `git commit` y posterior actualización del Pull Request.

### Conflicto 2: `ssh-firewall.md`

Ambos modificamos la sección de reglas UFW. Una versión proponía reglas más restrictivas para permitir SSH solo desde la red de oficina, mientras que otra versión proponía una configuración general de laboratorio.

La solución fue combinar ambas ideas: dejar como recomendación principal la configuración restrictiva y añadir una alternativa menos restrictiva para entornos de prácticas. Este conflicto se resolvió usando `git pull --rebase origin main`, `git rebase --continue` y `git push --force-with-lease`.

## 3. ¿Qué comandos Git usamos más?

Los comandos más utilizados fueron:

```bash
git clone
git checkout -b
git status
git add
git commit
git push
git pull
git pull --rebase
git merge
git rebase --continue
git push --force-with-lease
```

También usamos Pull Requests, revisiones cruzadas, comentarios en GitHub, Issues y Releases.

## 4. ¿Qué haríamos diferente en un próximo proyecto?

En un próximo proyecto intentaríamos:

- Hacer `git pull origin main` antes de empezar cualquier cambio.
- Dividir mejor las tareas para evitar tocar la misma sección sin avisar.
- Crear Issues desde el principio para cada documento.
- Usar commits más pequeños y frecuentes.
- Revisar los enlaces internos antes de abrir cada Pull Request.
- Mantener una plantilla fija para los Pull Requests.

## 5. ¿Cómo funcionó el intercambio de roles?

El intercambio de roles fue positivo porque obligó a los dos miembros a revisar y mejorar el trabajo del compañero. Guillermo no se quedó únicamente con la parte de plataforma y David no se quedó únicamente con la parte de operaciones. Esto hizo que ambos comprendieran mejor el proyecto completo.

Además, el intercambio permitió detectar errores de redacción, mejorar comandos y comprobar si la documentación era comprensible para otra persona.

## 6. Valoración final

El proyecto nos ha permitido practicar Git y GitHub en un caso realista. También hemos aprendido a estructurar documentación técnica en Markdown y a organizar una infraestructura LAMP con seguridad, monitorización, copias de seguridad y recuperación ante desastres.

La parte más útil ha sido resolver conflictos, porque nos ha obligado a entender cómo trabaja Git cuando dos ramas modifican la misma zona de un archivo.
