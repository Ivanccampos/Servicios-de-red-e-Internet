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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_01.png)

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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_40.png)




Continuamos con la configuración, añadimos nombre a nuestro sitio, correo electrónico y contraseña de administrador.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_41.png)


Habremos finalizado la configuración básica de Wordpress.

Accedemos a la página de inicio de Wordpress e ingresamos con nuestras credenciales.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_42.png)


Finalmente comprobamos el correcto funcionamiento de Wordpress.

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_43.png)


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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_61.png)

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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_65.png)

<br>
<br>

## Instalación de awstats

1. Abrimos la terminal

2. Instalamos el paquete awstats
`````
sudo apt install awstats
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_2.png)


3. Habilitar el módulo CGI en Apache

`````
sudo a2enmod cgi
sudo a2enconf awstats
`````

Y reiniciamos apache2

`````
sudo systemctl restart apache2
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_4.png)

4. Crear el archivo de configuración awstats

`````
cp /etc/awstats/awstats.conf /etc/awstats/awstats"dominio".conf
sudo nano /etc/awstats/awstats"dominio".conf
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_8.png) 

Y modificamos LogFile y HostAliases

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_6.png)
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_7.png)

5. Crear el archivo con las estadísiticas iniciales.
`````
sudo /usr/lib/cgi-bin/awstats.pl -config="dominio" -update
````` 
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_10.png)

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_9.png)

6. Configurar Apache para awstats

`````
sudo cp -r /usr/lib/cgi-bin /var/www/html/
sudo chown -R www-data:www-data /var/html/cgi-bin/
sudo chmod 755 /var/www/html/cgi-bin/

`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_11.png)

7. Acceder a la página

`````
http://ip-servidor/cgi-bin/awstats.pl?config="dominio"
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/awstats/Screenshot_12.png)


## Instalación de nginx y phpmyadmin

### Instalación nginx


![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_66.png)
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_67.png)
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_68.png)
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_69.png)


### Configuracion nginx

1. Cambiar la configuración del dominio en Nginx
Editar el archivo de configuración del servidor virtual:

`````
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_70.png)

2. Configura el archivo para el puerto 8080 y PHP:

Copia y pega la siguiente configuración en el archivo:
`````
server {
    listen 8080;
    server_name servidor2.centro.intranet;

    root /var/www/servidor2; # Ruta al directorio raíz del sitio
    index index.php index.html index.htm;

    # Configuración para manejar PHP
    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock; # Cambiar a la versión de PHP instalada
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_71.png)
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_72.png)

3. Crear el directorio raíz del sitio:

`````
sudo mkdir -p /var/www/servidor2
sudo chown -R www-data:www-data /var/www/servidor2
sudo chmod -R 755 /var/www/servidor2
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_73.png)
4. Habilitar la configuración del sitio:

Si el archivo está en sites-available, habilítalo con un enlace simbólico:

`````
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_74.png) 

5. Probar la configuración de Nginx:

`````
sudo nginx -t
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_75.png)

Si no hay errores, recarga Nginx:

`````
sudo systemctl reload nginx
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_76.png)

### Configurar PHP-FPM

1. Asegúrate de tener instalado PHP-FPM:


`````
sudo apt install php-fpm
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_77.png) 

2. Verifica la versión instalada de PHP y el socket correspondiente:


`````
ls /var/run/php/
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_78.png)

3. Ajusta el archivo de configuración de PHP-FPM para que trabaje con Nginx:

`````
sudo nano /etc/php/8.1/fpm/php.ini
`````

Busca y asegúrate de que las siguientes opciones estén activas:

`````
cgi.fix_pathinfo=0
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_79.png)

Reinicia PHP-FPM para aplicar los cambios:


`````
sudo systemctl restart php8.1-fpm
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_80.png) 

### Configurar el dominio en el sistema

Para acceder al dominio servidor2.centro.intranet, agrega una entrada al archivo /etc/hosts en el cliente o servidor de pruebas (si es local):

1. Edita el archivo /etc/hosts:
`````
sudo nano /etc/hosts
`````
2. Agrega la siguiente línea:
`````
127.0.0.1 servidor2.centro.intranet
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_81.png)

### Probar la configuración
Coloca un archivo PHP de prueba en el directorio del sitio:

`````
echo "<?php phpinfo(); ?>" | sudo tee /var/www/servidor2/index.php
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_82.png)

Accede al sitio en el navegador:

Visita http://servidor2.centro.intranet:8080 y deberías ver la página de información de PHP.


![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_83.png)

### Instalar phpmyadmin
1. Teclea el siguiente comando:
`````
sudo apt-get install phpmyadmin
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_85.png)

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_86.png)

2. Reinicia los servicios de apache

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/main/Proyecto/proyecto_img/Screenshot_87.png)