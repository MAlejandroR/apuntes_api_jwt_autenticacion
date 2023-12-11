---
title: "4_Api php"
linkTitle: "4_Api php"
weight: 40
icon: fa-brands fa-php
draft: false
---


{{< pageinfo >}}
Qué es un api y cómo creaer una con php  
{{< /pageinfo >}}
{{% pageinfo %}}
https://github.com/firebase/php-jwt
{{% /pageinfo %}}


## API
{{% pageinfo%}}
 __API (Interfaz de Programación de Aplicaciones):__   
Un API, o Interfaz de Programación de Aplicaciones, es un conjunto de reglas y definiciones que permiten que dos aplicaciones se comuniquen entre sí.   
La idea es crear una aplicación web para que sea consultada/consumida, en general invocada desde otra apliación web.    
En el contexto de la gestión de datos en una base de datos, esta aplicación se conoce como un API RESTful (Representational State Transfer) proporciona un conjunto de puntos finales (__endpoints__) a los cuales se puede acceder a través de solicitudes HTTP para realizar operaciones CRUD (Crear, Leer, Actualizar, Borrar) en los datos, cada una de ellas va asociada a un verbo http (POST GET PUSH o PUT y DELETE)
{{% /pageinfo%}}


| __Método HTTP__ | __Operación CRUD__  |
|-----------------|---------------------|
| GET             | READ (Leer)         |
| POST            | CREATE (Crear)      |
| PUT/PATCH       | UPDATE (Actualizar) |
| DELETE          | DELETE (Borrar)     |


{{< swaggerui src="/api.yaml" >}}



