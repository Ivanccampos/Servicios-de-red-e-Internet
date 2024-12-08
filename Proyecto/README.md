## La arquitectura Web es un modelo compuesto de tres capas, ¿cuáles son y cuál es  la función de cada una de ellas?


La arquitectura Web se compone de tres capas fundamentales: la capa de presentación, la capa de aplicación y la capa de datos. Cada capa tiene una función específica y se comunica con las capas adyacentes mediante interfaces bien definidas.

Capa de Presentación: Esta capa se encarga de la interfaz de usuario, es decir, la parte visual y interactiva de la aplicación web. Su función es recibir entradas del usuario (por ejemplo, clics en botones o introducción de texto) y mostrar resultados en forma de contenido web (imágenes, texto, videos, etc.). La capa de presentación se comunica con la capa de aplicación a través de protocolos como HTTP y HTTPS.

Capa de Aplicación: Esta capa se ocupa de la lógica de negocio, es decir, la parte que procesa las solicitudes del usuario y las transforma en acciones efectivas. La capa de aplicación se comunica con la capa de presentación mediante interfaces de programación de aplicaciones (APIs) y con la capa de datos mediante protocolos de acceso a datos como SQL o NoSQL.

Capa de Datos: Esta capa se encarga del almacenamiento y gestión de los datos de la aplicación web. La capa de datos se comunica con la capa de aplicación mediante interfaces de acceso a datos y con la capa de presentación mediante consultas y recuperación de datos.
Una plataforma web es el entorno de desarrollo de software empleado para  diseñar y ejecutar un sitio web; destacan dos plataformas web, LAMP y WISA. Explica en qué consiste cada una de ellas.


## Las plataformas web LAMP y WISA son dos de las más populares y difundidas en el mercado, utilizadas para desarrollar sitios web dinámicos y aplicaciones web. A continuación, se describe cada una de ellas:



LAMP (Linux, Apache, MySQL, PHP)
La plataforma LAMP es una combinación de software de código abierto y gratuito. Está compuesta por:
Linux: Sistema operativo de código abierto.
Apache: Servidor web de código abierto y gratuito.
MySQL: Gestor de bases de datos relacional de código abierto y gratuito.
PHP: Lenguaje de programación interpretado de código abierto y gratuito.

La plataforma LAMP es conocida por ser escalable, segura y flexible, lo que la hace popular entre los desarrolladores web. Es ideal para aplicaciones web que requieren una gran cantidad de tráfico y procesamiento de datos.



WISA (Windows, Internet Information Services, SQL Server, ASP)
La plataforma WISA, por otro lado, se basa en tecnologías desarrolladas por Microsoft. Está compuesta por:
Windows: Sistema operativo de Microsoft.
Internet Information Services (IIS): Servidor web de Microsoft.
SQL Server: Gestor de bases de datos relacional de Microsoft.
ASP (Active Server Pages): Tecnología de Microsoft para desarrollar aplicaciones web dinámicas.

La plataforma WISA es conocida por ser más costosa que LAMP, pero ofrece un mayor nivel de soporte y es ideal para aplicaciones web que requieren una gran cantidad de recursos y procesamiento, como intranets o aplicaciones con un gran volumen de transacciones electrónicas.



## Instala la pila Linux, Apache, MySQL y PHP (LAMP) en Ubuntu

### Instalar Apache
Instale Apache usando el administrador de paquetes de Ubuntu, apt:

sudo apt update
sudo apt install apache2

![img1](/Actividad1/images/001.png)
![img1](/Actividad1/images/002.png)

Una vez que la instalación se complete, deberá ajustar la configuración de su firewall para permitir tráfico HTTP y HTTPS. UFW tiene diferentes perfiles de aplicaciones que puede aprovechar para hacerlo. Para enumerar todos los perfiles de aplicaciones de UFW disponibles, puede ejecutar lo siguiente:

![img1](/Actividad1/images/003.png)

Por ahora, es mejor permitir conexiones únicamente en el puerto 80, ya que se trata de una instalación nueva de Apache y todavía no tiene un certificado TLS/SSL configurado para permitir tráfico HTTPS en su servidor.

