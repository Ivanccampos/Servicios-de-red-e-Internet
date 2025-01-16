 # Actividad #5

##  Instala y configura bin9 en primer lugar como servidor caché y por último como forwarding.

Partimos desde una máquina ubuntu ya instala y actualizada

`````
sudo apt update
sudo apt upgrade
`````

### Instalar Bind en el servidor DNS
Necesitamos actualizar nuestro índice de paquetes locales e instalar el software usando apt

`````
sudo apt-get install bind9 bind9utils bind9-doc
`````

![img1](/Tema_2_Actividad_5/images/Screenshot_1.png)


### Configurar como un servidor DNS Caching

Los archivos de configuración de Bind se mantienen de forma predeterminada en un directorio en /etc/bind. Nos movemos a este directorio:

`````
cd /etc/bind
`````

![img1](/Tema_2_Actividad_5/images/Screenshot_2.png)



Para un servidor DNS de almacenamiento en caché, solo modificaremos el archivo named.conf.options.

`````
sudo nano named.conf.options
`````

Para configurar el almacenamiento en caché, el primer paso es configurar una lista de control de acceso o ACL.

![img1](/Tema_2_Actividad_5/images/Screenshot_3.png)


Por encima del bloque de opciones, crearemos un nuevo bloque llamado acl. Cree una etiqueta para el grupo ACL que está configurando.

![img1](/Tema_2_Actividad_5/images/Screenshot_4.png)


Dentro de este bloque, enumere las direcciones IP o las redes que deberían poder usar este servidor DNS.

![img1](/Tema_2_Actividad_5/images/Screenshot_5.png)
![img1](/Tema_2_Actividad_5/images/Screenshot_8.png)

Ahora que tenemos una ACL de clientes para los que queremos resolver la solicitud, podemos configurar esas capacidades en el bloque de opciones.

![img1](/Tema_2_Actividad_5/images/Screenshot_6.png)

### Configurar como un servidor DNS Forwarding


Comenzaremos con la configuración que dejamos en la configuración del servidor de almacenamiento en caché.

Utilizaremos la misma lista ACL para restringir nuestro servidor DNS a una lista específica de clientes. Sin embargo, debemos cambiar la configuración para que el servidor ya no intente realizar consultas recursivas.


Para hacer esto, necesitamos configurar una lista de servidores de almacenamiento en caché para reenviar nuestras solicitudes.

Esto se hará dentro de las opciones {} bloque. Primero, creamos un bloque dentro de los reenviadores de llamadas que contiene las direcciones IP de los servidores de nombres recursivos a los que queremos reenviar solicitudes.

![img1](/Tema_2_Actividad_5/images/Screenshot_7.png)

Después, deberíamos establecer la directiva de reenvío en “only”, ya que este servidor reenviará todas las solicitudes y no debería intentar resolver las solicitudes por sí solo.


![img1](/Tema_2_Actividad_5/images/Screenshot_9.png)


Un cambio final que debemos hacer es a los parámetros dnssec. Con la configuración actual, dependiendo de la configuración de los servidores DNS reenviados, puede ver algunos errores.


### Comprobar la configuración y reinicia Bind


Ahora que tiene su servidor Bind configurado como un servidor DNS de almacenamiento en caché o un servidor DNS de reenvío, estamos listos para implementar nuestros cambios.

Antes de dar el paso y reiniciar el servidor Bind en nuestro sistema, debemos usar las herramientas incluidas en Bindings para verificar la sintaxis de nuestros archivos de configuración.

`````
sudo named-checkconf
`````

Si no hay errores de sintaxis en su configuración, el mensaje del shell volverá inmediatamente sin mostrar ninguna salida.

Cuando haya verificado que sus archivos de configuración no tienen ningún error de sintaxis, reinicie el demonio Bind para implementar sus cambios.

`````
sudo systemctl restart bind9
`````

![img1](/Tema_2_Actividad_5/images/Screenshot_10.png)


Si siguió la guía de configuración inicial del servidor, el firewall UFW está habilitado en su servidor. Necesitamos permitir el tráfico DNS a nuestro servidor para responder a las solicitudes de los clientes.

![img1](/Tema_2_Actividad_5/images/Screenshot_11.png)

Luego, vigile los registros del servidor mientras configura su máquina cliente para asegurarse de que todo salga bien. Deje esto en ejecución en el servidor:

`````
sudo journalctl -u bind9 -f
`````

### Configurar la máquina cliente

Necesitamos editar el archivo /etc/resolv.conf para apuntar nuestro servidor al servidor de nombres. Los cambios realizados aquí solo durarán hasta el reinicio, lo cual es ideal para las pruebas. Si estamos satisfechos con los resultados de nuestras pruebas, podemos hacer que estos cambios sean permanentes.

![img1](/Tema_2_Actividad_5/images/Screenshot_12.png)

El archivo enumerará los servidores DNS que se utilizarán para resolver consultas estableciendo las directivas del servidor de nombres. Comente todas las entradas actuales y agregue una línea de servidor de nombres que apunte a su servidor DNS:

![img1](/Tema_2_Actividad_5/images/Screenshot_13.png)

Ahora, puede probar para asegurarse de que las consultas puedan resolverse correctamente utilizando algunas herramientas comunes. 

Puede usar ping para probar que se pueden hacer conexiones a dominios:

![img1](/Tema_2_Actividad_5/images/Screenshot_14.png)


Esto significa que nuestro cliente puede conectarse con google.com utilizando nuestro servidor DNS. 

Podemos obtener información más detallada mediante el uso de herramientas específicas de DNS como dig.

![img1](/Tema_2_Actividad_5/images/Screenshot_15.png)

