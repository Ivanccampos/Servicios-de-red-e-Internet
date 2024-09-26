# Actividad 0.1

## Descargar python y ejecutar localhost

Descargamos python para nuestro equipo, activamos el servidor de Apache mediante el programa XAMPP y escribimos en nuestro buscador "localhost" o "127.0.0.1".

Si está hecho de forma correcta podemos ver que se ha creado un servidor y que accedemos a la página de Apache.

![img1](/Actividad0/imagenes/index.png)

Si no hemos activado Apache previo a acceder al servidor, la página web por defecto nos mostrará nuestra carpeta de usuario.

![img1](/Actividad0/imagenes/error.png)

# Actividad 0.2

## Crear un server en python y compartirlo en clase

Vamos a crear un pequeño archivo html y lo vamos a guardar en la carpeta de usuario de nuestro equipo.

![img2](/Actividad0/imagenes/index.png)

Un vex creado el archivo podemos activar Apache y crear un servidor para mostrar la página mediante el comando 
```
server.py
´´´
 ó 
 
 ```
 python -m http.server 8000
 ´´´
Por defecto "server.py" nos asigna el puerto 8000.

![img3](/Actividad0/imagenes/create.png)

Cuando accedemos al buscador y si buscamos "localhost:8000" podemos ver nuestra página web.

![img4](/Actividad0/imagenes/check.png)

Esta página puede ser visualizada por otros usuarios si escribien en su buscador ---------- .