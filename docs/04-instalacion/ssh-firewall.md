# Configuración de SSH y firewall UFW

## Reglas UFW para el ejemplo

- Permitir SSH solo desde IP de la oficina: `sudo ufw allow from 192.168.1.0/24 to any port 22`
- Permitir tráfico web HTTP: `sudo ufw allow 80/tcp`
- Permitir tráfico HTTPS: `sudo ufw allow 443/tcp`

## 1. Objetivo

Documentar el acceso remoto seguro mediante SSH y la configuración básica del firewall UFW.

## 2. Instalación de OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server -y
```

Comprobar estado:

```bash
sudo systemctl status ssh
```

Iniciar, parar o reiniciar:

```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
```

## 3. Configuración segura de SSH

Editar archivo:

```bash
sudo nano /etc/ssh/sshd_config
```

Parámetros recomendados:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Reiniciar SSH:

```bash
sudo systemctl restart ssh
```

## 4. Acceso mediante clave pública

Desde el equipo cliente:

```bash
ssh-keygen -t ed25519 -C "administrador@empresa"
ssh-copy-id usuario@IP_DEL_SERVIDOR
```

Conexión:

```bash
ssh usuario@IP_DEL_SERVIDOR
```

## 5. Instalación de UFW

```bash
sudo apt install ufw -y
```

## 6. Reglas UFW recomendadas

Política por defecto:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Permitir SSH solo desde la red de oficina:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
```

Permitir tráfico web:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Si Netdata se consulta desde administración:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 19999 proto tcp
```

Activar firewall:

```bash
sudo ufw enable
```

Consultar estado:

```bash
sudo ufw status verbose
```

## 7. Alternativa menos restrictiva para laboratorio

En un entorno de prácticas donde no se conozca la IP de administración, podría usarse temporalmente:

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

En producción se recomienda restringir SSH por IP.

## 8. Comprobaciones

```bash
sudo systemctl status ssh
sudo ufw status numbered
ssh usuario@IP_DEL_SERVIDOR
```

## 9. Resolución de problemas

| Problema | Posible causa | Solución |
|---|---|---|
| No conecta SSH | Puerto bloqueado | Revisar UFW y red. |
| Clave rechazada | Clave no copiada | Repetir `ssh-copy-id`. |
| Apache no responde | Puerto 80 cerrado | Permitir `80/tcp`. |
| HTTPS no responde | Puerto 443 cerrado | Permitir `443/tcp`. |

## 10. Buenas prácticas

- Desactivar login directo de root.
- Usar claves SSH en lugar de contraseña.
- Limitar SSH a IPs conocidas.
- Revisar `/var/log/auth.log`.
- Documentar cambios en reglas UFW.
