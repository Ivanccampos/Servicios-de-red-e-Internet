# Docker. Práctica 4



## Lleva a cabo la práctica descrita

### Ejemplo 1: Despliegue de la aplicación Guestbook

En este ejemplo vamos a crear una imagen con una página estática. Vamos a crear tres versiones de la imagen.

#### Versión 1: Desde una imagen base

Tenemos un directorio, que en Docker se denomina contexto, donde tenemos el fichero `Dockerfile` y un directorio, llamado `public_html` con nuestra página web:

```bash
$ ls
Dockerfile  public_html
```

En este caso vamos a usar una imagen base de un sistema operativo sin ningún servicio. El fichero `Dockerfile` será el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM debian:stable-slim
RUN apt-get update && apt-get install -y apache2 && apt-get clean && rm -rf /var/lib/apt/lists/*
WORKDIR /var/www/html/
COPY public_html .
EXPOSE 80
CMD apache2ctl -D FOREGROUND
```

Al usar una imagen base `debian:stable-slim` tenemos que instalar los paquetes necesarios para tener el servidor web, en este acaso apache2. A continuación añadiremos el contenido del directorio `public_html` al directorio `/var/www/html/` del contenedor y finalmente indicamos el comando que se deberá ejecutar al crear un contenedor a partir de esta imagen: iniciamos el servidor web en segundo plano.

Para crear la imagen ejecutamos:

```
$ docker build -t icarcam940050/ejemplo1:v1 .
```
![img1](/Docker/Images/act6/Screenshot_1.png)

Comprobamos que la imagen se ha creado:

```
$ docker images
```
![img1](/Docker/Images/act6/Screenshot_2.png)
Y podemos crear un contenedor:

```bash
$ docker run -d -p 80:80 --name ejemplo1 icarcam940050/ejemplo1:v1
```
![img1](/Docker/Images/act6/Screenshot_3.png)
Y acceder con el navegador a nuestra página:

![img1](/Docker/Images/act6/Screenshot_4.png)


#### Versión 2: Desde una imagen con apache2

En este caso el fichero `Dockerfile` sería el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM httpd:2.4
COPY public_html /usr/local/apache2/htdocs/
EXPOSE 80
```

En este caso no necesitamos instalar nada, ya que la imagen tiene instalado el servidor web. En este caso y siguiendo la documentación de la imagen el *DocumentRoot* es `/usr/local/apache2/htdocs/`. No es necesario indicar el `CMD` ya que por defecto el contenedor creado a partir de esta imagen ejecutará el mismo proceso que la imagen base, es decir, la ejecución del servidor web.

De forma similar, crearíamos una imagen y un contenedor:

```
$ docker build -t icarcam940050/ejemplo1:v2 .
$ docker run -d -p 80:80 --name ejemplo1 icarcam940050/ejemplo1:v2
```
![img1](/Docker/Images/act6/Screenshot_5.png)
![img1](/Docker/Images/act6/Screenshot_6.png)


#### Versión 3: Desde una imagen con nginx

En este caso el fichero `Dockerfile` sería:

```Dockerfile
# syntax=docker/dockerfile:1
FROM nginx:1.24
COPY public_html /usr/share/nginx/html
EXPOSE 80
```

De forma similar, crearíamos una imagen y un contenedor:

```
$ docker build -t icarcam940050/ejemplo1:v3 .
$ docker run -d -p 80:80 --name ejemplo1 icarcam940050/ejemplo1:v3
```
![img1](/Docker/Images/act6/Screenshot_7.png)
![img1](/Docker/Images/act6/Screenshot_8.png)

---

### Ejemplo 2: Construcción de imágenes con una una aplicación PHP

En este ejemplo vamos a crear una imagen con una página desarrollada con PHP. Vamos a crear dos versiones de la imagen.

#### Versión 1: Desde una imagen base

En el contexto vamos a tener el fichero `Dockerfile` y un directorio, llamado `app` con nuestra aplicación.

En este caso vamos a usar una imagen base de un sistema operativo sin ningún servicio. El fichero `Dockerfile` será el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM debian:stable-slim
RUN apt-get update && apt-get install -y apache2 libapache2-mod-php7.4 php7.4 && apt-get clean && rm -rf /var/lib/apt/lists/* && rm /var/www/html/index.html
COPY app /var/www/html/
EXPOSE 80
CMD apache2ctl -D FOREGROUND
```

Al usar una imagen base `debian:stable-slim` tenemos que instalar los paquetes necesarios para tener el servidor web, php y las librerias necesarias. Eliminamos el A continuación añadiremos el contenido del directorio `app` al directorio `/var/www/html/` del contenedor. Hemos borrado el fichero `/var/www/html/index.html` para que no sea el que se muestre por defecto y finalmente indicamos el comando que se deberá ejecutar al crear un contenedor a partir de esta imagen: iniciamos el servidor web en segundo plano.

Para crear la imagen ejecutamos:

```
$ docker build -t icarcam940050/ejemplo2:v1 .
```
![img1](/Docker/Images/act6/Screenshot_9.png)

Comprobamos que la imagen se ha creado:

```
$ docker images

```
![img1](/Docker/Images/act6/Screenshot_10.png)


Y podemos crear un contenedor:

```
$ docker run -d -p 80:80 --name ejemplo2 icarcam940050/ejemplo2:v1
```
![img1](/Docker/Images/act6/Screenshot_11.png)

Y acceder con el navegador a nuestra página:

![img1](/Docker/Images/act6/Screenshot_12.png)

La aplicación tiene un fichero `info.php`que me da información sobre PHP, en este caso observamos que estamos usando la versión 7.3:

![img1](/Docker/Images/act6/Screenshot_13.png)


#### Versión 2: Desde una imagen con PHP instalado

En este caso el fichero `Dockerfile` sería el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM php:7.4-apache
COPY app /var/www/html/
EXPOSE 80
```

En este caso no necesitamos instalar nada, ya que la imagen tiene instalado el servidor web y PHP. No es necesario indicar el `CMD` ya que por defecto el contenedor creado a partir de esta imagen ejecutará el mismo proceso que la imagen base, es decir, la ejecución del servidor web.

De forma similar, crearíamos una imagen y un contenedor:

```bash
$ docker build -t icarcam940050/ejemplo2:v2 .
$ docker run -d -p 80:80 --name ejemplo2 icarcam940050/ejemplo2:v2
```
![img1](/Docker/Images/act6/Screenshot_14.png)
![img1](/Docker/Images/act6/Screenshot_15.png)

Podemos acceder al fichero `info.php` para comprobar la versión de php que estamos utilizando con esta imagen:

![img1](/Docker/Images/act6/Screenshot_16.png)

---

### Ejemplo 3: Construcción de imágenes con una una aplicación Python

En este ejemplo vamos a construir una imagen para servir una aplicación escrita en Python utilizando el framework flask. La aplicación será servida en el puerto 3000/tcp. Puedes encontrar los ficheros en este [directorio](https://github.com/icarcam940050/curso_docker_ies/tree/main/ejemplos/modulo5/ejemplo3) del repositorio.

En el contexto vamos a tener el fichero `Dockerfile` y un directorio, llamado `app` con nuestra aplicación.

En este caso vamos a usar una imagen base de un sistema operativo sin ningún servicio. El fichero `Dockerfile` será el siguiente:

```Dockerfile
# syntax=docker/dockerfile:1
FROM debian:12
RUN apt-get update && apt-get install -y python3-pip  && apt-get clean && rm -rf /var/lib/apt/lists/*
WORKDIR /usr/share/app
COPY app .
RUN pip3 install --no-cache-dir --break-system-packages -r requirements.txt
EXPOSE 3000
CMD python3 app.py
```

Algunas consideraciones:

* Sólo tenemos que instalar pip, que utilizaremos posteriormente para instalar los paquetes Python.
* Copiamos nuestra aplicación en cualquier directorio.
* Con `WORKDIR` nos posicionamos en el directorio indicado. Todas las instrucciones posteriores se realizarán sobre ese directorio.
* Instalamos los paquetes python con pip, que están listados en el fichero `requirements.txt`.
* El proceso que se va a ejecutar por defecto al iniciar el contenedor será `python3 app.py` que arranca un servidor web en el puerto 3000/tcp ofreciendo la aplicación.

Para crear la imagen ejecutamos:

```
$ docker build -t icarcam940050/ejemplo3:v1 .
```
![img1](/Docker/Images/act6/Screenshot_17.png)

Comprobamos que la imagen se ha creado:

```
$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
icarcam940050/ejemplo3     v1                  8c3275799063        1 minute ago      226MB
```
![img1](/Docker/Images/act6/Screenshot_18.png)

Y podemos crear un contenedor:

```bash
$ docker run -d -p 80:3000 --name ejemplo2 icarcam940050/ejemplo3:v1
```
![img1](/Docker/Images/act6/Screenshot_19.png)

Y acceder con el navegador a nuestra página:

![img1](/Docker/Images/act6/Screenshot_20.png)

#### Versión 2: Desde una imagen con python instalado

En este caso el dichero `Dockerfile` podría ser de esta manera:

```Dockerfile
# syntax=docker/dockerfile:1
FROM python:3.12.1-bookworm
WORKDIR /usr/share/app
COPY app .
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 3000
CMD python app.py
```

Podemos crear un contenedor:

```bash
$ docker run -d -p 80:3000 --name ejemplo2 icarcam940050/ejemplo3:v2
```
![img1](/Docker/Images/act6/Screenshot_21.png)

Y acceder con el navegador a nuestra página:

![img1](/Docker/Images/act6/Screenshot_22.png)

![img1](/Docker/Images/act6/Screenshot_23.png)

---



