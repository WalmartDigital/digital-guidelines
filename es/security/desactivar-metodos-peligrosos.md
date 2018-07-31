---
title: "Desactivar métodos peligrosos HTTP"
description: "Sobre Estándares de Seguridad"
author: "Chin Dou / César Vergara"
---
 
 ## Desactivar métodos peligrosos HTTP

La mayoría de las aplicaciones web utilizan solamente los métodos de petición HTTP GET y POST. Como un esfuerzo para reducir al máximo la superficie de ataque de sus sistemas, usted debe deshabilitar todos los demás métodos HTTP que no serán utilizados y que no tienen una justificación de negocio, estos métodos incluyen:

-   HEAD
-   PUT
-   DELETE
-   OPTIONS
-   CONNECT
-   DEBUG
-   TRACE
-   …​  
      
    

Por ejemplo, en servidor web **Apache**, se pueden restringir los métodos HTTP permitidos de la siguiente manera (usando el módulo _mod_allowmethods_):  
````
​<Location "/">
	AllowMethods GET POST
</Location>  
````

Las solicitudes usando cualquier otro tipo de métodos no mencionados serán rechazadas por el servidor.  

En **Microsoft IIS**, se deben usar los elementos _<requestFiltering>_ en conjunto con _<verbs>_ para restringir a un conjunto limitado los métodos HTTP autorizados, tal como se explica en el siguiente artículo: [https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/requestfiltering/](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/requestfiltering/).

