---
title: "Estándares de Configuración Indicadores Cookies"
description: "Sobre Estándares de Seguridad"
author: "Chin Dou / César Vergara"
---
 
 ## Configuración de indicadores de cookie "secure" y "HTTP only"

Algunas cookies pueden contener información sensible tales como tokens usados para mantener sesiones autenticadas. Por lo tanto, se debe tener especial cuidado para protegerlas. Existen dos mecanismos que procuran proteger cookies conocidos como los indicadores _Secure_ y _HttpOnly_, los cuales describiremos a continuación:

-   _**Secure**_: le ordena al navegador que nunca envíe una cookie a ​través de un canal que no sea HTTPS. Esto previene ataques eavesdropping y Man-in-the-Middle que apuntan a revelar la cookie.
-   _**HttpOnly**_: le ordena al navegador a evitar que los códigos client-side accedan a la cookie. Esto reduce los ataques de código cross-site que generalmente están diseñados para robar la sesión de las víctimas.

Estos indicadores deben ser configurados cuando el servidor envía la directriz Set al navegador.

Los indicadores Secure y HttpOnly pueden ser configurados en varios niveles, incluyendo:

-   A nivel del código
-   A nivel del servidor web/de aplicaciones
-   A nivel del proxy y el balanceador de carga​  
    

A continuación, un ejemplo de cómo configurar ambos indicadores usando código Java:  
````
​Cookie cookie = getMyCookie("myCookieName");
cookie.setHttpOnly(true);
cookie.setSecure(true);​  
````
Así se debe configurar la sesión de la cookie como segura en el descriptor de despliegue WEB-INF/web.xml de la aplicación J2EE:  
````
​<session-config>
		<cookie-config>
			<http-only>true</http-only>
			<secure>true</secure>
	</cookie-config>
</session-config>​  
````

​A continuación, otro ejemplo de cómo configurar todas las cookies con Secure y HttpOnly en servidor web Apache usando el módulo _mod_headers_:  
````
​Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
````


