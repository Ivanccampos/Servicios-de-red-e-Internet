 Práctica Servidores Web - 1º Trimestre


## Instalación del servidor web Apache

### Instalación de Apache en Ubuntu

1. Abrimos la terminal

2. Actualizamos los paquetes instalados
`````
sudo apt update
`````
3. Instalamos las nuevas versiones
````
sudo apt upgrade
````
4. Instalamos el paquete de Apache
`````
sudo apt install apache2
`````
5. Ajustamos el Firewall para Apache con los siguientes dos comandos:
`````
sudo ufw app list
`````
`````
sudo ufw allow "Apache"
`````
6. Comprobar en nuestro navegador la siguiente dirección
````
http://localhost
````
Si hemos seguido los pasos, habremos instalado correctamente Apache2 para Ubuntu

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_01.png)

<br>

<br>


## Activación de los módulos necesarios para PhP y MySQL

### Instalación de MySQL

1. Instalamos MySQL-server

`````
sudo apt install mysql-server
`````
2. Comprobamos, entrando a MySQL con el siguiente comando:
`````
sudo mysql 
`````
3. Para salir de MySQL:
`````
exit
`````
<br>

## Instalación y configuración Wordpress

### Instalamos las dependencias para Wordpress

Ejecutamos los siguientes comandos:

`````
sudo apt install -y php8.1-cli php8.1-fpm php8.1-mysql
`````

### Instalamos Wordpress

1. Descargamos el archivo de instalación de Wordpress:

````
wget https://es.wordpress.org/latest.zip
````
 2. Movemos los contenidos de Wordpress a la carpeta del dominio

````
sudo mv latest.tar.gz /var/www
````

3. Extraemos los archivos

````
sudo tar -xzvf latest.tar.gz
````


4. Cambiamos los permisos de la carpeta

````
sudo chown -R www-data:www-data /var/www/html/*
````

````
sudo chmod -R 755 wordpress
````

<br>

<br>

### Creación y modificación de un archivo de configuración para Wordpress

1. Creamos el archivo de configuración

````
sudo nano /etc/apache2/sites-available/wordpress.conf
````
2. Editamos con lo siguiente:

````
<VirtualHost *:80>
    ServerName localhost
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/wordpress
</VirtualHost>
````

<br>

3. Habilitamos la página de Wordpress

<br>

````
sudo a2ensite wordpress
````

<br>

4. Recargamos Apache

<br>

````
sudo service Apache2 reload
````

<br>

### Configuración de la base de datos
<br>

Accedemos a MySQL como administradrores

````
sudo mysql -u root
````
<br>

Creamos nuestra base datos, en este caso el nombre será wordpress
````
CREATE DATABASE wordpress;
````
<br>
Creamos un usuario admin para MySQL 

````
CREATE USER 'admin'@'localhost'  IDENTIFIED  BY 'admin';
````
<br>
Otorgamos todos los privilegios al usuario admin

````
GRANT ALL PRIVILEGES -> ON wordpress.* -> TO 'admin'@'localhost';
````
<br>
Actualizamos los privilegios de nuestra base de datos

````
FLUSH PRIVILEGES;
````
<br>
Cerramos la sesión de MySQL

````
quit
````
<br>


<br>

### Configurar la conexión entre Wordpress y la base de datos

<br>

Copiamos la configuracion por defecto de wordpress en el archivo wp-config-sample.php a wp-config.php

````
sudo -u www-data cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
````

Modificamos el archivo de configuración de WordPress para que se conecte a la base de datos que creamos anteriormente.

````
sudo -u www-data nano /srv/www/wordpress/wp-config.php
````

Dentro del documento , debemos cambiar los siguientes valores por aquellos que definimos en nuestra base de datos:

````
define( 'DB_NAME',          'Nombre_Base_Datos' );
define( 'DB_USER',          'Nombre_Usuario' );
define( 'DB_PASSWORD',      'Contraseña' );
````

<br>
<br>

### Configuración de Wordpress

Ingresamos a nuestro dominio y comenzamos la configuración básica de Wordpress.
Seleccionamos el idioma.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_40.png)




Continuamos con la configuración, añadimos nombre a nuestro sitio, correo electrónico y contraseña de administrador.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_41.png)


Habremos finalizado la configuración básica de Wordpress.

Accedemos a la página de inicio de Wordpress e ingresamos con nuestras credenciales.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_42.png)


Finalmente comprobamos el correcto funcionamiento de Wordpress.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_43.png)



<br>

### Instalación de Apache en Ubuntu

## Activación del módulo 'WSGI' para aplicaciones Python

1. Instalamos Python:
````
sudo apt install python3 libexpat1 -y
````

2. Instalamos el módulo WSGI
````
sudo apt install libapache2-mod-wsgi-py3 -y
````

<br>
<br>

## Utilizando aplicaciones Python

1. Probamos el módulo con un script de prueba:

````
sudo nano /var/www/html/wsgi_prueba.py
````

2. Agregamos el siguiente código al archivo wsgi_prueba.py:
````Python
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello !\n'

    response_headers = [
        ('Content-type', 'text/plain'),
        ('Content-Length', str(len(output)))
    ]

    start_response(status, response_headers)
    return [output]
````

3. Cambiamos el dueño y permisos del archivo

````
sudo chown www-data:www-data /var/www/html/wsgi_prueba.py
sudo chmod 775 /var/www/html/wsgi_prueba.py
````

4. En el archivo de configuración de nuestro VirtualHost añadimos:
````
WSGIScriptAlias / /var/www/html/wsgi_prueba.py
````

5.  Ingresamos al dominio añadiendo al  final /wsgi

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_61.png)

<br>
<br>

## Medidas de seguridad al acceso de la aplicación Python

1. Utilizaremos el módulo auth_basic, para empezar a utilizar este módulo debemos habilitarlo, primero para ello escribimos en una terminal lo siguiente:
````
sudo a2enmod auth_basic
````

2. Definimos primero un archivo donde se almacene la información relativa de los usuarios, entonces desde una terminal escribimos:

````
sudo htpasswd -c /etc/apache2/.htpasswd usuario
````

3. Ingresamos al archivo de configuración del VirtualHost:
````
sudo nano /etc/apache2/sites-available/nombre-dominio.conf
````

4. Agregamos la siguiente línea al archivo de configuración:
````
<Location /wsgi>
  AuthType Basic
  AuthName "Nombre_Opcional"
  AuthUserFile /etc/apache2/.htpsswd
  Require valid-user
</Location>
````

5. Comprobamos que no podemos acceder al módulo WSGI

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/31892d2ceb2d5c4d6d567a28fd9fb2892a5c2611/Proyecto/proyecto_img/Screenshot_65.png)

<br>
<br>
