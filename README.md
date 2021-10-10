# Aplicaciones Web

Una aplicación web es un componente de software que se encuentra alojado en un servidor web, que será eventualmente pedido por un cliente web y que podrá ejecutarse parcialmente en el lado del servidor y parcialmente en el lado del cliente. Permite a un usuario interactuar con un sistema remoto sin necesidad de descargar e instalar una aplicación completa sino solo los elementos que va necesitando para su trabajo específico, gracias al uso de un navegador web.

## Definiciones y términos

### Internet

La Internet (EL Internet???) es una red de redes. Se basa en la idea de interconectar múltiples redes independientes, las cuales pueden tener arquitecturas y diseños muy variados, pero responden a ciertos principios fundamentales que permiten su interacción. 

Tal vez el más fundamental de estos principios actualmente es la interconexión mediante "packet switching", que en general se implementa mediante el protocolo IP (Internet Protocol).

### World Wide Web

Ó WWW, ó Web simplemente, representa la colección de aplicaciones, documentos y recursos en general, a los que se puede acceder mediante la Internet.

Para identificar un recurso de forma única se utiliza una URI (Uniform Resource Identifier). Con esta URI se puede pedir el recurso mediante el protocolo HTTP (Hypertext Transfer Protocol) o HTTPS (HTTP Seguro).

### Recursos Web

Los recursos, como ya dijimos, pueden ser cualquier cosa susceptible de transferirse, o de transferir una representación del mismo, por la internet, y suelen estar agrupados en páginas y sitios web. 

Los sitios web presentan como su interfaz de navegación una colección de páginas web. Una página web presenta información, que puede provenir de varios recursos, y presenta hyperlinks atados a URIs; al dar click en un hyperlink, se pide el recurso ligado a la URI, que muchas veces suele ser otra página web (la cual a su vez agrupa recursos y así...).

### Página Web

Lo que normalmente se conoce como una página web es un archivo Html o HyperText Markup Language. Html es un lenguaje de marcado, familia del XML (eXtensible Markup Language), derivados ambos del SGML (Standard Generalized Markup Language).

El Html permite definir la estructura y contenido de una página web, y si bien permite especificar cierto tipo de formato también, en la actualidad se prefiere utilizar CSS o Cascading Style Sheets para el formato. Html y Css todavía requieren de un componente adicional para brindar la funcionalidad a una página web: el lenguaje de programación JavaScript. Este permite "programar" la página web y brindarle dinamismo cuando ya se encuentra cargada en un navegador. Entre estas tres tecnologías tenemos lo que se conoce como el Frontend de una aplicación web.

Adicionalmente, una página web también consta de imágenes y otros elementos que se referencian en el archivo html, y que son recuperados automáticamente por el navegador para completar la presentación de la página para el usuario.

### HTTP

El Hypertext Transfer Protocol es el protocolo universal de comunicación en la Web. Es un protocolo de aplicación, es decir permite a una aplicación comunicarse con otra aplicación en la Internet.

HTTP utiliza a su vez otros protocolos para la transferencia de datos, más específicamente TCP (o QUIC en HTTP/3, pero por facilidad nos referiremos solo a TCP en lo futuro), de manera que HTTP no se preocupa por la transferencia y el control.

En HTTP se utiliza el modelo Cliente-Servidor. En un extremo de la comunicación hay una aplicación Servidor, que está lista para recibir peticiones de los clientes. En el otro extremo hay una aplicación Cliente que de manera proactiva pide recursos al servidor.

El protocolo HTTP funciona en modo petición-respuesta (request-response). El cliente efectúa una petición y el servidor envía una respuesta; este conjunto petición-respuesta suele conocerse como una transacción. Para realizar una transacción, es necesario que primero se haya establecido una conexión TCP entre los dos puntos.

Inicialmente (HTTP/1.0) era necesario establecer una nueva conexión por cada transacción. Sin embargo, a partir de HTTP/1.1 ya es posible efectuar varias transacciones a través de la misma conexión TCP.

### Servidor Web

Es una aplicación que implementa el protocolo HTTP para proporcionar recursos Web a las aplicaciones cliente.

Un servidor web se encuentra permanentemente listo para recibir conexiones de cualquier cliente y en general recibe simultaneamente conexiones de mútiples clientes simultaneamente.

En un servidor se encuentran cargados los recursos que serán entregados a los clientes. Estos recursos pueden ser estáticos: archivos varios, o pueden ser dinámicos: aplicaciones web. Los recursos estáticos simplemente se envían tal cual al cliente, los dinámicos primero se ejecutan en el servidor, de manera que se envían al cliente ya procesados y muy posiblemente modificados dependiendo de la petición.

Hay una multitud de servidores web disponibles. Los más tradicionales son Apache HTTP Server e Internet Information Server; sin embargo, con el tiempo ha aparecido una gran variedad, como por ejemplo Tomcat o Node.js.

### Cliente Web

Es una aplicación que implementa el protocolo HTTP para solicitar recursos Web a las aplicaciones servidor.

Se los conoce comunmente como browsers o navegadores web y los usamos todo el tiempo: Edge, Chrome, Firefox, Safari, curl, entre muchos otros.

