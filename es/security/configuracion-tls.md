---
title: "Estándares de Configuración TLS"
description: "Sobre Estándares de Seguridad"
author: "Chin Dou / César Vergara"
---

# Configuración TLS de manera segura

​Transport Layer Security (TLS) y su predecesor, Secure Socket Layer (SSL), son protocolos ampliamente utilizados que son diseñados para proteger las comunicaciones en redes, garantizando la autenticación, la confidencialidad y la integridad en el intercambio de datos.  

​Contrariamente a lo que suele creerse, TLS / SSL no solo se usa en aplicaciones web basadas en HTTP, sino que también se usa a menudo para proteger otros protocolos, incluso de email (SMTP, POP e IMAP), FTP, comunicaciones de chat (XMPP) , VPNs, etc.  

TLS / SSL usa uno o más conjuntos de cifrado criptográfico para cumplir sus objetivos y habilitar la compatibilidad entre una amplia gama de clientes y servidores. Un conjunto de cifrado es una combinación de algoritmos utilizados para la autenticación, el intercambio de claves, el cifrado y códigos de autenticación de mensajes (MAC). En la fase inicial de una conexión TLS / SSL, el cliente y el servidor negocian qué conjunto de cifrado se utilizará para proteger los intercambios de datos posteriores, en función de las suites de cifrado compatibles tanto en el cliente como en el servidor y otros aspectos de configuración.  

Aunque implementar TLS es un paso importante en la dirección correcta, configurar incorrectamente su servidor SSL / TLS puede proporcionar una falsa sensación de seguridad y dejar los sistemas afectados vulnerables a varios tipos de ataques, todo lo cual puede comprometer la confidencialidad e integridad de los datos intercambiados. Algunos de estos ataques incluyen, pero no se limitan a:  

