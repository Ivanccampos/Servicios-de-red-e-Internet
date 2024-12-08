
Vamos a instalar un servidor web interno para un instituto. Se Pide:
●	Instalación del servidor web apache. Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en python
●	Activar los módulos necesarios para ejecutar php y acceder a mysql
●	Instala y configura wordpress

Instalar y configurar el servidor web Apache
El primer paso para configurar el stack de LAMP es instalar y configurar el servidor Apache. En primer lugar, tenemos que actualizar la lista de paquetes de tu sistema y actualizar los paquetes a la versión más reciente

sudo apt update

Ahora es el momento de instalar el servidor web Apache2

sudo apt install apache2


Instalar PHP
PHP es necesario para que WordPress se comunique con la base de datos MySQL y muestre contenido dinámico. También necesitarás instalar extensiones PHP adicionales para WordPress.
Ejecuta el siguiente comando para instalar PHP y las extensiones de PHP a la vez:


sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-xml php-mbstring php-xmlrpc php-zip php-soap php-intl -y


Configurar MySQL y crear una base de datos
Una vez que tengas el servidor web en funcionamiento, puedes instalar la base de datos MySQL.


apt install mysql-server -y

Después de instalar MySQL en tu VPS, abre el terminal de MySQL escribiendo el siguiente comando:

sudo mysql

Puedes establecer una contraseña para el usuario root mediante el siguiente comando:

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH 
mysql_native_password BY 'Password';


Para reflejar estos cambios, usa el comando Flush como se muestra a continuación:
mysql> FLUSH PRIVILEGES;
Utiliza el siguiente comando para crear una base de datos de WordPress:
mysql> CREATE DATABASE WordPressBD DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
Ahora, crearemos una cuenta de usuario MySQL para operar en la nueva base de datos de WordPress. Utilizaremos WordPressDB como nombre de la base de datos y UsuarioWordPress como nombre de usuario. Para crear un nuevo usuario y otorgar privilegios usa el siguiente comando:
GRANT ALL ON WordPressBD.* TO ' UsuarioWordPress '@'localhost' IDENTIFIED BY 'NuevaContraseña';
Proporciona una contraseña segura en lugar de NuevaContraseña. Para reflejar estos cambios, usa el comando:
mysql> FLUSH PRIVILEGES;
Una vez que hayas terminado, sal de la pantalla de MySQL escribiendo este comando:
mysql> EXIT;





4. Prepárate para instalar WordPress en Ubuntu
Es el momento de preparar la instalación de WordPress creando un archivo de configuración de WordPress y un directorio de WordPress.
Creando un archivo de WordPress.conf
Crea un archivo de configuración, por ejemplo: WordPress.conf. Colócalo en /etc/apache2/sites-available/. Esta será una réplica del archivo de configuración predeterminado que ya existe en esta ubicación. Utiliza el siguiente comando:
nano /etc/apache2/sites-available/WordPress.conf
Una vez que ejecutes ese comando, accederás al editor de texto Nano para editar el archivo WordPress.conf. Activa el .htaccess añadiendo estas líneas al bloque VirtualHost:
&lt;Directory /var/www/wordpress /&gt;
&nbsp;&nbsp;AllowOverride All
&lt;/Directory&gt;
Cierra y guarda el archivo pulsando CTRL+X. Pulsa Y y Enter cuando se te pida.
Crear un directorio para WordPress
A continuación, crea un directorio para WordPress en /var/www/. En nuestro ejemplo, su ruta completa será /var/www/wordpress. Para ello, utiliza el comando mkdir para crear el directorio:
mkdir /var/www/wordpress
Ahora, activa el mod_rewrite para utilizar la función de permalink o enlace permanente de WordPress ejecutando el siguiente comando en el terminal:
sudo a2enmod rewrite
Tendrás que reiniciar el servidor web Apache utilizando el siguiente comando:
systemctl restart apache2
El siguiente paso es cambiar la directiva ServerName en el archivo /etc/apache2/apache2.conf. Abre el archivo con este comando:
nano /etc/apache2/apache2.conf
Tendrás que configurar la directiva ServerName con la dirección IP o el nombre del servidor añadiendo la siguiente línea al archivo /etc/apache2/apache2.conf:
ServerName <Your IP Address>
Cierra y guarda el archivo.
Ahora, tienes que comprobar si la configuración de Apache es correcta ejecutando el siguiente comando en el terminal:
apachectl configtest
Si la configuración funciona bien, debería imprimir la siguiente salida:
Syntax OK











●	Activar el módulo “wsgi” para permitir la ejecución de aplicaciones Python
●	Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.
●	Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación



•  Instalar el módulo WSGI para Apache
Para Python 3, instala el paquete libapache2-mod-wsgi-py3:
bash
Copiar código
sudo apt update
sudo apt install libapache2-mod-wsgi-py3
•  Activar el módulo WSGI
Una vez instalado, habilita el módulo WSGI usando el siguiente comando:
bash
Copiar código
sudo a2enmod wsgi
•  Reiniciar Apache
Para que los cambios surtan efecto, reinicia el servidor Apache:
bash
Copiar código
sudo systemctl restart apache2
•  Configurar Apache para tu aplicación Python
Edita el archivo de configuración de tu sitio (por ejemplo, /etc/apache2/sites-available/tu-sitio.conf) para incluir la configuración WSGI. Por ejemplo:
apache
Copiar código
<VirtualHost *:80>
    ServerName tu-dominio.com
    DocumentRoot /ruta/a/tu/proyecto

    # Archivo WSGI para tu aplicación
    WSGIScriptAlias / /ruta/a/tu/proyecto/tuapp.wsgi

    <Directory /ruta/a/tu/proyecto>
        Require all granted
    </Directory>

    # Configuración del entorno Python
    WSGIDaemonProcess tuapp python-path=/ruta/a/tu/proyecto python-home=/ruta/a/entorno/virtual
    WSGIProcessGroup tuapp
</VirtualHost>
•	WSGIScriptAlias apunta al archivo .wsgi de tu aplicación (ver ejemplo más abajo).
•	python-path indica el directorio raíz del proyecto.
•	python-home se usa si tienes un entorno virtual configurado.
•  Crear un archivo WSGI para tu aplicación
Este archivo será el punto de entrada para tu aplicación. Un ejemplo básico es:
python
Copiar código
import sys
sys.path.insert(0, "/ruta/a/tu/proyecto")

from tuapp import app as application
•	Reemplaza tuapp y app según tu aplicación.
•  Verificar la configuración
Revisa que no haya errores en la configuración de Apache:
bash
Copiar código
sudo apachectl configtest
Si todo está bien, reinicia Apache nuevamente:
bash
Copiar código
sudo systemctl restart apache2




●	Instala y configura awstat.
●	Instala un segundo servidor de tu elección (nginx, lighttpd) bajo el dominio “servidor2.centro.intranet”. Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar php. Instala phpmyadmin.
