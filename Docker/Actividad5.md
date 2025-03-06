# Docker. Práctica 5



## Lleva a cabo la práctica descrita

### Ejemplo 1: Despliegue de la aplicación Guestbook

En este ejemplo vamos a desplegar con Docker Compose la aplicación *guestbook*.

En el fichero `docker-compose.yaml` vamos a definir el escenario. El comando `docker compose` se debe ejecutar en el directorio donde este ese fichero. 

```yaml
version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    environment:
      REDIS_SERVER: redis
    ports:
      - 8080:5000
  db:
    container_name: redis
    image: redis
    restart: always
    command: redis-server --appendonly yes
    volumes:
      - redis:/data
volumes:
  redis:
```
![img1](/Docker/Images/act5/Screenshot_1.png)

Veamos algunas observaciones:

Para crear el escenario:

```
$ docker compose up -d
```

![img1](/Docker/Images/act5/Screenshot_2.png)

Para listar los contenedores:

```
$ docker compose ps
```
![img1](/Docker/Images/act5/Screenshot_3.png)

Para parar los contenedores:

```
$ docker compose stop
```


![img1](/Docker/Images/act5/Screenshot_5.png)

Para eliminar el escenario:

```
$ docker compose down
```

Para eliminar también el volumen usaremos `docker compose down -v`.

![img1](/Docker/Images/act5/Screenshot_6.png)

---
![img1](/Docker/Images/act5/Screenshot_4.png)

### Ejemplo 2: Despliegue de la aplicación Temperaturas

En este ejemplo vamos a desplegar con Docker Compose la aplicación *Temperaturas*.

En este caso el fichero `docker-compose.yaml` puede tener esta forma:

```yaml
version: '3.1'
services:
  frontend:
    container_name: temperaturas-frontend
    image: iesgn/temperaturas_frontend
    restart: always
    ports:
      - 8081:3000
    environment:
      TEMP_SERVER: temperaturas-backend:5000
    depends_on:
      - backend
  backend:
    container_name: temperaturas-backend
    image: iesgn/temperaturas_backend
    restart: always
```

Como hicimos en el ejemplo anterior, aunque no es necesario porque es valor por defecto, declaramos la variable de entorno `TEMP_SERVER: temperaturas-backend:5000`. Como indicábamos también, podríamos uso del nombre del servicio, de esta manera quedaría como `TTEMP_SERVER: backend:5000`.

Para crear el escenario:

```
$ docker compose up -d
```
![img1](/Docker/Images/act5/Screenshot_7.png)
Para listar los contenedores:

```
$ docker compose ps
```
---
![img1](/Docker/Images/act5/Screenshot_8.png)
![img1](/Docker/Images/act5/Screenshot_9.png)


### Ejemplo 3: Despliegue de WordPress + Mariadb

En este ejemplo vamos a desplegar con Docker Compose la aplicación WordPress + MariaDB.


Para la ejecución de wordpress persistente con volúmenes docker podríamos tener un fichero `docker-compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - wordpress_data:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    wordpress_data:
    mariadb_data:
```

Para la ejecución de wordpress persistente con bind mount podríamos tener un fichero `docker-compose.yaml` con el siguiente contenido:

```yaml
version: '3.1'
services:
  wordpress:
    container_name: servidor_wp
    image: wordpress
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: asdasd
      WORDPRESS_DB_NAME: bd_wp
    ports:
      - 80:80
    volumes:
      - ./wordpress:/var/www/html/wp-content
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: bd_wp
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - ./mysql:/var/lib/mysql
```


Para crear el escenario:

```
$ docker compose up

```
![img1](/Docker/Images/act5/Screenshot_10.png)
Para listar los contenedores:

```
$ docker compose ps
```
![img1](/Docker/Images/act5/Screenshot_11.png)
![img1](/Docker/Images/act5/Screenshot_12.png)

