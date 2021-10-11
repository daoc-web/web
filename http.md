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

Uno de los componentes que más nos interesará como desarrolladores, y que al menos los principales browsers en sistemas de escritorio presentan con opciones muy similares, son justamente las herramientas para desarrolladores o Developer tools, que podemos presentar simplemente dando click derecho en la página desplegada y seleccionando "Inspect" (o Inspect element). Aquí podremos ver e incluso modificar los elementos de la página web: html, css, javascript, y muchas otras cosas, como por ejemplo las transacciones http.