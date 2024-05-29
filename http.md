# El protocolo HTTP

Una breve introducción a HTTP se presentó en la [sección anterior](README.md#http), de manera que aquí nos dedicaremos a brindar detalles específicos de su funcionamiento.

Para que se pueda realizar una transacción Http entre un cliente y un servidor, el servidor debe estar activo y listo para recibir conexiones. Las conexiones se reciben sobre un puerto, y el puerto reservado para el protocolo http es el puerto 80 (se puede usar otro, pero habría que especificarlo explícitamente en la URL).

Una transacción típica en Http inicia con un usuario escribiendo una URI en la barra de direcciones de su navegador, por ejemplo `http://fiipi.webs.vc/index.html`. Este evento desencadena varias acciones:

- El navegador establece una conexión TCP con el servidor
- El navegador realiza una petición (request) http de acuerdo a los datos de la URI
- El servidor recibe la petición y busca el recurso solicitado (la página index.html)
- El servidor construye una respuesta (response) y la envía al navegador, con la información del recurso solicitado
- El navegador recibe la respuesta y la presenta al usuario en la pantalla del navegador (presenta la página index.html)
- Al ir presentando la página, el navegador analiza si hay más recurso referenciados (imágenes, por ejemplo), y por cada uno de ellos realiza una nueva petición
- Al finalizar de recuperar todos los recursos, ya se puede cerrar la conexión TCP.

## Apache HTTP Server

Para servir contenido Web, especialmente si este es dinámico, debemos contar con un servidor Web, y uno de los más utilizados es Apache HTTP Server, el cual permite entregar cualquier tipo de contenido web estático (html, css, js, imágenes, ...) y también permite ejecutar programas en el lado del servidor mediante módulos (o plugins); una de las tecnologías más utilizadas para la programación del lado servidor con Apache es PHP. 

Una aplicación web generalmente deberá guardar una cierta cantidad de información en alguna base de datos, y una de las más populares suele ser MySQl (o MaríaDB).

Esta combinación Apache, PHP y MySQL es tan famosa y utilizada que se le ha dado el nombre del stack AMP, y hay varios instaladores que dependiendo del sistema operativo al que se enfoquen se llaman LAMP (Linux), WAMP (Windows) y XAMPP (varios), entre otros.

Todos estos programas pueden instalarse en cualquier sistema operativo, pero en general los entornos de producción suelen ser Linux, de manera que es buena idea tener una cierta práctica en el uso y configuración de servidores Linux, sin importar la distribución, aunque entre las más usadas podemos nombrar Ubuntu y CentOS. Vamos entonces a ver brevemente cómo instalar y usar AMP sobre Ubuntu Server, mediante VirtualBox (si tiene una computadora extra para instalar físicamente, hágalo, es muy similar).

Primero, para la instalación de una máquina virtual Ubuntu Server, sobre VirtualBox, revise este video: https://youtu.be/pd00qaOSzSg.

Luego, ya con el sistema operativo instalado y funcional, desde un terminal (por que Ubuntu Server no tiene interfaz gráfica), ejecute en orden los siguientes comandos:

- `sudo apt install apache2`

    - Puede probar que está listo si va al browser en el host y pone la URL: `http://<ip_de_la _vm>/`, con lo que debería ver la página de bienvenida de apache, que es una página estática: index.html, que se encuentra en el directorio /var/www/html.
    - Ó... escriba en el mismo terminal: `curl localhost`, lo que debería presentar como texto el html de la página inicial.

- `sudo apt install mysql-server`

    - Puede probar que está listo poniendo en el terminal de la misma vm: `sudo mysql`. Debería ver mensajes como "Welcome to the MySQL monitor..." y el prompt cambiará a `mysql> `. Para salir escriba `quit;` y Enter.

- `sudo apt install php libapache2-mod-php php-mysql`

    - Puede probar que está listo poniendo en el terminal: `php -v`. Debería ver un mensaje con información de la versión de php.
    - php es el intérprete del lenguaje, libapache2-mod-php es el módulo (o plugin) que permite incorporar php al servidor apache, y php-mysql es el módulo que permite ligar php y mysql.

Ya puede empezar a poner el contenido web en el servidor Apache. Esto debe hacerlo dentro del directorio raíz, el cual es: `/var/www/html`. Aquí puede poner los archivos directamente, o mejor crear un subdirectorio para cada aplicación que vaya a desarrollar, y ahí poner todo el contenido.

> Si bien hacer esto en Linux es un gran ejercicio, si prefiere trabajar directamente en su máquina Windows y "no hacerse lío", puede instalar por ejemplo XAMPP. Para ello revise este video: https://youtu.be/DOZPG4V6-JU.

Hagamos un pequeño ejemplo, creando una página php que brinde información del sistema, en el subdirectorio *prueba*. Ejecute en orden los siguientes comandos:

- `cd /var/www/html`
    > Nos cambiamos al directorio /var/www/html (lo hacemos el directorio actual)
- `sudo mkdir prueba`
    > Creamos en el directorio actual el subdirectorio prueba. Deberá utilizar `sudo` por que el directorio /var/www/html le pertenece al usuario root.
- `cd prueba`
    > Nos cambiamos al nuevo directorio prueba.
- `sudo nano info.php`
    > Esto abre el editor de texto nano en el terminal, creando al mismo tiempo el archivo info.php (o abriéndolo si existiera), donde vamos a escribir nuestro programa.
- Ahora, dentro de la pantalla del editor de texto en el terminal escriba:
    ```php
    <?php
    phpinfo();
    ?>
    ```

    > `<?php` es la etiqueta de apertura php, que indica que a continuación pondremos código php. `phpinfo();` es una función php que presenta la información del sistema, y `?>` es la etiqueta de cierre php, que indica que ya no habrá más código php.

- Grabe el archivo oprimiendo `Ctrl+o` Enter (la tecla control y la letra o, al mismo tiempo, y luego la tecla Enter)
- Cierre el editor oprimiendo `Ctrl+x`

Ahora vaya al browser y escriba en la URL: `http://<ip_de_la _vm>/prueba/info.php`, y verá la información del sistema.

Ya hemos creado nuestra primera aplicación web dinámica!

## Navegador

Para poder recuperar y revisar contenido web necesitamos algún navegador o browser. Cualquiera sirve en principio: Edge, Chrome, Firefox, Safari,... Los navegadores actualmente tratan de mantener ciertos estándares para brindar compatibilidad, y en general tienden a usar los mismos motores de renderizado o "web-engine", principalmente: WebKit (Apple), Blink (Google) y Gecko (Mozilla).

Uno de los componentes que más nos interesará como desarrolladores, y que al menos los principales browsers en sistemas de escritorio presentan con opciones muy similares, son justamente las herramientas para desarrolladores o Developer tools, que podemos presentar simplemente dando click derecho en la página desplegada y seleccionando "Inspect" (o con Ctr+Shift+J en Edge). Aquí podremos ver e incluso modificar los elementos de la página web: html, css, javascript, y también podremos hacer muchas otras cosas, como por ejemplo revisar las transacciones http.

Revisemos nuestra primera transacción Http. Abra su navegador, abra las Developer tools, y ponga en la línea de direcciones la URL: `http://daoc.ml/index.html`. En el panel de las Dev.Tools seleccione, arriba, "Network", y abajo seleccione "index.html". A la derecha seleccione "Headers" y ahí podrá ver los encabezados de la transacción Http. Primero verá información general, y hacia abajo verá información específica de la Response y la Request:

![img01.png](images/img01.png)

En la sección Request verá la petición completa realizada por el browser (oprima "View source" si prefiere), que aquí presentamos resumida, ya que hay muchos encabezados:

```
GET /index.html HTTP/1.1
Host: daoc.ml
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36 Edg/94.0.992.50
Accept: text/html,application/xhtml+xml,
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,es;q=0.8
...
```

> La primera línea en una petición es probablemente la más relevante. Ahí se indica el método http (GET), el recurso solicitado (/index.html) y la versión del protocolo (HTTP/1.1). El resto de líneas corresponden a encabezados, uno por línea, que definen más precisamente las condiciones y características de la petición.

Como resultado de la petición, el servidor emitirá una respuesta, que generalmente contendrá el recurso solicitado (si la petición fue adecuada,claro está). En el mismo sector de headers en las Dev.Tools puede ver los encabezados de la respuesta:

```
HTTP/1.1 200 OK
Date: Tue, 19 Oct 2021 17:44:53 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Thu, 31 Oct 2019 20:30:12 GMT
ETag: "2aa6-5963ab767b796-gzip"
Accept-Ranges: bytes
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 3138
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html

<Cuerpo del mensaje>
```

> También la primera línea de la respuesta es muy relevante, ya que indica sobretodo si se pudo o no hacer lo solicitado. Primero va la versión del protocolo (HTTP/1.1), luego el código de la respuesta, (200) y luego una brevísima descripción del código (OK). En este caso este código indica que sí se pudo hacer lo solicitado, en este caso enviar el recurso "index.html". Las siguientes líneas contienen los encabezados varios que envía el servidor describiendo la respuesta, y luego de una línea en blanco, irá el cuerpo de la respuesta, es decir el recurso solicitado, en este caso el html, que presentamos una pequeña parte del inicio a continuación:

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2016-11-16
    See: https://launchpad.net/bugs/1288690
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    ...
```

> Este html será procesado por el navegador para presentarlo formateado en la pantalla principal.

Luego de cargar el html, el navegador analizará si hay más recursos que solicitar para completar el pedido, y por cada uno de ellos se hará otra transacción. En este caso se pide también la imagen "ubuntu-logo.png", para la cual también puede revisar en las Dev.Tools los encabezados de su petición-respuesta.

## Estructura sintáctica de una URI típica

Los recursos se referencian mediante una URI (Uniform Resource Identifier), las cuales deben guardar una cierta estructura de base:

```
<scheme>://<host>:<port>/<path>?query>#<frag>
```

- El *scheme* o esquema, es el protocolo, en este caso http ó https
- *host*, es el nombre o la dirección ip del servidor
- *port*, se refiere al puerto de escucha del servidor. Cuando no se incluye se asume un valor por defecto (en este caso 80 ó 443)
- *path*, es la ubicación del recurso dentro del servidor
- *query*, son posibles parámetros o atributos que se pasan al servidor como pares clave-valor. Permiten dar información adicional sobre la petición,cuando el recurso es una aplicación (?clave1=valor1&clave2=valor2…). Note el símbolo '?' que divide el path de la query
- *frag*(fragmento o ancla), es la ubicación de una parte interna al recurso (#cap2). Se suele indicar la id de un elemento interno al recurso y generalmente se procesa en el navegador, para que dicho elemento sea visible en la pantalla

Una URI solo puede utilizar un cierto grupo de caracteres. Los caracteres válidos son algunos de los pertenecientes al código US-ACII en 7 bits, es decir los códigos 0 al 127: `A-Z, a-z, 0-9, -, ., _, ~, :, /, ?, #, [, ], @, !, $, &, ', (, ), *, +, ,, ;, %, and =.`.

Más aún, hay un cierto subset de caracteres dentro de este grupo que están reservados y no pueden usarse libremente puesto que tienen un significado especial: `! * ' ( ) ; : @ & = + $ , / ? % # [ ]`

En ocasiones es necesario utilizar estos caracteres reservados, en algún parámetro por ejemplo, para lo cual se puede utilizar URL Encoding, que es una forma de reemplazar cualquier caracter. Esto se hace poniendo el símbol de porcentaje '%' seguido del código numérico del caracter en la tabla ASCII. Algunos ejemplos:

```
%20 -> espacio
%3D -> =
%25 -> %
%3F -> ?
...
```

## Estructura de los mensajes HTTP

Los mensajes http pueden ser una petición o una respuesta, y ambos deben guardar un formato específico para poder ser procesados apropiadamente.

Una petición debe configurarse de la siguiente manera:

```
<método> <recurso> <versión>
<encabezados>*

<cuerpo del mensaje>?
```

> El método indica básicamente qué queremos hacer, y hay varias alternativas GET y POST siendo las más utilziadas.

> El recurso indica aquello que queremos manipular en el servidor (recuperar, modificar, ...)

> La versión indica la versión del protocolo HTTP con la que está estructurado el mensaje y con la que se prefiere trabajar: 1, 1.1, 2, ...

> Los encabezados pueden ser ninguno o muchos, uno por cada línea, y deben tener también un formato específico: `<nombre>: <valor>`. El nombre del encabezado, seguido de dos puntos, seguido de una  cadena de texto con el valor para dicho encabezado.

> Luego de los encabezados debe ir una línea en blanco, y finalmente el cuerpo del mensaje, que es opcional, y que no tiene ningún formato en particular (se puede enviar prácticamente cualquier cosa). De hecho, que haya o no cuerpo del mensaje depende mucho del método http usado. Por ejemplo GET nunca lleva cuerpo y POST casi siempre lo lleva.

La respuesta debe configurarse de la siguiente manera:

```
<versión> <código> <descripción>
<encabezados>*

<cuerpo del mensaje>?
```

> La versión indica la versión del protocolo HTTP con la que se responde (generalmente la misma de la petición)

> El código indica el estado de la petición, se hizo, no se hizo, qué pasó?... Hay muchos códigos posibles, pero por ejemplo, 200 indica que sí se cumplió la solicitud y 404 que no se encontró lo solicitado

> La descripción está atada al código, y es un pequeño texto descriptivo, por ejemplo para 200 es "OK" y para 404 es "Not Found"

> Igual que para la petición, puede haber una serie de encabezados

> Luego de los encabezados debe ir una línea en blanco, y finalmente el cuerpo del mensaje, que también es opcional y sin un formato en particular. Lo más común, pero, es que en una respuesta haya un cuerpo del mensaje con la información solicitada (la página html, la imagen, ...)

## Métodos HTTP

En una petición el primer elemento que se idica es el método http, el cual indica el tipo de acción que se pide al servidor que ejecute. Hay varios métodos, pero al trabajar con html los dos más usados son GET y POST (también por que son los únicos válidos en un formulario html) y de manera más amplia los más utilizados son GET, POST,PUT y DELETE, que permiten definir operaciones tipo CRUD sobre los recursos. Cada método tiene su significado específico:

- GET: Solicita un recurso, lo recupera del servidor
- POST: Permite crear un nuevo recurso con la información contenida en el cuerpo del mensaje. El nuevo recurso generalmente es o forma parte de aquel definido en la URI
- PUT: Permite modificar el recurso definido en la URI, con la información en el cuerpo del mensaje
- DELETE: Elimina el recurso definido en la URI
- Otros métodos en los que no nos detendremos son: OPTIONS, HEAD, TRACE, CONNECT y PATCH (puede revisar https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

## Códigos de estado

Como vimos, una respuesta lleva incluido un código que nos indica de cierta manera qué pasó al  tratar de resolver la petición, si se pudo hacer lo solicitado, si hubo algún tipo de error, si es necesario ir a buscar el recurso en otro lado, entre otras posibilidades.

Los códigos de estado son un número de tres cifras, y se agrupan en familias de códigos:

- 1xx (100 - 199): respuestas meramente informativas, ej:
    - 101 Switching Protocol
- 2xx (200 - 299): peticiones que se pudieron cumplir correctamente, ej:
    - 200 OK
- 3xx (300 - 399): mensajes de redirección
    - 302 Moved Permanently
- 4xx (400 - 499): errores en el lado del cliente, ej:
    - 404 Not Found
- 5xx (500 - 599): errores en el lado del servidor
    - 500 Internal Server Error

## Algunos encabezados comunes

Los encabezados pueden ir tanto en la petición como en la respuesta y permiten ampliar su significado o definirla de manera más precisa. Hay una gran variedad, pero algunos ejemplos típicos que se suelen encontrar pueden ser:

- Accept: \*/\*
    - Va en la petición y le dice al servidor el tipo de contenido que se prefiere recibir y procesar
        - Accept: image/jpg
        - Accept: application/json
- Content-Type: text/html; charset=iso-latin-1
    - Se refiere al tipo de contenido que se envía (petición o respuesta)
        - Content-Type: text/plain
        - Content-Type: application/octet-stream
- Set-Cookie: animal=perro; raza=pug
    - Va en la respuesta. El servidor pide al cliente que guarde los valores indicados
- Cookie: animal=perro; raza=pug
    - Va en la petición. El cliente envía los valores guardados al servidor
- Authorization: Basic dXN1YXJpbzpwYXNzd29yZA==
    - Envía las credenciales del usuario para autenticarse en el servidor
    - Las credenciales están codificadas en Base64, y contienen "usuario:password"
- WWW-Authenticate: Basic
    - Pide al cliente que se identifique
        - Generalmente se acompaña de un código 401
- Location: http<span>://www<span>.example.org/index.php
    - Redirecciona al cliente a otro recurso
        - Generalmente se acompaña de un código 302

## Tipos de los recursos (MIME)

Los recursos web pueden ser de una gran variedad, y suele ser necesario especificar el tipo del recurso que se desea recibir o que se está enviando.

Para especificar los tipos se usa el estándar MIME: Multipurpose Internet Mail Extensions. MIME utiliza un formato `tipo/subtipo`. El tipo es una categoría general, por ejemplo imagen, y el subtipo indica con exactitud la naturaleza del dato, por ejemplo jpeg: `image/jpeg`.

Si es necesario se puede añadir al tipo un parámetro con detalles adicionales:
`tipo/subtipo;parameter=value`, por ejemplo `text/plain;charset=UTF-8`.

Los tipos se encuentran registrados en la Internet Assigned Numbers Authority, IANA (https://www.iana.org/assignments/media-types/media-types.xhtml), donde puede consultar la lista.

> Tipos muy comunes son text/html, application/pdf, image/png, application/json, o application/octet-stream (genérico para cualquier contenido binario).

## Base64

Por Http se puede enviar cualqueir tipo de información, en cualquier encodaje, texto o binario. Sin embargo, algunos formatos, sobretodo binarios, pueden dar problemas en la práctica si son mal interpretados (algunos caracteres pueden cerrar la conexión, por ejemplo).

Para evitar cualquier problema de interpretación, se suele convertir algunos mensajes, sobretodo binarios, a un formato llamado Base64, que no es más que recodificar los bytes en 6 bits, en lugar de ocho. En 6 bits se puede representar hasta 64 caracteres, razón del nombre del formato.

Primero se toma el valor binario de cada caracter, según la tabla [ASCII](https://htmlpreview.github.io/?https://github.com/dordonez-ute-apweb/web/blob/main/Ascii.htm), se reempaqueta en grupos de 6 bits, y luego se ve en la tabla [Base64](https://htmlpreview.github.io/?https://github.com/dordonez-ute-apweb/web/blob/main/Base64.htm), donde para cada valor (en decimal) entre 0 y 63, tiene atado un caracter, con los que se construye la codificación en Base64.

En todos los lenguajes de programación hay funciones que permiten efectuar estas conversiones, sin embargo, para clarificar, un pequeño ejemplo.

Supongamos que queremos enviar el texto original Ascii "hola", codificado en Base64:

```
hola
h       |o       |l       |a
    en la tabla ASCII:
01101000|01101111|01101100|01100001
    reempaquetado en 6 bits:
011010|000110|111101|101100|011000|010000
    en la tabla Base64:
a     |G     |9     |s     |Y     |Q     
aG9sYQ==
```

> Los símbolos de igual (=) al final son caracteres de relleno o padding que completan la cadena. Para facilitar la conversión de Base64 al valor original, la cadena de caracteres en Base64 debe tener un múltiplo de 4 caracteres, si faltan en el útimo bloque se completa con el padding.

## Un poco de historia de HTTP

HTTP nace con la Web. Todo inicia con una propuesta escrita por Tim Berners-Lee, del CERN, en 1989. Esta propuesta da lugar a html y http, así como al primer servidor y al primer navegador. En 1991 ya había tráfico web fuera del CERN. A continuación destacaremos ciertos elementos relevantes en cada versión (pero la lista es muy grande!).

### HTTP/0.9

La primera versión era sumamente simple. La petición consistía solo de la instrucción GET seguida del path al recurso, y la respuesta era directamente el recurso, sin ningún tipo de encabezados; todo era puro html.

### HTTP/1.0

Aparece en 1997. Se añade la versión del protocolo a la petición y se añaden ya los encabezados. Era posible ya enviar otro tipo de documentos que no fueran html, y la respuesta ya incluía el código de estado.

Era necesario hacer una conexión TCP nueva por cada transacción (si el html referenciaba 4 imágenes, se requerían 5 conexiones en total).

### HTTP/1.1

Se publica apenas pocos meses luego de la versión 1.0. Ya era posible reutilizar una conexión para varias transacciones y además se podía hacer pipelining, es decir enviar varias peticiones al hilo sin tener que esperar por la respuesta. Se añadió el encabezado Host, que permitía que una misma IP (servidor) aloje varios dominios.

Mientras se seguía evolucionando la versión 1.1, aparecieron HTTPS y las APIs REST para servicios web.

### HTTP/2

Aparece oficialmente en 2015. Es un protocolo binario, ya no en modo texto. Es multiplexado, lo que permite que las respuestas a varias peticiones (pipelining) puedan ser recibidas fuera de orden, es decir evita el bloqueo [head-of-line](https://en.wikipedia.org/wiki/Head-of-line_blocking) sobre HTTP (aunque este subsiste a nivel de TCP).

Implementa también la compresión de los encabezados y permite usar "push" para que el servidor envíe información por adelantado (antes de la petición) al cliente.

### HTTP/3

Todavía es experimental, aunque ya hay ciertos servidores y clientes que lo implementan. Tal vez el mayor cambio es que ya no utiliza TCP, sino un protocolo basado en UDP llamado QUIC. Esto ya evita el bloqueo [head-of-line](https://en.wikipedia.org/wiki/Head-of-line_blocking) sobre TCP, y también se supone que lo hace más rápido.

Siguiente: [El protocolo HTTPS](https.md)
