# Actividad 0.1

## Actividad 0.1 - HTTP Introduction

Quién, dónde y cuándo se crea el primer servidor web?

El primer servidor web se creó en el CERN, Suiza, en 1989-1990. El inventor del primer servidor web fue Tim Berners-Lee, un científico británico que trabajaba en el CERN en ese momento.

¿Qué pila de protocolos usa http?

	TCPIP


¿Componentes de una URL? 
-Protocolo o esquema.
-Dominio del servidor.
-Puerto.
-Ruta hacia el recurso.
-Cadena de consulta (opcional).
-Fragmento de ID (opcional).

¿Pasos en la recuperación de una página web mediante HTTP?  

	Mediante peticiones de mensaje/respuesta, normalmente de tipo GET.

	
Diferencia entre páginas dinámicas y estáticas.

Las páginas estáticas son diseñadas por los distintos creadores y diseñadores web, pero, a diferencia de las páginas dinámicas, estas se hacen sin ningún tipo de interacción con el usuario. La web estática está más bien diseñada para negocios o empresas, su finalidad es prestar servicios y  productos. Están compuestas por una serie de archivos html que permiten mostrar los textos, imágenes, vídeos… que forman la página.

La web dinámica, como su nombre indica se trata de interactuar más, no es una web fija.  El usuario puede realizar acciones como por ejemplo, escribir comentarios, comprar etc. Se ha impuesto en el mundo del diseño.                                                                                            Para crear este tipo de páginas se necesitan conocimientos de programación y saber manejar bases de datos.

Request. Métodos principales

GET
El método GET solicita una representación de un recurso específico. Las peticiones que usan el método GET sólo deben recuperar datos.

HEAD
El método HEAD pide una respuesta idéntica a la de una petición GET, pero sin el cuerpo de la respuesta.

POST
El método POST se utiliza para enviar una entidad a un recurso en específico, causando a menudo un cambio en el estado o efectos secundarios en el servidor.

PUT
El modo PUT reemplaza todas las representaciones actuales del recurso de destino con la carga útil de la petición.

DELETE
El método DELETE borra un recurso en específico.

CONNECT
El método CONNECT establece un túnel hacia el servidor identificado por el recurso.

OPTIONS
El método OPTIONS es utilizado para describir las opciones de comunicación para el recurso de destino.

TRACE
El método TRACE realiza una prueba de bucle de retorno de mensaje a lo largo de la ruta al recurso de destino.

PATCH
El método PATCH es utilizado para aplicar modificaciones parciales a un recurso.




	


Response. Códigos

Los códigos de estado de respuesta HTTP indican si se ha completado satisfactoriamente una solicitud HTTP específica. Las respuestas se agrupan en cinco clases:

Respuestas informativas (100–199),

Respuestas satisfactorias (200–299),

Redirecciones (300–399),

Errores de los clientes (400–499),

y errores de los servidores (500–599).

	
Content type. Tipos principales
	

	El content type, también conocido como media type o MIME type (extensiones de correo de Internet multipropósito), es un recurso que se utiliza en la cabecera HTTP (header) y que le indica al cliente o navegador qué clase de archivo o medio le está enviando el servidor.

Dicho así, especificar el tipo de contenido ayuda a los user agents a entender el contexto y mejorar significativamente la experiencia de usabilidad, al mostrar el archivo o medio de la mejor forma posible.

Existe un amplio número de media types que se pueden utilizar al configurar un content type. A continuación se muestran solamente algunos de los más comunes, pero la lista completa se puede encontrar en: Iana.org.
application/pdf
application/xml
audio/ogg
audio/mpeg
image/apng
image/jpeg (.jpg, .jpeg, .jfif, .pjpeg, .pjp)
text/css
text/html
text/php
text/xml
video/mp4


# Actividad 0.2

## UDP and TCP: Comparison of Transport Protocols

Diferencias entre udp y tcp? (min 2:46 y 4:15)

UDP
– Utiliza paquetes de menor tamaño (hasta un 60%).
– Cabecera de 8 bytes.
– No requiere conexión previa para mandar datos.
– Mayor control sobre cuándo se manda el mensaje.

TCP
– Cabecera de 20 bytes.
– Más fiable que UDP.
– Mayor overhead de comunicación.
– Confirma la llegada de información.
– Retransmisión (vuelve a mandar un mensaje si se pierde).
– Control sobre congestión (mensajes pueden llegar más lentos).
– Se ordenan los mensajes antes de llegar al destinatario.
– Detección de errores.

¿Qué aplicaciones usan tcp?  

HTTP, FTP, SMTP, SSH, pop, imap y Telnet.

¿Qué aplicaciones usan udp?

DHCP, DNS, SNMP, TFTP, VoIP, IPTV

¿Qué capa almacena el puerto?

La capa de transporte (Capa 4 del modelo OSI) es la responsable de asignar y gestionar los puertos

¿Qué capa almacena la dirección IP?

La dirección IP se almacena en la capa de red (Capa 3) del modelo OSI

¿Qué es three-way handshake?

	El proceso de negociación entre dos equipos para establecer una conexión en el protocolo TCP.


# Actividad 0.3

## Práctica telnet/http
Lee el artículo y prueba los ejemplos sugeridos en él.

![img1](/Actividad0/imagenes/0.3.png)

# Actividad 0.4

## Usando Curl

*USAR HTTPS PARA ACCEDER A LAS PÁGINAS WEB*

Busca información sobre el comando curl y muestra al menos cinco ejemplos de uso

Obtener la página principal de un servidor web.

![img1](/Actividad0/imagenes/0.41.png)

![img1](/Actividad0/imagenes/0.42.png)

Obtener una página y guardarla en un archivo local

![img1](/Actividad0/imagenes/0.43.png)

Los 100 primeros bits de un documento

![img1](/Actividad0/imagenes/0.44.png)

Los 500 últimos bits de un documento

![img1](/Actividad0/imagenes/0.45.png)

Obtenga una página web y almacene en un archivo local, haga que el archivo local obtenga el nombre del documento remoto

![img1](/Actividad0/imagenes/0.46.png)





# Actividad 0.5

## Descargar python y ejecutar localhost


Descargamos python para nuestro equipo, activamos el servidor de Apache mediante el programa XAMPP y escribimos en nuestro buscador "localhost" o "127.0.0.1".

Si está hecho de forma correcta podemos ver que se ha creado un servidor y que accedemos a la página de Apache.

![img1](/Actividad0/imagenes/act1.png)

Si no hemos activado Apache previo a acceder al servidor, la página web por defecto nos mostrará nuestra carpeta de usuario.

![img1](/Actividad0/imagenes/error.png)


## Crear un server en python y compartirlo en clase

Vamos a crear un pequeño archivo html y lo vamos a guardar en la carpeta de usuario de nuestro equipo.

![img2](/Actividad0/imagenes/index.png)

Un vex creado el archivo podemos activar Apache y crear un servidor para mostrar la página mediante el comando 
```python
server.py
 ```

 ó 
 
 ```python
 python -m http.server 8000
 ```
Por defecto "server.py" nos asigna el puerto 8000.

![img3](/Actividad0/imagenes/create.png)

Cuando accedemos al buscador y si buscamos "localhost:8000" podemos ver nuestra página web.

![img4](/Actividad0/imagenes/check.png)

Esta página puede ser visualizada por otros usuarios si escribien en su buscador "dirección_ip_del_host:puerto".

![img4](/Actividad0/imagenes/check2.png)