Para permitir tráfico únicamente en el puerto 80 utilice el perfil Apache:

![img1](/Actividad1/images/004.png)

Puede realizar una verificación rápida para comprobar que todo se haya realizado según lo previsto dirigiéndose a la dirección IP pública de su servidor en su navegador web (consulte la nota de la siguiente sección para saber cuál es su dirección IP pública si no dispone de esta información):

![img1](/Actividad1/images/005.png)

### Instalar MySQL

Ahora que dispone de un servidor web funcional, deberá instalar un sistema de base de datos para poder almacenar y gestionar los datos de su sitio. MySQL es un sistema de administración de bases de datos popular que se utiliza en entornos PHP.

Una vez más, utilice apt para adquirir e instalar este software:

![img1](/Actividad1/images/006.png)

Cuando la instalación se complete, se recomienda ejecutar una secuencia de comandos de seguridad que viene preinstalada en MySQL Con esta secuencia de comandos se eliminarán algunos ajustes predeterminados poco seguros y se bloqueará el acceso a su sistema de base de datos. Inicie la secuencia de comandos interactiva ejecutando lo siguiente:


![img1](/Actividad1/images/007.png)


Cuando termine, compruebe si puede iniciar sesión en la consola de MySQL al escribir lo siguiente:

![img1](/Actividad1/images/008.png)

Esto permitirá establecer conexión con el servidor de MySQL como root user de la base de datos administrativa, lo que se infiere del uso de sudo cuando se ejecuta este comando. Debería ver el siguiente resultado:

Para salir de la consola de MySQL, escriba lo siguiente:

mysql> exit

### Instalar PhP

Instaló Apache para presentar su contenido y MySQL para almacenar y gestionar sus datos. PHP es el componente de nuestra configuración que procesará el código para mostrar contenido dinámico al usuario final. Además del paquete php, necesitará php-mysql, un módulo PHP que permite que este se comunique con bases de datos basadas en MySQL. También necesitará libapache2-mod-php para habilitar Apache para gestionar archivos PHP. Los paquetes PHP básicos se instalarán automáticamente como dependencias.

Para instalar estos paquetes, ejecute lo siguiente:

![img1](/Actividad1/images/009.png)

Una vez que la instalación se complete, podrá ejecutar el siguiente comando para confirmar su versión de PHP:

![img1](/Actividad1/images/010.png)


### Crear un Host virtual para su sitio web

Ubuntu 20.04 tiene habilitado un bloque de servidor por defecto, que está configurado para proporcionar documentos del directorio /var/www/html. Si bien esto funciona bien para un solo sitio, puede ser difícil de manejar si aloja varios. En lugar de modificar /var/www/html, crearemos una estructura de directorio dentro de /var/www para el sitio your_domain y dejaremos /var/www/html establecido como directorio predeterminado que se presentará si una solicitud de cliente no coincide con ningún otro sitio.

Cree el directorio para your_domain de la siguiente manera:

![img1](/Actividad1/images/011.png)

A continuación, asigne la propiedad del directorio con la variable de entorno $USER, que hará referencia a su usuario de sistema actual:
![img1](/Actividad1/images/012.png)

Luego, abra un nuevo archivo de configuración en el directorio sites-available de Apache usando el editor de línea de comandos que prefiera. En este caso, utilizaremos nano:

![img1](/Actividad1/images/013.png)

De esta manera, se creará un nuevo archivo en blanco. Pegue la siguiente configuración básica:

![img1](/Actividad1/images/014.png)

Con esta configuración de VirtualHost, le indicamos a Apache que proporcione your_domain usando /var/www/your_domain como directorio root web. Si desea probar Apache sin un nombre de dominio, puede eliminar o convertir en comentario las opciones ServerName y ServerAlias añadiendo un carácter # al principio de las líneas de cada opción.

Ahora, puede usar a2ensite para habilitar el nuevo host virtual:

![img1](/Actividad1/images/015.png)

