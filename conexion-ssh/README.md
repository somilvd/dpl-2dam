# Conexión SSH mediante clave pública y deshabilitación de contraseña

### Somil Vasandani 2º DAM

# 1. Generar un par de claves SSH en el equipo local

Compruebo si ya tengo una clave SSH: 

![](/img/comprobar-ssh.png)

# 2. Copiar la clave pública al servidor remoto

Usé `ssh-copy-id`:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub alumno@127.0.0.1
```

![](/img/copiar-ssh.png)

# 3. Probar la autenticación con clave

Verifico que puedo iniciar sesión sin contraseña:

```bash
ssh -i ~/.ssh/id_ed25519 alumno@127.0.0.1
```
![](/img/probar-autenticacion.png)

# 4. Deshabilitar el inicio de sesión por contraseña en el servidor remoto

Edita la configuración de SSH en el servidor remoto:

```bash
sudo nano /etc/ssh/sshd_config
```

Y añadí las siguientes directivas:

![](/img/directivas.png)

# 5. Validar y aplicar la configuración

Antes de recargar, compruebé que no haya errores de sintaxis, mediante:

```bash
sudo sshd -t
```
Después de ejecutar el anterior comando no me salió nada, así que todo ha ido bien.

Recargué el servicio SSH:

```bash
sudo systemctl reload ssh
```
Una vez reiniciado el servicio, verifica la configuración efectiva:

```bash
sudo sshd -T | grep -E 'passwordauthentication|pubkeyauthentication|kbdinteractiveauthentication|challengeresponseauthentication|permitemptypasswords'
```

![](/img/verificar-configuracion.png)