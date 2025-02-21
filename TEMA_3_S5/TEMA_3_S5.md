
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

![image](https://github.com/user-attachments/assets/1447877b-0751-4492-a115-874ca379bdbc)

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

![image](https://github.com/user-attachments/assets/1b0747fa-e201-4eaf-9cae-7c892a616a1b)

Copiamos el contenido de wp-config-sample.php a wp-config.php:

![image](https://github.com/user-attachments/assets/98ae3ac4-3ae9-497e-8f5e-6385bfd21c6a)

Modificamos el contenido de wp-config.php:

![image](https://github.com/user-attachments/assets/f7e1cb16-07ab-4b9c-ab10-27cf5b045890)

Renombramos la carpeta wp-content a wp-content2:

![image](https://github.com/user-attachments/assets/be004cab-4271-4bab-9f59-de877a55666d)

Creamos de nuevo wp-content:

![image](https://github.com/user-attachments/assets/b0a81fbf-453e-492a-9d92-8dfb05f715bf)

Instalamos las dependencias de nfs:

![image](https://github.com/user-attachments/assets/fa6a2374-a5af-4052-b1ca-566067233e43)

Montamos el EFS en wp-content:

![image](https://github.com/user-attachments/assets/51678320-84cd-423e-a1e4-e748ad2a76c6)

Instalamos wordpress:

![image](https://github.com/user-attachments/assets/5289795b-575f-4386-b2c7-947c3475dea9)

![image](https://github.com/user-attachments/assets/de7af036-acfc-40ed-bcff-408af693f011)

Nos autenticamos y accedemos:

![image](https://github.com/user-attachments/assets/818a37c6-0d82-4d30-a2a5-d3a4d3602919)






















