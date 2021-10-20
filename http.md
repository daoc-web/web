# El protocolo HTTP

Una breve introducción a HTTP se presentó en la [sección anterior](README.md#http), de manera que aquí nos dedicaremos a brindar detalles específicos de su funcionamiento.

Para que se pueda realizar una transacción Http entre un cliente y un servidor, el servidor debe estar activo y listo para recibir conexiones. Las conexiones se reciben sobre un puerto, y el puerto reservado para el protocolo http es el puerto 80 (se puede usar otro, pero habría que especificarlo explícitamente en la URL).

Una transacción típica en Http inicia con un usuario escribiendo una URI en la barra de direcciones de su navegador, por ejemplo `http://daoc.ml/index.html`. Este evento desencadena varias acciones:

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

