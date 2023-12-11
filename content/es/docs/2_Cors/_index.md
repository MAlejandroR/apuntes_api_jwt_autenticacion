---
title: "CORS"
linkTitle: "2_Cors"
weight: 30
icon: fa-solid fa-screwdriver-wrench
draft: false
---


{{< pageinfo >}}
Cómo crear tokens en php  
{{< /pageinfo >}}
### Referencias Web
1. **Mozilla Developer Network (MDN) - CORS**
  > - [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

2. **Wikipedia - Same-origin policy**
  > - [Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy)

3. **Enable CORS**
  > - [Enable CORS](https://enable-cors.org/)



## Cors
{{% pageinfo%}}
  __CORS__ 
> Cross-Origin Resource Sharing o compartir recursos de orígenes cruzados.
 
{{% /pageinfo%}}
{{% pageinfo%}}

 
### Situación
En la web existen muchas situaciones en las cuales una aplicación web alojada en un dominio intenta realizar solicitudes a otro dominio distinto al de origen. Por ejemplo cuando hacemos una API, para ofrecer datos.   
Esto puede suponer un problema porque no sabemos desde dónde nos pueden consumir datos, y podría incluso ocurrir que los datos sean sensibles y no queremos que cualquiera los consulte.     
### Solución
__CORS__ permite a los servidores declarar explícitamente qué dominios tienen permitido acceder a sus recursos, mediante el uso de cabeceras HTTP. Esta restricción ayuda a evitar solicitudes no autorizadas y protege la integridad y la privacidad de los datos en aplicaciones web.    
{{% /pageinfo%}}
### Cabeceras concretas usadas en CORS
{{< highlight php "linenos=table,anclorlinenos=true, hl_lines=1-4" >}}
header("Access-Control-Allow-Origin: * ");
header("Access-Control-Allow-Methods: GET, POST, OPTIONS");
header("Access-Control-Allow-Headers: Content-Type");
header("Access-Control-Allow-Credentials: true");
{{< / highlight >}}
### Nuestra acción para el proyecto
>Tenemos dos opciones.     

##### Opción 1: dentro del fichero php    
Establecer directamente con php usando la función __header__ como hemos expuesto anteriormente:
{{< highlight php "linenos=table,anclorlinenos=true, hl_lines=7-10" >}}
<?php
//código donde queremos enviar una respuesta a otro dominio
 ......
  //Preparamos la información que queremos enviar
  $datos = ["msj"=>"Los datos son incorrectos"];

  header("Access-Control-Allow-Origin: * ");
  header("Access-Control-Allow-Methods: GET, POST, OPTIONS");
  header("Access-Control-Allow-Headers: Content-Type");
  header("Access-Control-Allow-Credentials: true");

  //Lo enviamos con un simple echo cofificando en json
  echo json_encode($datos);
  exit();
{{< / highlight >}}



##### Opción 2: En la configuracion de apache  

Establecer el __header__ como configuración de apache2.      
Podemos crear un fichero .htaccess en el directorio del proyecto y esa configuración se cargará para apache en la entrega de este proyecto       
Para que apache lea los directorios __.htacces__ debe de tener configurado la opción __AllowOverride__ en este directorio   
Para conseguir todo, vamos a realizar dos acciones en nuestro docker:    1. Estableceremos un fichero de configuración para el directorio __DocumentRoot__ de apache
{{< highlight bash "linenos=table,anclorlinenos=true, hl_lines=6" >}}
<VirtualHost *:80>
    ServerAdmin manuelromeromigue@gmail.com
    DocumentRoot /var/www/html
      <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
{{< / highlight >}}

>Ahora en el fichero __.htaccess__ estalblecemos las cabeceras para __CORS__

{{< highlight bash "linenos=table,anclorlinenos=true, hl_lines=2-4" >}}
<IfModule mod_headers.c>
    Header set Access-Control-Allow-Origin "*"
    Header set Access-Control-Allow-Methods "GET, POST, OPTIONS"
    Header set Access-Control-Allow-Headers "Content-Type"
</IfModule>
{{< / highlight >}}