Puede ser conveniente deshabilitar el sitio web predeterminado que viene instalado con Apache. Es necesario hacerlo si no se utiliza un nombre de dominio personalizado, dado que, en este caso, la configuración predeterminada de Apache sobrescribirá su host virtual. Para deshabilitar el sitio web predeterminado de Apache, escriba lo siguiente:

![img1](/Actividad1/images/016.png)

Para asegurarse de que su archivo de configuración no contenga errores de sintaxis, ejecute lo siguiente:

![img1](/Actividad1/images/017.png)

Por último, vuelva a cargar Apache para que estos cambios surtan efecto:

![img1](/Actividad1/images/018.png)

Ahora, su nuevo sitio web está activo, pero el directorio root web /var/www/your_domain todavía está vacío. Cree un archivo index.html en esa ubicación para poder probar que el host virtual funcione según lo previsto:

![img1](/Actividad1/images/019.png)

Incluya el siguiente contenido en este archivo:

![img1](/Actividad1/images/020.png)

Ahora, diríjase a su navegador y acceda al nombre de dominio o la dirección IP de su servidor una vez más:

![img1](/Actividad1/images/021.png)

Verá una página como la siguiente:

![img1](/Actividad1/images/022.png)

Si ve esta página, su host virtual de Apache está funcionando según lo previsto.

### Nota sobre DirectoryIndex en Apache
Con la configuración predeterminada de DirectoryIndex en Apache, un archivo denominado index.html siempre tendrá prioridad sobre un archivo index.php. Esto es útil para establecer páginas de mantenimiento en aplicaciones PHP, dado que se puede crear un archivo index.html temporal que contenga un mensaje informativo para los visitantes. Como esta página tendrá precedencia sobre la página index.php, se convertirá en la página de destino de la aplicación. Una vez que el mantenimiento se completa, el archivo index.html se elimina del root de documentos, o se le cambia el nombre, para volver mostrar la página habitual de la aplicación.

Si desea cambiar este comportamiento, deberá editar el archivo /etc/apache2/mods-enabled/dir.conf y modificar el orden en el que el archivo index.php se enumera en la directiva 

DirectoryIndex:

sudo nano /etc/apache2/mods-enabled/dir.conf


/etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

![img1](/Actividad1/images/023.png)

![img1](/Actividad1/images/024.png)

Después de guardar y cerrar el archivo, deberá volver a cargar Apache para que los cambios surtan efecto:

sudo systemctl reload apache2
En el siguiente paso, crearemos una secuencia de comandos PHP para probar que PHP esté correctamente instalado y configurado en su servidor.


### Probar el procesamiento de PHP en su servidor web

Ahora que dispone de una ubicación personalizada para alojar los archivos y las carpetas de su sitio web, crearemos una secuencia de comandos PHP de prueba para verificar que Apache pueda gestionar solicitudes y procesar solicitudes de archivos PHP.

Cree un archivo nuevo llamado info.php dentro de su carpeta root web personalizada:

nano /var/www/your_domain/info.php

![img1](/Actividad1/images/025.png)

Con esto se abrirá un archivo vacío. Añada el siguiente texto, que es el código PHP válido, dentro del archivo:

/var/www/your_domain/info.php
<?php
phpinfo();

![img1](/Actividad1/images/026.png)

Cuando termine, guarde y cierre el archivo.

Para probar esta secuencia de comandos, diríjase a su navegador web y acceda al nombre de dominio o la dirección IP de su servidor, seguido del nombre de la secuencia de comandos, que en este caso es info.php:


Verá una página similar a la siguiente:

![img1](/Actividad1/images/027.png)

En esta página, se proporciona información básica sobre su servidor desde la perspectiva de PHP. Es útil para la depuración y para asegurarse de que sus ajustes se apliquen correctamente.

Si puede ver esta página en su navegador, su instalación de PHP funciona según lo previsto.

Tras comprobar la información pertinente sobre su servidor PHP a través de esa página, es recomendable que elimine el archivo que creó, dado que contiene información confidencial sobre su entorno PHP y su servidor de Ubuntu. Puede usar rm para hacerlo:

sudo rm /var/www/your_domain/info.php

![img1](/Actividad1/images/028.png)

Siempre puede recrear esta página si necesita acceder a la información posteriormente.