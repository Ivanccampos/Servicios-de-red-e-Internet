# Docker. Práctica 4



## Lleva a cabo la práctica descrita

### Ejemplo 1: Despliegue de la aplicación Guestbook

Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que ip va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:

```bash
$ docker network create red_guestbook
```
![img1](/Docker/Images/act4/Screenshot_1.png)
Para ejecutar los contenedores:

```bash
$ docker run -d --name redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes


$ docker run -d -p 80:5000 --name guestbook --network red_guestbook iesgn/guestbook
```
![img1](/Docker/Images/act4/Screenshot_2.png)

![img1](/Docker/Images/act4/Screenshot_3.png)

#### Configuración de la aplicación guestbook

Como hemos indicado anteriormente, en la creación de la imagen `iesgn/guestbook` se ha creado una variable de entorno (llamada `REDIS_SERVER`) donde se configura el nombre del servidor de base de datos redis al que se accede, por defecto el valor de esta variable es `redis`. Por lo tanto, es necesario que el contenedor de la base de datos tenga el nombre `redis` para que el contenedor de guestbook pueda conectar a la base de datos.

Si creamos un contenedor redis con otro nombre, por ejemplo:

```bash
$ docker run -d --name contenedor_redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes
```
![img1](/Docker/Images/act4/Screenshot_4.png)

Tendremos que configurar la aplicación guestbook parea que acceda a la base de datos redis usando como nombre `contenedor_redis`, por lo tanto en la creación tendremos que definir la variable de entorno `REDIS_SERVER`, para ello ejecutamos:

```bash
$ docker run -d -p 80:5000 --name guestbook -e REDIS_SERVER=contenedor_redis --network red_guestbook iesgn/guestbook
```

---

### Ejemplo 2: Despliegue de la aplicación Temperaturas

Vamos a hacer un despliegue completo de una aplicación llamada Temperaturas. Esta aplicación nos permite consultar la temperatura mínima y máxima de todos los municipios de España.

Vamos a crear una red para conectar los dos contenedores:

```bash
$ docker network create red_temperaturas
```
![img1](/Docker/Images/act4/Screenshot_5.png)
Para ejecutar los contenedores:

```bash
$ docker run -d --name temperaturas-backend --network red_temperaturas iesgn/temperaturas_backend

$ docker run -d -p 80:3000 --name temperaturas-frontend --network red_temperaturas iesgn/temperaturas_frontend
```
![img1](/Docker/Images/act4/Screenshot_6.png)
![img1](/Docker/Images/act4/Screenshot_7.png)

![img1](/Docker/Images/act4/Screenshot_8.png)

#### Configuración de la aplicación Temperaturas

Como hemos indicado anteriormente, en la creación de la imagen `iesgn/temperaturas_frontend` se ha creado una variable de entorno (llamada `TEMP_SERVER`) donde se configura el nombre del servidor y el puerto de acceso del microservicio `frontend` y que debe corresponder con el nombre y el puerto del microservicio `backend`. Por defecto esta variable tiene como valor `temperaturas-backend:5000`, por lo tanto, es necesario que el contenedor del `backend` se llame `temperaturas-backend` y debe ofrecer el servicio en el puerto `5000`.

Si creamos un contenedor `backend` con otro nombre, por ejemplo:

```bash
$ docker run -d --name temperaturas-api --network red_temperaturas iesgn/temperaturas_backend
```
![img1](/Docker/Images/act4/Screenshot_9.png)

Tendremos que configurar la aplicación `frontend` parea que acceda al `backend` usando como nombre `temperaturas-api`, por lo tanto en la creación tendremos que definir la variable de entorno `TEMP_SERVER`, para ello ejecutamos:

```bash
$ docker run -d -p 80:3000 --name temperaturas-frontend -e TEMP_SERVER=temperaturas-api:5000 --network red_temperaturas iesgn/temperaturas_frontend
```
![img1](/Docker/Images/act4/Screenshot_10.png)

---

### Ejemplo 3: Despliegue de Wordpress + mariadb

Para la instalación de WordPress necesitamos dos contenedores: la base de datos (imagen `mariadb`) y el servidor web con la aplicación (imagen `wordpress`). Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que ip va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:

```bash
$ docker network create red_wp
```
![img1](/Docker/Images/act4/Screenshot_11.png)

Siguiendo la documentación de la imagen [mariadb](https://hub.docker.com/_/mariadb) y la imagen [wordpress](https://hub.docker.com/_/wordpress) podemos ejecutar los siguientes comandos para crear los dos contenedores:

```bash
$ docker run -d --name servidor_mysql \
                --network red_wp \
                -v /opt/mysql_wp:/var/lib/mysql \
                -e MYSQL_DATABASE=bd_wp \
                -e MYSQL_USER=user_wp \
                -e MYSQL_PASSWORD=asdasd \
                -e MYSQL_ROOT_PASSWORD=asdasd \
                mariadb
                
$ docker run -d --name servidor_wp \
                --network red_wp \
                -v /opt/wordpress:/var/www/html/wp-content \
                -e WORDPRESS_DB_HOST=servidor_mysql \
                -e WORDPRESS_DB_USER=user_wp \
                -e WORDPRESS_DB_PASSWORD=asdasd \
                -e WORDPRESS_DB_NAME=bd_wp \
                -p 80:80 \
                wordpress

$ docker ps

```
![img1](/Docker/Images/act4/Screenshot_12.png)
![img1](/Docker/Images/act4/Screenshot_13.png)
![img1](/Docker/Images/act4/Screenshot_14.png)


![img1](/Docker/Images/act4/Screenshot_15.png)

---