En general el cliente es manipulado por un usuario quien indica el recurso que necesita, sea escribiendo su URI en la línea de direcciones, o dando click sobre un hyperlink, lo cual desencadena una petición y queda a la espera de una respuesta, completando así la transacción. 

Al pedir una página web se suelen encadenar muchas transacciones hasta recuperar todos los recursos web utilizados por la página (imágenes, por ejemplo). Los browsers se suelen encargar automáticamente de pedir todos estos recursos sin necesidad de intervención del usuario.

Un browser tiene como principal responsabilidad permitir la interacción del usuario con la aplicación, y para ello una de sus tareas es formatear la información recibida, de manera que se ajuste a la descripción de la página web, para que el usuario la vea "armoniosamente" (esto, claro, depende de la página web, la cual lleva en sí las definiciones. Si la página web es "fea", se verá "fea" en el browser). Hay browsers más sencillos, como curl, que es un browser de línea de comandos que solamente recupera el recurso y nada más.

### Full Stack: Frontend y Backend

El desarrollo de una aplicación web suele dividirse en frontend y backend, pero aclarando que no son independientes el uno del otro, y ambos deben trabajar al unísono e integrados en un todo coherente. La división sobre todo se hace por la gran complejidad alcanzada por las aplicaciones web, que muchas veces obliga a dividir las actividades y el equipo de desarrolladores en estos dos frentes. En la práctica, realmente, son inseparables, y para desarrollar el frontend será necesario conocer el backend y viceversa.

#### Frontend

El frontend o lado del cliente se preocupa principalmente en producir una interfaz de usuario atractiva, ergonómica y fácil de usar. Las tecnologías involucradas en el frontend son principalmente Html, Css y JavaScript, pero actualmente se suelen utilizar toolkits, librerías o frameworks que facilitan la integración de las mismas; entre los más empleados podemos nombrar Bootstrap, React y Angular.

Uno de los principales problemas con los que debe enfrentar el desarrollo de frontend es el del correcto renderizado de las páginas web.  Esto requiere en gran medida buenas nociones de diseño gráfico, y también tratar con la enorme variedad de dispositivos, navegadores y configuraciones en las que deberán desplegarse las páginas creadas.

#### Backend

El backend o lado del servidor puede decirse que forma la columna vertebral de la aplicación. Se ocupa de proporcionar los servicios que brindará la aplicación, relacionados con el manejo de la información, el acceso a datos y su correcto procesamiento. Aquí se reciben y procesan las peticiones del usuario se interactúa con la capa o lógica del negocio, con las bases de datos y se construye la respuesta.

Las tecnologías involucradas en el backend tienen que ver primero con los servidores web como Node o Apache, luego con los servicios web, como el uso de REST, hay que trabajar con bases de datos SQL y NoSQL y construir programas con lenguajes como PHP o Java.

## HTTP


## Apache HTTP Server

Para servir contenido Web, especialmente si este es dinámico, debemos contar con un servidor Web, y uno de los más utilizados es Apache HTTP Server, el cual permite entregar cualquier tipo de contenido web estático (html, css, js, imágenes, ...) y también permite ejecutar programas en el lado del servidor mediante plugins; una de las tecnologías más utilizadas para la programación del lado servidor con Apache es PHP. Una aplicación web generalmente deberá guardar una cierta cantidad de información en alguna base de datos, y una de las más populares suele ser MySQl (o MaríaDB). Esta combinación Apache, PHP y MySQL es tan famosa y utilizada que se le ha dado el nombre del stack AMP, y hay varios instaladores que dependiendo del sistema operativo al que se enfoquen se llaman LAMP (Linux), WAMP (Windows) y XAMPP (varios), entre otros.

Todos estos programas pueden instalarse en cualquier sistema operativo, pero en general los entornos de producción suelen ser Linux, de manera que es buena idea tener una cierta práctica en el uso y configuración de servidores Linux, sin importar la distribución, aunque entre las más usadas podemos nombrar Ubuntu y CentOS. Vamos entonces a ver brevemente cómo instalar y usar AMP sobre Ubuntu Server, mediante VirtualBox (si tiene una computadora extra para instalar físicamente, hágalo, es muy similar).

Primero, para la instalación de una máquina virtual Ubuntu Server, sobre VirtualBox, revise este video: https://youtu.be/pd00qaOSzSg.

Luego, ya con el sistema operativo instalado y funcional, desde un terminal (por que Ubuntu Server no tiene interfaz gráfica), ejecute en orden los siguientes comandos:

- `sudo apt install apache2`

    - Puede probar que está listo si va al browser en el host y pone la URL: `http://<ip_de_la _vm>/`, con lo que debería ver la página de bienvenida de apache.
    - Ó... escriba en el mismo terminal: `curl localhost`, lo que debería presentar como texto el html de la página inicial.

- `sudo apt install mysql-server`

    - Puede probar que está listo poniendo en el terminal de la misma vm: `sudo mysql`. Debería ver mensajes como "Welcome to the MySQL monitor..." y el prompt cambiará a `mysql> `. Para salir escriba `quit;` y Enter.

- `sudo apt install php libapache2-mod-php php-mysql`

    - Puede probar que está listo poniendo en el terminal: `php -v`. Debería ver un mensaje con información de la versión de php.

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