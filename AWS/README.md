 Práctica Servidores Web - 1º Trimestre


## Cómo crear un certificado SSL autofirmado para Apache

### Crear el certificado SSL

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
5. Instalamos mysql
`````
sudo apt install mysql-server
`````
`````
sudo mysql_secure_installation
`````
6. Crear un par de clave y certificado autofirmados con OpenSSL:
````
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
````

Los dos archivos que creó se ubicarán en los subdirectorios correspondientes en /etc/ssl.

### Configurar Apache para usar SSL

Aplicaremos algunos ajustes a nuestra configuración:

Crearemos un fragmento de configuración para especificar configuraciones SSL seguras predeterminadas.
Modificaremos el archivo de host virtual de Apache SSL incluido para apuntar a los certificados SSL que generamos.
Modificaremos el archivo de host virtual no cifrado para redireccionar las solicitudes de forma automática al host virtual cifrado.

1. Crear un fragmento de configuración de Apache con ajustes de cifrado seguro
`````
sudo nano /etc/apache2/conf-available/ssl-params.conf
`````
2. Modificar el archivo de host virtual de Apache SSL predeterminado
`````
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
`````
Haremos algunos ajustes menores en el archivo. Configuraremos las cosas normales que querríamos ajustar en un archivo de host virtual (dirección de correo electrónico de ServerAdmin, ServerName, etc.), y ajustaremos las directivas SSL para que apunten a nuestros archivos de certificados y claves.

`````
sudo nano /etc/apache2/sites-available/default-ssl.conf
`````
3. Modificar el archivo de host HTTP para el redireccionamiento a HTTPS
`````
sudo nano /etc/apache2/sites-available/000-default.conf
`````

### Ajustar el firewall
`````
sudo ufw app list
`````
`````
sudo ufw status
`````

Para permitir la entrada de tráfico HTTPS, podemos permitir el perfil “Apache Full” y luego eliminar la asignación de perfil “Apache” redundante:
`````
sudo ufw allow 'Apache Full'
sudo ufw delete allow 'Apache'
`````

### Habilitar los cambios en Apache

Ahora que realizamos los cambios y ajustamos el firewall, podemos habilitar los módulos y encabezados SSL de Apache, y también nuestro host virtual listo para SSL, y reiniciar Apache.

`````
sudo a2enmod ssl
sudo a2enmod headers
`````
`````
sudo a2ensite default-ssl
`````
`````
sudo a2enconf ssl-params
`````

En este punto, nuestro sitio y los módulos necesarios quedarán habilitados. Deberíamos comprobar que no haya errores de sintaxis en nuestros archivos. Podemos hacerlo escribiendo lo siguiente:
`````
sudo apache2ctl configtest
`````

`````
sudo systemctl restart apache2
`````

### Probar el cifrado

Ahora, estamos listos para probar nuestro servidor SSL.

Abre el navegador web y escribe https:// seguido del nombre de dominio o el IP de su servidor:

`````
https://server_domain_or_IP
`````
Debido a que el certificado que creamos no está firmado por una de las autoridades de certificados de confianza de su navegador, es probable que se vea una advertencia


### Cambiar a una redireccionamiento permanente

Si su redireccionamiento funcionó de forma correcta y está seguro que quiere permitir solo tráfico cifrado, deberá modificar el host virtual de Apache no cifrado de nuevo para que el redireccionamiento sea permanente. 


`````
sudo nano /etc/apache2/sites-available/000-default.conf
`````
`````
sudo apache2ctl configtest
`````

`````
sudo systemctl restart apache2
`````

<br>

<br>
