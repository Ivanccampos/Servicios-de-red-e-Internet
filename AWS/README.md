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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_4.png)
<br>

5. Instalamos mysql
`````
sudo apt install mysql-server
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_5.png)

`````
sudo mysql_secure_installation
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_6.png)


6. Crear un par de clave y certificado autofirmados con OpenSSL:
````
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_8.png)

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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_9.png)


2. Modificar el archivo de host virtual de Apache SSL predeterminado
`````
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
`````
Haremos algunos ajustes menores en el archivo. Configuraremos las cosas normales que querríamos ajustar en un archivo de host virtual (dirección de correo electrónico de ServerAdmin, ServerName, etc.), y ajustaremos las directivas SSL para que apunten a nuestros archivos de certificados y claves.

`````
sudo nano /etc/apache2/sites-available/default-ssl.conf
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_11.png)


3. Modificar el archivo de host HTTP para el redireccionamiento a HTTPS
`````
sudo nano /etc/apache2/sites-available/000-default.conf
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_12.png)


### Ajustar el firewall
`````
sudo ufw app list
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_13.png)

`````
sudo ufw status
`````

Para permitir la entrada de tráfico HTTPS, podemos permitir el perfil “Apache Full” y luego eliminar la asignación de perfil “Apache” redundante:
`````
sudo ufw allow 'Apache Full'
sudo ufw delete allow 'Apache'
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_14.png)

### Habilitar los cambios en Apache

Ahora que realizamos los cambios y ajustamos el firewall, podemos habilitar los módulos y encabezados SSL de Apache, y también nuestro host virtual listo para SSL, y reiniciar Apache.

`````
sudo a2enmod ssl
sudo a2enmod headers
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_15.png)

`````
sudo a2ensite default-ssl
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_17.png)

`````
sudo a2enconf ssl-params
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_16.png)


En este punto, nuestro sitio y los módulos necesarios quedarán habilitados. Deberíamos comprobar que no haya errores de sintaxis en nuestros archivos. Podemos hacerlo escribiendo lo siguiente:
`````
sudo apache2ctl configtest
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_18.png)

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

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_19.png)

### Cambiar a una redireccionamiento permanente

Si su redireccionamiento funcionó de forma correcta y está seguro que quiere permitir solo tráfico cifrado, deberá modificar el host virtual de Apache no cifrado de nuevo para que el redireccionamiento sea permanente. 


`````
sudo nano /etc/apache2/sites-available/000-default.conf
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/aws_img/Screenshot_20.png)

`````
sudo apache2ctl configtest
`````

`````
sudo systemctl restart apache2
`````

<br>

<br>

## Ajustar la autenticación MySQL


En los sistemas Ubuntu con MySQL 5.7 (y versiones posteriores), el usuario root de MySQL se configura para la autenticación usando el complemento auth_socket de manera predeterminada en lugar de una contraseña. Esto en muchos casos proporciona mayor seguridad y utilidad, pero también puede generar complicaciones cuando deba permitir que un programa externo (como phpMyAdmin) acceda al usuario.

Para usar una contraseña para conectar con MySQL como root, deberá cambiar su método de autenticación de auth_socket a otro complemento, como caching_sha2_password o mysql_native_password. Para hacer esto, abra la consola de MySQL desde su terminal:

`````
sudo mysql

`````

A continuación, compruebe con el siguiente comando el método de autenticación utilizado por una de sus cuentas de usuario de MySQL:

`````
SELECT user,authentication_string,plugin,host FROM mysql.user;

`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/autenticacion/Screenshot_1.png)

En este ejemplo, puede ver que, en efecto, el usuario root se autentica utilizando el complemento de auth_socket. Para configurar la cuenta root para autenticar con una contraseña, ejecute una instrucción ALTER USER para cambiar qué complemento de autenticación utiliza y establecer una nueva contraseña.

`````
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';

`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/autenticacion/Screenshot_2.png)

A continuación, ejecute FLUSH PRIVILEGES para indicar al servidor que vuelva a cargar la tabla de permisos y aplique sus nuevos cambios:

`````
FLUSH PRIVILEGES;
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/autenticacion/Screenshot_3.png)
Compruebe de nuevo los métodos de autenticación empleados por cada uno de sus usuarios para confirmar que root deje de realizarla usando el complemento de auth_socket:
`````
SELECT user,authentication_string,plugin,host FROM mysql.user;
`````
![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/autenticacion/Screenshot_4.png)
Puede ver en este resultado de ejemplo que el root user de MySQL ahora autentica usando caching_sha2_password. Una vez que confirme esto en su propio servidor, podrá cerrar el shell de MySQL:

`````
mysql> exit

`````
 Si tiene la autenticación por contraseña habilitada para root deberá usar un comando diferente para acceder al shell de MySQL. A través de lo siguiente, se ejecutará su cliente de MySQL con privilegios de usuario regulares y solo obtendrá privilegios de administrador dentro de la base de datos mediante la autenticación:

`````
mysql -u root -p
`````

![img1](https://github.com/Ivanccampos/Servicios-de-red-e-Internet/blob/86b11fc8c59f5162ed1d8f147f54630acf1e5243/AWS/autenticacion/Screenshot_5.png)
