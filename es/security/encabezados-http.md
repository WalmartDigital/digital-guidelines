---
title: "Habilitar encabezados de seguridad HTTP"
description: "Sobre Estándares de Seguridad"
author: "Chin Dou / César Vergara"
---

# Habilitar encabezados de seguridad HTTP

## Contexto  

Varios de los mecanismos de seguridad validados en el navegador deben ser activados por el servidor mediante encabezados HTTP. Estos mecanismos de seguridad están diseñados para mitigar algunos de los riesgos de seguridad más comunes en las aplicaciones web. Esta sección explica las configuraciones de seguridad del navegador que deben ser habilitadas para las aplicaciones web de Walmart Chile. Se debe considerar que algunas de estas configuraciones pueden afectar la funcionalidad de la aplicación. Por ende, es altamente recomendable validar la configuración del servidor web con las partes interesadas (por ejemplo, desarrolladores y dueños) para prevenir cualquier trastorno en la funcionalidad del sistema.  

  

## X-Frame-Options

Esta configuración mitiga los ataques de Clickjacking previniendo que el contenido (para el cual el servidor está autorizado) sea reproducido dentro de los elementos del tipo _frame_ o _iframe_. Para mayor información sobre ataques de Clickjacking puede revisar [https://www.owasp.org/index.php/Clickjacking](https://www.owasp.org/index.php/Clickjacking).

El valor del encabezado X-Frame-Options debe ser ajustado a alguno de los siguientes valores:

-   X-Frame-Options: DENY
-   No se permite la reproducción dentro de elementos de tipo frame y iframe **(configuración recomendada)**
-   X-Frame-Options: SAMEORIGIN
-   La entrega de contenidos mediante elementos de tipo frame y iframe está permitida desde páginas dentro del mismo dominio que el servidor autoritativo  
    

 
## X-XSS-Protection  

Esta configuración instruye al navegador a prevenir la reproducción de información peligrosa en el contexto de ataques de cross-site scripting (XSS) reflejado. Para mayor información sobre ataques XSS, puede revisar [https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)#Stored_and_Reflected_XSS_Attacks](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)​.  

El encabezado X-XSS-Protection debe ser ajustado a alguno de los siguientes valores:

-   0: deshabilitado **(no recomendado a menos que exista una justificación proveniente del negocio)**
-   1: filtrado habilitado, el navegador limpiará la página codificando cualquier carácter peligroso  **(configuración recomendada)**
-   1; mode=block: previene mostrar la página  
-   1; report=\<reporting-URI>: limpia la página y reporta el ataque a la URI especificada

Nota: esta configuración generalmente aplica a sitios web y no aplica a servicios web.

## X-Content-Type-Options  

Esta configuración se usa para prevenir la confusión de los tipos MIME del navegador. Para mayor información sobre los problemas de confusión de tipos MIME, puede revisar [https://blog.mozilla.org/security/2016/08/26/mitigating-mime-confusion-attacks-in-firefox/](https://blog.mozilla.org/security/2016/08/26/mitigating-mime-confusion-attacks-in-firefox/).  

El valor del encabezado _X-Content-Type-Options_ debe ser:

-   X-Content-Type-Options: nosniff
-   Bloquea la solicitud si su tipo es
-   "style" y el tipo MIME no es "text/css", o
-   "script" y el tipo MIME no es del tipo JavaScript MIME.  
    

Nota: esta configuración generalmente aplica a sitios web y no aplica a servicios web.  

## Content-Security-Policy  

Esta configuración busca prevenir ataques de inyección de código (tales como el XSS) que carga contenidos maliciosos de ubicaciones no autorizadas. Para hacerlo, la configuración controla desde donde puede alimentarse el navegador, en terminos de los siguientes tipos de recursos:

-   Javascript (JS)
-   Cascading Stylesheets (CSS)
-   Images (IMG)  
      
    

El encabezado debe ser configurado para especificar la política de seguridad de contenido del servidor. Una política de búsqueda define los orígenes permitidos para cierto tipo de contenido (por ejemplo, imágenes, JS, hojas de estilo)

La política de búsqueda _default-src_ es las más importante ya que especifica la política por defecto, que debe ser la contingencia de seguridad para los tipos de recursos que no tienen políticas propias.

Otras políticas de búsqueda incluyen:

-   _img-src_: especifica fuentes válidas para imágenes y favicons.
-   _script-src_: especifica fuentes válidas para JavaScript.
-   _style-src_: especifica fuentes válidas para hojas de estilo.
-   _media-src_: especifica fuentes válidas para cargar media usando los elementos \<audio>, \<video> and \<track>.  
    

Para una lista completa de las posibles directrices, por favor visite [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy).

Cada política específica una lista que incluye los orígenes permitidos para los tipos de recursos disponibles.

Los orígenes pueden ser especificados como se ve a continuación:

-   Usando la palabra clave ´self´ que se refiere al dominio del servidor, excluyendo subdominios
-   Especificando nombres de dominios

-   static.lider.cl: permite cargar recursos solo desde _static.lider.cl_.
-   *.lider.cl: solo permite cargar recursos desde _lider.cl_ y todos sus subdominios.
-   *: permite cargar recursos desde cualquier dominio.  
      
    

El uso del carácter comodín (*) debe hacerse cuidadosamente ya que una política de seguridad excesivamente permisiva podría exponer al sistema a ataques de inyección de código.

A continuación, un ejemplo de política de seguridad que incluye imágenes de cualquier origen en su propio contenido pero que restringe audio y video a proveedores confiables, y todos los códigos únicamente a un servidor especifico que almacena código confiable.

Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com

Para una descripción completa del mecanismo de política de seguridad de contenido, dirigirse a [https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

Se debe considerar que la política de seguridad de contenido de un sitio web debe ser cuidadosamente coordinada entre los equipos de infraestructura y desarrollo debido a que una política excesivamente restrictiva podría provocar que el sitio no funcione según lo esperado por el bloqueo de acceso a ciertos recursos requeridos.

Nota: esta configuración generalmente aplica a sitios web y no aplica a servicios web. ​​  

## Strict-Transport-Security  

Este mecanismo esta designado para mitigar ataques que apuntan a comprometer conexiones que están hechas para la versión no HTTPS de su sitio. A pesar de que su servidor podría estar redirigiendo HTTP a HTTPS, la conexión inicial HTTP es susceptible a ataques Man-in-the-Middle (MitM). _Strict-Transport-Security_ reduce los riesgos instruyendo al navegador a conectarse siempre mediante HTTPS, aun cuando el usuario explícitamente se conecte a la versión HTTP de su sitio (por ejemplo, escribiendo la URL en la barra de direcciones).

La primera vez que el sitio es accedido usando HTTPS se convoca el encabezado Strict-Transport-Security, de modo que el navegador registra esta información para que los próximos intentos de cargar el sitio usaran automáticamente HTTPS en lugar de HTTP.

El encabezado Strict-Transport-Security admite las siguientes directrices:

-   _max-age=<expire-time_>: el tiempo, en segundos, en que el navegador tiene que recordar que el sitio solo es accedido usando HTTPS  **(valor recomendado: 11491200).**
-   _includeSubDomains_: Si este parámetro opcional es especificado, esta regla aplica también a todos los sitios del subdominio.​  
    

Nota: esta configuración generalmente aplica a sitios web y no aplica a servicios web.  
  
## Ejemplo de configuración  

Este es un ejemplo de configuración segura de encabezados HTTP para servidores Apache usando el módulo _mod_headers_:  
````
​Header set X-Content-Type-Options nosniff

Header always set Strict-Transport-Security "max-age=11491200; includeSubDomains"

Header set X-XSS-Protection "1"

Header set X-Frame-Options DENY

Header set Content-Security-Policy "default-src 'self' *.lider.cl *.bazaarvoice.com *.survicate.com *.googletagmanager.com *.googleadservices.com *.google-analytics.com *.hotjar.com *.doubleclick.net *.queue-it.net *.richrelevance.com *.gstatic.com *.facebook.net *.mouseflow.com *.nr-data.net;​​
````

