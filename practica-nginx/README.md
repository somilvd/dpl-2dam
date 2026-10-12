# NGINX para todos y todos para NGINX

## Somil Vasandani 2º DAM

# Instalación NGINX

Instalo los requisitos previos mediante:

```bash
sudo apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring
```
![](/img/requisitos.png)

Importé una clave de firma oficial de nginx para que apt pueda verificar la autenticidad de los paquetes. Para obtener la clave hice:

```bash
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

![](/img/clave.png)

Verifiqué que el archivo descargado contenga la clave correcta:

```bash
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
```
También verifiqué que la salida debía contener la huella digital completa `573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62` de la siguiente manera:

![](/img/huella-digital.png)

Para configurar el repositorio apt para los paquetes estables de nginx, ejecuté el siguiente comando:

```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
https://nginx.org/packages/debian `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```

![](/img/paquetes-estables.png)

Configuré la fijación de repositorios para que los paquetes tengan preferencia sobre los proporcionados por la distribución:

```bash
echo -e "Paquete: *\nPin: origen nginx.org\nPin: lanzamiento o=nginx\nPin-Prioridad: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```
![](/img/preferencia.png)

Para instalar nginx, ejecuté los siguientes comandos:
```bash
sudo apt update
sudo apt install nginx
```
![](/img/instalacion.png)
