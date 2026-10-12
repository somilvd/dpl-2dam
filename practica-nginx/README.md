# NGINX para todos y todos para NGINX

### Somil Vasandani 2º DAM<hr>

# Fase 1. Despliegue de la documentación con Zensical

## Instalación NGINX

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

## Clonar el repositorio

Comprobé que tenía zensical instalado:

```bash
alumno@debian-des:~$ zensical --version
0.0.69
```
Luego, activé el entorno virtual (venv) para trabajar con zensical mediante:

```bash
source ~/herramientas/zensical/venv/bin/activate
```

Despues de instalar NGINX, cloné el repositorio `gitanddocs`:

![](/img/clonar-repo.png)

Hago, un `ls`, después, `zensical build` y otra vez `ls` y veo que apareció la carpeta site:

![](/img/zensical-build.png)

Compruebo que nginx está activo:

![](/img/estado-nginx.png)

## Copiar la documentación generada

Cree una carpeta para los archivos que NGINX va a servir y copié en ella el contenido generado por Zensical:

```bash
sudo mkdir -p /var/www/gitanddocs
sudo cp -r ~/gitanddocs/site/* /var/www/gitanddocs/
```

## Configurar NGINX

Abrí el archivo de configuración de NGINX:

```bash
sudo nano /etc/nginx/conf.d/default.conf
```

Busqué la línea:

```nginx
root /usr/share/nginx/html;
```

Y la cambié por:

```nginx
root /var/www/gitanddocs;
```

![](/img/cambio-conf.png)

Así, NGINX servirá la documentación generada por Zensical desde la carpeta `/var/www/gitanddocs`.

Guardé los cambios con `Ctrl + O`, pulsé `Enter` y salí con `Ctrl + X`.

Comprobé que la configuración sea correcta mediante:

```bash
sudo nginx -t
```
Y me apareció:

![](/img/nginx-test.png)

Reinicio NGINX, para que la configuración se guarde correctamente mediante:

```bash
sudo systemctl reload nginx
```

## Comprobar el funcionamiento

Abrí el navegador y puse `http://localhost` y apareció la documentación de `gitanddocs`:

![](/img/comprobacion-localhost.png)

# Fase 2. Despliegue de un blog personal con Jekyll

## Instalación del entorno Ruby

Ruby, básicamente, es el entorno que necesita Jekyll para funcionar. Así que, voy a instalar Ruby y las herramientas necesarias para Jekyll.

Esto instalará Ruby y las herramientas necesarias para poder instalar las dependencias de Jekyll:

```bash
sudo apt install -y ruby-full build-essential
```
![](/img/instalacion-ruby.png)

Compruebo que se instaló correctamente: 

![](/img/ruby-version.png)

## Instalación Jekyll y Bundler

Bundler es una herramienta que permite gestionar las dependencias Ruby que necesita un proyecto.

Ejecuté el siguiente comando para llevar a cabo la instalacion:

```bash
sudo gem install jekyll bundler
```
![](/img/instalacion-jekyll-bundler.png)

Compruebo que se instalaron correctamente mediante:

```bash
jekyll --version
```
y
```bash
bundle --version
```
![](/img/comprobacion-instalacion.png)

## Crear el blog personal

Voy a crear un blog con una temática de tecnología y programación. Tendrá una página de inicio y artículos que podré ampliar después.
Primero, voy a salir de la carpeta `gitanddocs` y volver al directorio personal.

Ejecuté:
```bash
cd ~
```

¿Por qué lo hago? Para crear el blog en una carpeta independiente de la documentación de Zensical. Así se mantienen separados los dos proyectos y de esta forma evito mezclar sus archivos.

Después de eso, ejecuté:

```bash
jekyll new blog-personal
```
Jekyll generará una estructura inicial con archivos de configuración, páginas, artículos de ejemplo, estilos y dependencias.

![](/img/creacion-blog.png)

Entro al proyecto mediante `cd /blog-personal`.

Luego, instalo las dependecias necesarias mediante:

```bash
bundle install
```
![](/img/dependecias-necesarias.png)

## Generar el blog

Ahora voy a comprobar que Jekyll puede construir el sitio web.

```bash
bundle exec jekyll build
```

+ `bundle exec`: ejecuta el programa utilizando las dependencias configuradas para este proyecto.
+ `jekyll build`: genera la versión estática del blog, convirtiendo sus archivos y plantillas en HTML, CSS y otros recursos listos para publicar.

![](/img/generacion-blog.png)

Terminó con:

![](/img/generacion-blog2.png)

## Comprobar que se ha generado el blog

Ejecuto:

```bash
ls -la _site
```

¿Qué hace y por qué?
+ `ls -la` muestra los archivos y carpetas, incluidos los ocultos.
+ `_site` es la carpeta donde Jekyll genera la web estática que después servirá NGINX.
+ Hay que comprobar que existe index.html y que se han generado los recursos del blog.

![](/img/site.png)

