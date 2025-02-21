
# Práctica Wordpress en AWS

---

##  Objetivo

Instalación de Wordpress en instancia Ubuntu EC2 con soporte de base de datos RDS y EFS

---

### 1. Creación de la VPC

Iniciamos el laboratorio de AWS:

![img1](/TEMA_3_S5/AWS/Screenshot_1.png)

Entramos al laboratorio:

Vamos al apartado de VPC y la creamos:

![img1](/TEMA_3_S5/AWS/Screenshot_2.png)

![img1](/TEMA_3_S5/AWS/Screenshot_3.png)

Configuramos la VPC:

![img1](/TEMA_3_S5/AWS/Screenshot_4.png)

![img1](/TEMA_3_S5/AWS/Screenshot_5.png)

![img1](/TEMA_3_S5/AWS/Screenshot_6.png)

Ahora la VPC se empieza a crear:

![img1](/TEMA_3_S5/AWS/Screenshot_7.png)

---

### 2. Creación de las máquinas EC2 en la subred publica y privada

Vamos a la zona de creación de máquinas EC2:

![img1](/TEMA_3_S5/AWS/Screenshot_8.png)

Creamos la máquina para la red pública:

![img1](/TEMA_3_S5/AWS/Screenshot_9.png)

Elegimos el sistema operativo:

![img1](/TEMA_3_S5/AWS/Screenshot_10.png)

Asignamos el par de claves para conexión con ssh

![img1](/TEMA_3_S5/AWS/Screenshot_11.png)

La asignamos a la VPC y subred publica 1:

![img1](/TEMA_3_S5/AWS/Screenshot_12.png)

Creamos las reglas de firewall:

![img1](/TEMA_3_S5/AWS/Screenshot_13.png)

Creamos la máquina:

![img1](/TEMA_3_S5/AWS/Screenshot_14.png)

---

### 3. Instalación de Apache y php en la instancia EC2

Instalamos la clave ppk para conexión con la máquina EC2:

![img1](/TEMA_3_S5/AWS/Screenshot_14.png)

Copiamos el nombre DNS de la máquina:

![img1](/TEMA_3_S5/AWS/Screenshot_15.png)

Conectamos con ssh por Putty:

![img1](/TEMA_3_S5/AWS/Screenshot_16.png)

![img1](/TEMA_3_S5/AWS/Screenshot_17.png)

![img1](/TEMA_3_S5/AWS/Screenshot_18.png)

![img1](/TEMA_3_S5/AWS/Screenshot_19.png)

Nos conectamos con el usuario Ubuntu:

![img1](/TEMA_3_S5/AWS/Screenshot_20.png)

Realizamos un update y upgrade:

![img1](/TEMA_3_S5/AWS/Screenshot_21.png)


Instalamos las apache y php:

![img1](/TEMA_3_S5/AWS/Screenshot_22.png)

![img1](/TEMA_3_S5/AWS/Screenshot_23.png)

Reiniciamos apache2:

![img1](/TEMA_3_S5/AWS/Screenshot_24.png)

---

### 4. Creación de la base de datos

Vamos al apartado de RDS:

![img1](/TEMA_3_S5/AWS/Screenshot_25.png)

Creamos la base de datos:

![img1](/TEMA_3_S5/AWS/Screenshot_26.png)

Asignamos el tipo de base de datos MYSQL:

![img1](/TEMA_3_S5/AWS/Screenshot_27.png)

Elegimos la capa gratuita:

![img1](/TEMA_3_S5/AWS/Screenshot_28.png)

Elegimos las credenciales del administrador y que sea autoadministrado:

![img1](/TEMA_3_S5/AWS/Screenshot_29.png)

Asignamos la configuracion de la instancia y almacenamiento:

![img1](/TEMA_3_S5/AWS/Screenshot_30.png)

Configuramos la conectividad de la base de datos:

![img1](/TEMA_3_S5/AWS/Screenshot_31.png)

![img1](/TEMA_3_S5/AWS/Screenshot_32.png)

Creamos la base de datos:

![img1](/TEMA_3_S5/AWS/Screenshot_33.png)


Configuramos la conexión con la instancia EC2:

![img1](/TEMA_3_S5/AWS/Screenshot_34.png)

![img1](/TEMA_3_S5/AWS/Screenshot_35.png)

![img1](/TEMA_3_S5/AWS/Screenshot_36.png)

![img1](/TEMA_3_S5/AWS/Screenshot_37.png)

### 5. Creación de sistema de archivos EFS

Vamos al apartado de EFS:

![img1](/TEMA_3_S5/AWS/Screenshot_38.png)

Creamos el EFS:

![img1](/TEMA_3_S5/AWS/Screenshot_39.png)

![img1](/TEMA_3_S5/AWS/Screenshot_40.png)

Asignamos la regla de entrada NFS y MYSQL al grupo de seguridad al que se unen el EFS, el RDS y el EC2:

![img1](/TEMA_3_S5/AWS/Screenshot_59.png)

---

### 6. Instalación de wordpress

Descargamos el comprimido de wordpress:

![img1](/TEMA_3_S5/AWS/Screenshot_53.png)

Extraemos el comprimido:

![img1](/TEMA_3_S5/AWS/Screenshot_54.png)

Instalamos el cliente MYSQL:

![img1](/TEMA_3_S5/AWS/Screenshot_57.png)

Nos conectamos al sistema gestor de base de datos MYSQL:

![img1](/TEMA_3_S5/AWS/Screenshot_55.png)

Creamos la base de datos para wordpress:

```
CREATE DATABASE wordpress; 
CREATE USER 'wordpress_user'@'%' IDENTIFIED BY 'password123'; 
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress_user'@'%'; 
FLUSH PRIVILEGES;
```

![img1](/TEMA_3_S5/AWS/Screenshot_56.png)

Vamos a la interfaz web de instalación de wordpress:

![img1](/TEMA_3_S5/AWS/Screenshot_61.png)

A continuación deberemos rellenar los campos con el nombre de la base de datos de RDS, el nombre de usuario y contraseña que creamos anteriormente, y el punto de enlace de la base de datos. Una vez rellenados los campos, pulsamos **Submit**.


#### Vinculación del EFS en Wordpress (Carpeta wp-content)

Detenmos primero apache con el comando:

````
sudo systemctl stop apache2
````

Movemos la carpeta **wp-content** a nuestro EFS con el comando:

````
sudo cp -r /var/www/html/wp-content Ruta_EFS
````

Podemos renombrar la carpeta original para que no se sobreescriba con el comando:

````
sudo mv /var/www/html/wp-content /var/www/html/wp-content-old
````

Creamos una vinculación de la carpeta wp-content en el EFS con el comando:

````
sudo ln -s /mnt/efs/wp-content /var/www/html/wp-content
````

Modificamos los permisos de la carpeta EFS para que el usuario apache pueda leer y escribir en ella:

````
sudo chown -R www-data:www-data /mnt/efs/wp-content
sudo chmod -R 775 /mnt/efs/wp-content
````

Finalmente , reiniciamos apache para que se apliquen los cambios:

````
sudo systemctl start apache2
````