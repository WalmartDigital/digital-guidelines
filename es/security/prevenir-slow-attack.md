---
title: "Prevenir ataques Slow HTTP"
description: "Sobre Estándares de Seguridad"
author: "Chin Dou / César Vergara"
---
 
 ## Prevenir ataques slow HTTP

Los ataques Slow HTTP son ataques de denegación de servicio (Denial-of-Service (DoS)) en los cuales el atacante envía solicitudes HTTP al servidor por partes, una a la vez y lentamente. Si una solicitud HTTP no está completa o si la tasa de transferencia es muy baja, el servidor mantiene sus recursos ocupados esperando por el resto de la información. Cuando la agrupación de conexiones del servidor alcanza su máximo se crea una condición DoS ya que el servidor no puede facilitar una nueva conexión. Es muy fácil ejecutar ataques Slow HTTP ya que el atacante requiere recursos mínimos y lo puede realizar desde una sola máquina. Para mayor información sobre ataques Slow HTTP, puede revisar [https://en.wikipedia.org/wiki/Slowloris_(computer_security)](https://en.wikipedia.org/wiki/Slowloris_%28computer_security%29).  

Se debe considerar que la mayoría de los servidores web y de aplicaciones son afectados por este tipo de vulnerabilidad, incluyendo:

-   Apache
-   Microsoft IIS
-   Tomcat
-   Oracle Weblogic
-   IBM Websphere
-   JBoss  
    

Se deben tomar medidas específicas en la configuración de su servidor web y de aplicaciones para prevenir esta vulnerabilidad. Para hacerlo, use una combinación de las siguientes técnicas:

-   **Configure apropiadamente el tiempo límite de la conexión**. Puede haber varias opciones en términos del timeout dependiendo del servidor, incluyendo:
-   Timeout absoluto: el tiempo máximo para mantener la conexión viva independientemente de la interacción con el cliente.
-   Timeout in-between-requests: el tiempo máximo entre solicitudes en el contexto de la conexión keep-alive/persistent.
-   **Definir la tasa mínima de información entrante**, y desechar conexiones que sean más lentas que esa tasa. Se debe tener precaución de no definir el mínimo a un valor muy bajo o se corre el riesgo de desechar conexiones legítimas.
-   **Limitar el tamaño del cuerpo y del encabezado** de la solicitud para prevenir solicitudes que envían información constantemente.
-   **Inhabilitar los métodos HTTP innecesarios** ya que algunos ataques utilizan verbos HTTP distintos a los comunes GET y POST.
-   **Limitar la cantidad de conexiones provenientes de la misma IP**. Esto ayudará a prevenir ataques que implican crear muchas conexiones desde el mismo host. Se debe tener en cuenta que algunos clientes podrían estar ubicados detrás de la misma dirección IP de origen. Por lo tanto, configurar este número muy bajo podría bloquear la conexión de clientes legítimos. Lo mismo aplica para servicios ubicados dentro de proxys inversos o balanceadores de carga. En estos casos es mejor implementar controles basados en la dirección IP de origen en el componente que está manteniendo directamente la conexión TCP/IP con el cliente.

​Se debe probar rigurosamente la configuración realizada por dos razones:  

-   Asegurar que es efectiva en la prevención de ataques Slow HTTP
-   Utilice la herramienta _slowhttptest_ para ejecutar ataques reales y probar su configuración: [https://github.com/shekyan/slowhttptest/wiki](https://github.com/shekyan/slowhttptest/wiki)
-   Garantizar que no se está limitando el acceso a usuarios legítimos por aplicar una política excesivamente restrictiva.
-   Utilice tests de stress que simulen un uso real y fuerte de su servicio.

A continuación, un ejemplo de cómo especificar solicitudes de timeout y de tasas mínimas de información entrante usando servidores web Apache y el módulo mod_timeout:  
````
RequestReadTimeout header=5-10,minrate=500 body=5,minrate=500
````
Para Microsoft IIS, siga los siguientes pasos:

-   Configure los valores de connectionTimeout, headerWaitTimeout, y minBytesPerSecond para los elementos <[limits](https://docs.microsoft.com/en-us/iis/configuration/system.applicationHost/sites/siteDefaults/limits)> y <[weblimits](https://docs.microsoft.com/en-us/iis/configuration/system.applicationHost/webLimits)>.
-   Seleccione [headerLimits](http://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits/headerlimits) para configurar el tipo y tamaño de encabezado que su servirdor web aceptará.
-   Limite los atributos de la petición a través del elemento [<requestlimits>](http://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits), específicamente los atributos maxAllowedContentLength, maxQueryString, y maxUrl.

