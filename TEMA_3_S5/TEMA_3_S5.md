
# Práctica Wordpress en AWS

---

##  Objetivo

Instalación de Wordpress en instancia Ubuntu EC2 con soporte de base de datos RDS y EFS

---

### 1. Creación de la VPC

Iniciamos el laboratorio de AWS:

![image](https://github.com/user-attachments/assets/e8be3c4b-43d3-4742-851d-c7a86f585632)

Entramos al laboratorio:

![image](https://github.com/user-attachments/assets/8b4156b7-e48e-4832-a49c-ac90dc622f87)

Vamos al apartado de VPC y la creamos:

![image](https://github.com/user-attachments/assets/2524a6e6-3f70-428c-ad7b-91a9abb41fb8)

![image](https://github.com/user-attachments/assets/c6494b13-19c4-4ad0-a35e-a8106e0332e0)

Configuramos la VPC:

![image](https://github.com/user-attachments/assets/5e8a9071-8364-4b2f-a4ca-1ada5b049fc1)

![image](https://github.com/user-attachments/assets/34e75304-366e-4675-8736-1f6a6fccd082)

![image](https://github.com/user-attachments/assets/86f49129-fc2f-45e0-9a2c-bae3e34349e2)

Ahora la VPC se empieza a crear:

![image](https://github.com/user-attachments/assets/47138904-7345-4432-a2fe-6a0727376d11)

---

### 2. Creación de las máquinas EC2 en la subred publica y privada

Vamos a la zona de creación de máquinas EC2:

![image](https://github.com/user-attachments/assets/84e03612-2354-42b9-b4f2-eddddf5501b4)

Creamos la máquina para la red pública:

![image](https://github.com/user-attachments/assets/db74e7d4-3f83-46b2-8efb-8774964093a1)

Elegimos el sistema operativo:

![image](https://github.com/user-attachments/assets/4df21756-6d51-4efe-9eba-9ebc53702482)

Asignamos el par de claves para conexión con ssh

![image](https://github.com/user-attachments/assets/ed5c5aaa-232f-4248-823e-889cc3547957)

La asignamos a la VPC y subred publica 1:

![image](https://github.com/user-attachments/assets/7b25c303-cb52-4f15-9a66-ae87dcd253bb)

Creamos las reglas de firewall:

![image](https://github.com/user-attachments/assets/f682c494-678c-4f56-9747-913a864cf23d)

Creamos la máquina:

![image](https://github.com/user-attachments/assets/ce3e4f62-785f-48f0-ad78-2175ab15ddb8)

---

### 3. Instalación de Apache y php en la instancia EC2

Instalamos la clave ppk para conexión con la máquina EC2:

![image](https://github.com/user-attachments/assets/6a60003e-d175-43e2-b563-eef4bb690b51)

Copiamos el nombre DNS de la máquina:

![image](https://github.com/user-attachments/assets/909a9cac-583b-4c7a-a058-ef2d23d0d5c8)

Conectamos con ssh por Putty:

![image](https://github.com/user-attachments/assets/52a384af-b49f-4dc2-83af-bebd4ca772bf)

![image](https://github.com/user-attachments/assets/24ef63ff-ec51-4b3d-b1f2-0844acc4a689)

Nos conectamos con el usuario Ubuntu:

![image](https://github.com/user-attachments/assets/c17a099d-fc8b-4df8-959a-6d41f49f59de)

Realizamos un update y upgrade:

![image](https://github.com/user-attachments/assets/3440ad74-3ddb-4fe8-8335-a9d07db9dbbd)

![image](https://github.com/user-attachments/assets/184e01d7-18a8-495a-b56b-4b25449007c8)

Instalamos las apache y php:

![image](https://github.com/user-attachments/assets/8789c5a9-25d2-45ad-a543-6a4db2f9f9aa)

![image](https://github.com/user-attachments/assets/2cfc2b93-768b-47dc-8881-86d4aec8f550)

Reiniciamos apache2:

![image](https://github.com/user-attachments/assets/610b012b-2b12-405f-bb84-1f0db7938e72)

---

### 4. Creación de la base de datos

Vamos al apartado de RDS:

![image](https://github.com/user-attachments/assets/23d3907e-1977-4e7b-81c2-9e76e75e8927)

Creamos la base de datos:

![image](https://github.com/user-attachments/assets/89bcd943-7ace-4ccd-be20-91a89bbc14f6)

Asignamos el tipo de base de datos MYSQL:

![image](https://github.com/user-attachments/assets/f854e409-cd44-4a56-a8ad-72f4043e6d2a)

Elegimos la capa gratuita:

![image](https://github.com/user-attachments/assets/480e0564-9077-458f-b2da-c3eaa1b7b843)

Elegimos las credenciales del administrador y que sea autoadministrado:

![image](https://github.com/user-attachments/assets/0bea2caa-4534-4acb-96b4-3aac655bcd18)

Asignamos la configuracion de la instancia y almacenamiento:

![image](https://github.com/user-attachments/assets/47db4e81-4a2e-4706-9a95-2eceae867b4e)

Configuramos la conectividad de la base de datos:

![image](https://github.com/user-attachments/assets/b9ca6771-9ce7-4ab0-a414-4b1360e1c477)

![image](https://github.com/user-attachments/assets/0c13e16d-295f-4e2e-9367-f90c61594448)

Creamos la base de datos:

![image](https://github.com/user-attachments/assets/7b944036-5bd0-454d-8bf6-8a37e2f6c146)

Configuramos la conexión con la instancia EC2:

![image](https://github.com/user-attachments/assets/a8628a74-9459-4abd-90e3-425009826ca5)

![image](https://github.com/user-attachments/assets/b240d175-41cc-4313-bb2e-8d8ef5e7c217)

![image](https://github.com/user-attachments/assets/c9c20f96-7a80-4bbc-9f45-4475d76f788d)

### 5. Creación de sistema de archivos EFS

Vamos al apartado de EFS:

![image](https://github.com/user-attachments/assets/df0d90a9-7b3c-4d1e-a892-9f5aed570317)

Creamos el EFS:

![image](https://github.com/user-attachments/assets/83a0afc3-6a5e-42ef-80d9-dca3ae38be53)

![image](https://github.com/user-attachments/assets/c46c9e14-b6c4-456d-afc9-509efc6e20e5)

Asignamos la regla de entrada NFS y MYSQL al grupo de seguridad al que se unen el EFS, el RDS y el EC2:

![image](https://github.com/user-attachments/assets/1447877b-0751-4492-a115-874ca379bdbc)

---

### 6. Instalación de wordpress

Descargamos el comprimido de wordpress:

![image](https://github.com/user-attachments/assets/7ef1d219-254e-47ba-9602-e81e78413cd5)

Extraemos el comprimido:

![image](https://github.com/user-attachments/assets/09693aac-d578-4b8c-93bd-20848d3007fe)

Instalamos el cliente MYSQL:

![image](https://github.com/user-attachments/assets/a1e87270-a46f-42f3-bd91-346208d84bda)

Nos conectamos al sistema gestor de base de datos MYSQL:

![image](https://github.com/user-attachments/assets/94b8b1d4-25aa-468a-99b7-ef0426e1d2b8)

Creamos la base de datos para wordpress:

```
CREATE DATABASE wordpress; 
CREATE USER 'wordpress_user'@'%' IDENTIFIED BY 'password123'; 
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress_user'@'%'; 
FLUSH PRIVILEGES;
```

![image](https://github.com/user-attachments/assets/3f1a52df-ea3b-4bd9-994d-c4f0879843fc)

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






