-   [POODLE](https://www.openssl.org/~bodo/ssl-poodle.pdf)
-   [CRIME](https://en.wikipedia.org/wiki/CRIME)
-   [BEAST](http://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art027)
-   [HeartBleed](http://heartbleed.com/)  
    

Los errores de configuración y de implementación del lado del servidor generalmente entran en las siguientes categorías:

-   Despliegue de implementaciones inseguras del protocolo
-   Soporte de versiones inseguras del protocolo
-   Soporte de suites de cifrado débiles
-   Soporte de opciones de protocolo inseguras  
    

Esta sección describe las prácticas de seguridad recomendadas en Walmart relacionadas con la configuración del lado servidor del protocolo TLS.  

## Despliegue una implementación segura del protocolo TLS  

Aunque esté configurado correctamente, la versión del servidor TLS que está ejecutando podría ser afectada por vulnerabilidades conocidas como [HeartBleed](http://heartbleed.com/) o [ROBOT](https://robotattack.org/). Asegúrese de verificar que la versión exacta del software que está ejecutando sea segura. Para hacerlo, consulte el sitio web del proveedor con la versión exacta del servidor TLS que está ejecutando. Alternativamente, puede consultar el sitio Common Vulnerabilities and Exposures (CVE) para verificar si su software se ve afectado por vulnerabilidades conocidas: [https://cve.mitre.org/](https://cve.mitre.org/).​  

## Consideraciones especiales con respecto a la vulnerabilidad ROBOT  

La vulnerabilidad ROBOT afecta ciertas implementaciones SSL / TLS que usan cifrado basado en el algoritmo de encriptación RSA. En 2018, la mayoría de los proveedores afectados han emitido un parche para su producto. Sin embargo, si no puede actualizar a una versión no vulnerable del producto afectado, debe desactivar todos los cifrados basados en la encriptación RSA para mitigar la vulnerabilidad.  

## Deshabilitar SSL v2.0 y v3.0

SSL v2.0 fue la primera versión de SSL publicada en el año 1995. Contenía varios problemas de seguridad importantes que condujeron a la introducción de SSL v3.0 en 1996. Aunque implicó un rediseño completo del protocolo, la versión 3.0 se encontró vulnerable en 2014 al ataque  [POODLE](https://www.openssl.org/~bodo/ssl-poodle.pdf). Además,  [Elliptic Curve Cryptography (ECC)](http://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/)  (una de las suites de cifrado recomendadas a partir de 2018) no se puede utilizar con SSL v3.0.  

Si no está deshabilitado por defecto,  **se debe deshabilitar SSL v2.0.**

**​SSL v3.0 debe ser deshabilitado también**, a menos que haya una necesidad específica de soportar clientes antiguos (como Internet Explorer 6); en ese caso, se debe habilitar el modo  [TLS_FALLBACK_SCSV](https://www.exploresecurity.com/poodle-and-the-tls_fallback_scsv-remedy/)  para evitar ataques de  [degradación de versión de protocolo](https://en.wikipedia.org/wiki/Downgrade_attack).  

## Deshabilitar TLS v1.0 y v1.1

**TLS v1.0 es vulnerable al ataque** [**BEAST**](http://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art027) **y por lo tanto debe ser deshabilitado.**  Ciertas implementaciones de TLS v1.1 son vulnerables al ataque  [POODLE](https://www.openssl.org/~bodo/ssl-poodle.pdf). Debido a esta razón y al hecho de que los clientes que generalmente admiten TLS v1.1 también son compatibles con TLS1.2,  **Walmart requiere que TLS v1.1 se deshabilite para los servicios accesibles desde el Internet.**​​​  
  

## Deshabilitar la compresión TLS v1.0

La compresión de TLS v1.0 es intrínsecamente vulnerable a los ataques CRIME y, por lo tanto, debe ser deshabilitada. Para obtener más información sobre el ataque CRIME y sobre cómo los atacantes pueden abusar de la compresión TLS v1.0 para robar datos confidenciales de una conexión TLS, vea  [https://en.wikipedia.org/wiki/CRIME](https://en.wikipedia.org/wiki/CRIME).  
  

## Deshabilitar los algoritmos de cifrado débiles y especificar el orden de cifrado seguro

Para compatibilidad con versiones anteriores, el protocolo TLS admite cifrados débiles que incluyen DES, RC4 y MD5, todos los cuales son propensos a ataques prácticos. Estos algoritmos de cifrado deben ser explícitamente deshabilitados.  

Además, se debe especificar el orden en que se seleccionarán los conjuntos de cifrado seguro en función de su disponibilidad en el cliente. Aquí se provee los conjuntos de cifrado y el orden en el que deben seleccionarse según lo recomendado por Walmart:  

1.  ECDHE‐ECDSA‐AES256‐GCM‐SHA384      
2.  ECDHE‐RSA‐AES256‐GCM‐SHA384
3.  ECDHE‐ECDSA‐CHACHA20‐POLY1305
4.  ECDHE‐RSA‐CHACHA20‐POLY1305
5.  ECDHE‐ECDSA‐AES128‐GCM‐SHA256
6.  ECDHE‐RSA‐AES128‐GCM‐SHA256
7.  ECDHE‐ECDSA‐AES256‐SHA384
8.  ECDHE‐RSA‐AES256‐SHA384
9.  ECDHE‐ECDSA‐AES128‐SHA256
10.  ECDHE‐RSA‐AES128‐SHA256  
  

## Excepciones a este estándar

En ciertos casos, es posible que su sistema no pueda cumplir con los requisitos anteriores. Por ejemplo, el servidor podría tener que soportar clientes más antiguos que solo son compatibles con versiones débiles del protocolo TLS / SSL. En estas situaciones, debe comunicarse con el equipo de arquitectura de seguridad de Walmart Chile para analizar la mejor alternativa y crear una excepción.  

## Muestra de configuración segura

Estas son algunas configuraciones específicas para ciertas tecnologías que se pueden utilizar para implementar los requisitos descritos en esta sección.  

### Apache Web Server  

```
​SSLProtocol TLSv1.2

SSLHonorCipherOrder On

SSLCipherSuite ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-

SSLCompression Off​  
````

En ciertas versiones desactualizadas de los sistemas operativos Linux basados en Redhat, la directiva  _SSLCompression_  no es soportada. En estos casos, la compresión de TLS v1.0 se debe deshabilitar agregando la siguiente línea al archivo  _/etc/sysconfig/httpd_:​  
````
-   OPENSSL_NO_DEFAULT_ZLIB=1
````
## Microsoft Internet Information Services (IIS)  

La forma más sencilla de configurar el protocolo TLS en IIS es usar la herramienta IIS Crypto:  [https://www.nartac.com/Products/IISCrypto](https://www.nartac.com/Products/IISCrypto).  

Para deshabilitar SSL v2 y v3 manualmente, siga los siguientes pasos:

-   IIS 8

	*   [https://www.digicert.com/ssl-support/iis-disabling-ssl-v3.htm](https://www.digicert.com/ssl-support/iis-disabling-ssl-v3.htm)

-   IIS 7

	*	https://www.sslshopper.com/article-how-to-disable-ssl-2.0-in-iis-7.html](https://www.sslshopper.com/article-how-to-disable-ssl-2.0-in-iis-7.html)
	*	[https://support.microsoft.com/en-us/help/187498/how-to-disable-pct-1-0-ssl-2-0-ssl-3-0-or-tls-1-0-in-internet-informat](https://support.microsoft.com/en-us/help/187498/how-to-disable-pct-1-0-ssl-2-0-ssl-3-0-or-tls-1-0-in-internet-informat)

IIS no soporta la compresión TLS. Por lo tanto, no hay acciones específicas que se deben realizar al respecto.  

## Testeando su configuración  

Una vez que haya completado su configuración, es muy recomendable que la pruebe exhaustivamente para los siguientes aspectos:

-   **_Seguridad_**: asegúrese de que se cumplan los requisitos anteriores. Puede probar la seguridad de su configuración utilizando la herramienta  [_testssl.sh_](https://github.com/drwetter/testssl.sh/)_._
-   **_Funcionalidad_**: asegúrese de que la configuración no impida que los clientes que debe soportar se conecten a los servicios considerados.