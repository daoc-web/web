# Aplicaciones Web

## Introducción


### Definiciones

#### Internet

La Internet (EL Internet???) es una red de redes. Se basó en la idea de interconectar múltiples redes independientes, las cuales pueden tener arquitecturas y diseños muy variados, pero responden a ciertos principios fundamentales que permiten su interacción. 

Tal vez el más fundamental de estos principios actualmente es la interconexión mediante "packet switching", que en general se implementa mediante el protocolo IP (Internet Protocol).

#### World Wide Web

Ó WWW, ó Web simplemente, representa la colección de aplicaciones, documentos y recursos en general, a los que se puede acceder mediante la Internet.

Para identificar un recurso de forma única se utiliza una URI (Uniform Resource Identifier). Con esta URI se puede pedir el recurso mediante el protocolo HTTP (Hypertext Transfer Protocol) o HTTPS (HTTP Seguro).

#### Recursos Web

Los recursos, como ya dijimos, pueden ser cualquier cosa susceptible de transferirse, o de transferir una representación del mismo, por la internet, y suelen estar agrupados en páginas y sitios web. 

Los sitios web presentan como su interfaz de navegación una colección de páginas web. Una página web presenta información, que puede provenir de varios recursos, y presenta hyperlinks atados a URIs; al dar click en un hyperlink, se pide el recurso ligado a la URI, que muchas veces suele ser otra página web (la cual a su vez agrupa recursos y así...).

#### HTTP

El Hypertext Transfer Protocol es el protocolo universal de comunicación en la Web. Es un protocolo de aplicación, es decir permite a una aplicación comunicarse con otra aplicación en la Internet.

HTTP utiliza a su vez otros protocolos para la transferencia de datos, más específicamente TCP (o QUIC en HTTP/3, pero por facilidad nos referiremos a TCP en lo futuro), de manera que HTTP no se preocupa por la transferencia y el control.

En HTTP se utiliza el modelo Cliente-Servidor. En un extremo de la comunicación hay una aplicación Servidor, que está lista para recibir peticiones de los clientes. En el otro extremo hay una aplicación Cliente que de manera proactiva pide recursos al servidor.

El protocolo HTTP funciona en modo petición-respuesta (request-response). El cliente efectúa una petición y el servidor envía una respuesta; este conjunto petición-respuesta suele conocerse como una transacción. Para realizar una transacción, es necesario que primero se haya establecido una conexión TCP entre los dos puntos.

Inicialmente (HTTP/1.0) era necesario establecer una nueva conexión por cada transacción. Sin embargo, a partir de HTTP/1.1 ya es posible efectuar varias transacciones a través de la misma conexión TCP.

##### Servidor Web

Es una aplicación que implementa el protocolo HTTP para proporcionar recursos Web a las aplicaciones cliente.

Un servidor web se encuentra permanentemente listo para recibir conexiones de cualquier cliente y en general recibe simultaneamente conexiones de mútiples clientes simultaneamente.

En un servidor se encuentran cargados los recursos que serán entregados a los clientes. Estos recursos pueden ser estáticos: archivos varios, o pueden ser dinámicos: aplicaciones web. Los recursos estáticos simplemente se envían tal cual al cliente, los dinámicos primero se ejecutan en el servidor, de manera que se envían al cliente ya procesados y muy posiblemente modificados dependiendo de la petición.

Hay una multitud de servidores web disponibles. Los más tradicionales son Apache Web Server e Internet Information Server; sin embargo con el tiempo ha aparecido una gran variedad, como por ejemplo Tomcat o Node.js.

##### Cliente Web

Es una aplicación que implementa el protocolo HTTP para solicitar recursos Web a las aplicaciones servidor.

Se los conoce comunmente como browsers o navegadores web y son muy conocidos: Edge, Chrome, Firefox, curl, entre muchos otros.

En general el cliente es manipulado por un usuario quien indica el recurso que necesita, sea escribiendo su URI en la línea de direcciones, o dando click sobre un hyperlink, lo cual desencadena una petición y queda a la espera de una respuesta, completando así la transacción. 

Al pedir una página web se suelen encadenar muchas transacciones hasta recuperar todos los recursos web utilizados por la página (imágenes, por ejemplo). Los browsers se suelen encargar automáticamente de pedir todos estos recursos sin necesidad de intervención del usuario.

Un browser tiene como principal tarea formatear la información recibida, de manera que se ajuste a la descripción de la página web, para que el usuario la vea "armoniosamente" (esto, claro, depende de la página web, la cual lleva en sí las definiciones. Si la página web es "fea", se verá "fea" en el browser). Hay browsers más sencillos, como curl, que es un browser de línea de comandos. Simplemente recupera el recurso y nada más.

