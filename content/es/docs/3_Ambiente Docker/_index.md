---
title: "Creando el ambiente"
linkTitle: "Ambiente con docker"
weight: 20
icon: fa-brands fa-docker
draft: false
---


{{< pageinfo >}}
Cómo crear tokens en php  
{{< /pageinfo >}}
{{% pageinfo %}}
https://github.com/firebase/php-jwt
{{% /pageinfo %}}

## Creando el ambiente 
Creamos un fichero docker file con los siguientes servicios    
Vamos a necesitar o disponer de 3 servicios:
> Apache con php
Para el servidor web con el intérprete php
> Mysql
Servidor de base de datos
> phpmyadmin
Una utilidad para poder acceder a la base de datos

### Apache y php

Partimos de la imagen __php:8.2-apache__ __(https://hub.docker.com/_/php)__
{{< imgproc  contenedor_apache_1.png Fit "100000x700000 center" >}}

{{< /imgproc >}}
La vamos a aportar dentro de un Dockerfile, ya que hay que instalar librerías que no vienen en la imagen oficial.
Necesitamos tener las siguientes librerías:
> * __En php__
pdo, mysql_pdo
> * __En apache__
>  habilitar los módulos:
> * headers
Estos módulos nos permitirán establecer cabeceras donde estableceremos que aceptamos solicitudes de direntes orígenes
> * Esto es importante por la seguridad CORS
> * rewrite
#### Cors

{{% pageinfo%}}
__CORS__
> Cross-Origin Resource Sharing o compartir recursos de orígenes cruzados.
Mira la sección de [CORS](/contenido/docs/cors/)

{{% /pageinfo%}}


* Agregamos un fichero de configuración, le llamamos por ejemplo  _apache-config.conf_ y lo copiamos
  {{< highlight apache2 "linenos=table,anclorlinenos=true, hl_lines=" >}}
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

* También tenemos que habilitar el módulo de __header__
  {{< imgproc contenedor_apache_2    Fit "100000x700000 center" >}}

{{< /imgproc >}}


```dockerfile
version: "3.6"
services:
  apache:
    build: apache
    ports:
      - ${PORT_APACHE}:80
    volumes:
      - ./../../app:/var/www/html
    depends_on:
      - mysql
```
En el directorio __apache__, habrá un __Dockerfilke
{{< highlight dockerfile "linenos=table,anclorlinenos=true, hl_lines=" >}}


En el directorio __apache__, habrá un __Dockerfile__
# Usa la imagen oficial de PHP con Apache
FROM php:8.2-apache

# Instala las extensiones necesarias
RUN docker-php-ext-install pdo pdo_mysql

# Instala Xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

COPY apache-config.conf /etc/apache2/sites-available/000-default.conf


# Habilita módulos de Apache
RUN a2enmod rewrite
RUN a2enmod headers

{{< / highlight >}}

### servicio mysql

Este sería un contenedor a partir de la imagen __mysql__ (https://hub.docker.com/_/mysql).       
> Mapeamos el directorio de datos para mantener los datos de la BD.

{{< highlight bash "linenos=table,anclorlinenos=true, hl_lines=6" >}}
mysql:
  image: mysql
    ports:
      - ${PORT_MYSQL}:3306
    volumes:
      - ./../../datos/mysql:/var/lib/mysql
      - ./../../datos/base_datos.sql:/docker-entrypoint-initdb.d/init.sql
    environment:
      - MYSQL_USER=${USER_}
      - MYSQL_PASSWORD=${PASSWORD}
      - MYSQL_DATABASE=${DATABASE}
      - MYSQL_ROOT_PASSWORD=${PASSWORD_ROOT}
{{< / highlight >}}
>  Establecemos variables de entorno a partir de un fichero .env.

{{< highlight bash "linenos=table,anclorlinenos=true, hl_lines=4 9 10 11 12" >}}
mysql:
image: mysql
ports:
- ${PORT_MYSQL}:3306
volumes:
- ./../../datos/mysql:/var/lib/mysql
- ./../../datos/base_datos.sql:/docker-entrypoint-initdb.d/init.sql
environment:
- MYSQL_USER=${USER_}
- MYSQL_PASSWORD=${PASSWORD}
- MYSQL_DATABASE=${DATABASE}
- MYSQL_ROOT_PASSWORD=${PASSWORD_ROOT}
{{< / highlight >}}
> El fichero __.env__
 
{{< highlight bash "linenos=table,anclorlinenos=true, hl_lines=" >}}
USER_="alumno_nett"
PASSWORD="password_nett"
HOST=mysql
PORT_MYSQL=23306
PORT_APACHE=8080
PORT_PHPMYADMIN=8081
DATABASE=usuarios
PASSWORD_ROOT=root12345
KEY=7KQKj0McKudurOcXJKeAfH6CsrscrZyVDi6Xd1Q65gY=
{{< / highlight >}}
 
> Tenemos un fichero sql que queremos que se ejecute al levantar el contenedor.

{{< highlight DockerFile "linenos=table,anclorlinenos=true, hl_lines=7" >}}
mysql:
image: mysql
ports:
- ${PORT_MYSQL}:3306
  volumes:
- ./../../datos/mysql:/var/lib/mysql
- ./../../datos/base_datos.sql:/docker-entrypoint-initdb.d/init.sql
  environment:
- MYSQL_USER=${USER_}
- MYSQL_PASSWORD=${PASSWORD}
- MYSQL_DATABASE=${DATABASE}
- MYSQL_ROOT_PASSWORD=${PASSWORD_ROOT}
  {{< / highlight >}}
> El fichero __base_datos.sql__

{{< highlight sql "linenos=table,anclorlinenos=true, hl_lines=" >}}
CREATE DATABASE IF NOT EXISTS usuarios;
USE usuarios;

CREATE TABLE IF NOT EXISTS usuarios (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50) unique NOT NULL,
password VARCHAR(255) NOT NULL,
rol  VARCHAR(10) NOT NULL check (rol in('admin','usuario','gestor'))
);

-- Insertar los usuarios de ejemplo
INSERT INTO usuarios (nombre, password,rol) VALUES
("maria", SHA2('maria', 256),"admin"),
("pedro", SHA2('pedro', 256),"admin"),
("lourdes", SHA2('lourdes', 256),"gestor"),
("luis", SHA2('luis', 256),"usuario");

{{< / highlight >}}



### phpmyadmin
Creamos este contenedor a partir de la imagen __phpmyadmin__ https://hub.docker.com/_/phpmyadmin
{{< highlight DockerFile "linenos=table,anclorlinenos=true, hl_lines=2" >}}
phpmyadmin:
image: phpmyadmin
ports:
- ${PORT_PHPMYADMIN}:80
depends_on:
- mysql
environment:
- PMA_ARBITRARY=1
- PMA_HOST=mysql
{{< / highlight >}}
> mapear un puerto para acceeder por http y utilizamos variuable de entorn
> 
{{< highlight DockerFile "linenos=table,anclorlinenos=true, hl_lines=4" >}}
phpmyadmin:
  image: phpmyadmin
  ports:
    - ${PORT_PHPMYADMIN}:80
  depends_on:
    - mysql
  environment:
    - PMA_ARBITRARY=1
    - PMA_HOST=mysql
  {{< / highlight >}}
>Utilizamos variables de entorno

