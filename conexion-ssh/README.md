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

# 6. Probar la nueva conexión

Abrí una nueva terminal y probé:

```bash
ssh -i ~/.ssh/id_ed25519 alumno@127.0.0.1
```
![](/img/probar-nueva-conexion.png)

# Solución de problemas

+ Revisé los logs del servidor:
```bash
sudo journalctl -u ssh -e
```
![](/img/revisar-logs.png)

El sistema está funcionando exactamente como se espera en un entorno seguro.

+ Verifiqué los permisos en el servidor remoto:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

+ Comprobé que la clave pública correcta esté en `~/.ssh/authorized_keys`:

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1YAE5AAAAIFZCl925qrdrpIKNnQhP+UKwn+fPxhCApuLyU4X3x7pD somildam7@gmail.com
```