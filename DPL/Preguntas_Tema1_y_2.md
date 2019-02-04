# Preguntas DPL

## Diff AppWeb vs PagWeb

Una aplicacion web es una pagina que el usuario puede controlar, siendo interactica o dinamica: mientras que la pagian web es estatica de y caracter meramente informativo.

## Modelo capas

La arquitectura web sigue el modelo de tres capas:
cliente, negocio y datos.

- La **capa cliente**, se refiere a donde se muestra la información del servicio, es decir, el navegador web del usuario final.
- La **capa de aplicación o de negocio**. Se divide en dos:
  - la _capa de presentación_ se encarga de integrar la parte dinámica con la estática, soliendo ser un servidor web.
  - La _capa lógica de negocio_: Suele llevar a cabo operaciones más complejas y se corresponde con la implantación de un servidor de aplicaciones.
- Por último, la **capa de datos**, que se comopone de un SGBD o fichero XML o texto plano.

## DNS. Qué es para qué sirve

Las siglas DNS se refieren a Domain Name System o Systema de Nombres de Dominio. Consiste en una jerarquía de nombres de cualquier recurso conectado a Internet o red privada, que relacionan cada uno de los nombres de dominio con cierta información que permite asociar, localizar y direccionar estos equipos.

Los tres componentes principales son:

- **Cliente DNS:** Se ejecuta en el pc del usuario y realiza solicitudes al servidor DNS para que resuelva los nombres.
- **Servidor DNS:** Resuelve la peticion del cliente DNS y le da una respuesta. Existen varios tipos pero los mas comunes son los _recursivos_.
- **Zonas de autoridad:** Parte del sistema de nombres de dominios sobre la que es responsable un servidor DNS.

Respecto a su función, sirve para `resolver la petición que hace un usuario a un nombre de dominio y transformala en una dirección IP` que corresponde con el servidor web donde se hospeda la página web.

## Pila de protocolos TCP/IP

Esta pila son una colección ordenada de protocolos que se han inpuesto como estandar junto al modelo OSI y que se divide en capas. Cada nivel o capa implementa un nivel de abstracción que depende del anterior.

Entre los protocolos mas comunes de esta pila, podemos encotnrar el HTTP. FTP SMTP, POP o TELNET.

La pila tiene cuatro capas:

- Capa de **acceso a la red** (asociada con OSI 1 y 2)
- Capa de **internet** (OSI 3)
- Capa de **transporte** (OSI 4)
- Capa de **aplicación** (OSI 5, 6, 7)

## Resolución directa e inversa, de nombres y correo. Que y para qué

El DNS tiene varios objetivos:

- **Resolución de nombres:** Dado un host recibir su IP
- **Resolución inversa de nombres:** Dada una IP, recuperar un nombre asociado
- **Resolución de servidores de correo:** Dado un nombre de dominio, devolver un servidor a través del cual hay que realizar una entrega de correo electrónico.

## Tipos de servidores DNS

Podemos encontrar varios tipos de servidores DNS:

- Servidores de zona maestra: Son los denominados servidores autoritativos. Tendrán una base de datos de toda la información asociada a una zona particular, y la misma se encontrará en dos servidores el serivdor maestro primario y el servidor esclavo secundario.

Existen dos tipos de servidores DNS según su método de resolución:

- **Resolución iterativa:** El servidor DNS devuelve la mejor respuesta que puede ofrecer al cliente en función de su caché,, pero si no la encuentra, pasará al siguiente servidor autorizado a preguntar: y siempre comenzando por un servidor Raíz.
- Una **resolución recursiva** realiza una petición de resolución de nombres al servidor local, y si no la tiene consulta al servidor raíz que menos tarde en ofrecer la respuesta. Irá preguntando a servidores intermedios hasta llegar al adecuado

## **Definiciones:** Zona, espacio de nombres, dominio, jerarquia, etc

Un **espacio de nombres de dominio** es una estructura con forma de árbol. El elemento raíz no tiene etiqueta, es decir, se denomina con un punto. y cada nodo está compuesto por _un grupo de servidores que se encargan de resolver un conjunto de dominios_. Ese conjunto se denomina **zonas de autoridad**.

## Tipos de registros -> servidores autoritativos

En cada zona de autoridad pueden existir distintos tipos de registros:

- A / AAAA
- CNAME o alias
- NS
- MX
- PTR
- TXT
