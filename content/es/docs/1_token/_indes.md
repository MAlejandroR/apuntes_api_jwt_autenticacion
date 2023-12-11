---
title: "El  Token"
linkTitle: "Token"
weight: 10
icon: fa-brands fa-php
draft: false
---


{{< pageinfo >}}
Vamos a Estudiar y utilizar que es un :shield: Token y cómo lo podemos utilizar  
{{< /pageinfo >}}
{{% pageinfo %}}
https://github.com/firebase/php-jwt
{{% /pageinfo %}}


## Token
Un token __JWT__ _(JSON Web Token)_ es una cadena de caracteres que se utiliza para representar información con estructura JSON de manera segura entre dos partes.

Esta cadena de caracteres, la vamos a __firmar__  para verificar su integridad y autenticidad.

El token va a permitir recomponer información inicial como veremos

### Estructrua del token
{{< pageinfo >}}
 Un token JWT tiene tres partes separadas por puntos (.):
{{< /pageinfo >}}

1. __Header__: Contiene información sobre el tipo de token y el algoritmo de firma.  
1. __Payload__: Contiene los datos del usuario o cualquier información adicional que se desea transportar.  
1. __Signature__: Se utiliza para verificar que el remitente del token es quien dice ser y para asegurar que la carga útil del token no ha sido alterada.

### Firma y Verificación 

__El token__ puede ser firmado usando una clave  secreta compartida entre el emisor y el receptor. 
El algoritmo de firma se especifica en el encabezado.   
La firma es utilizada para verificar la integridad del token y asegurar que no ha sido modificado por terceros.    

### 