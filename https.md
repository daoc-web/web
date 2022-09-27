# HTTPS

HTTP envía la información en claro, sin ningún tipo de cifrado o encriptamiento, de manera que es inseguro y nada apropiado para enviar información sensible, como claves.

HTTPS añade una capa de seguridad mediante el empleo de [TLS/SSL](https://es.wikipedia.org/wiki/Seguridad_de_la_capa_de_transporte), que es un mecanismo de clave pública-privada, con validación por una autoridad certificada (CA).

El uso de HTTPS es transparente para las aplicaciones, es decir una aplicación web funcionará sobre http o https sin necesidad de ninguna adaptación. Lo que es necesario es que los navegadores y servidores soporten https. Todos los navegadores actuales lo hacen, y son los servidores los que deben configurarse con un certificado para usar https.

El proceso de una conexión HTTPS es razonablemente complejo, así que vamos a presentar aquí una versión simplificada del mismo:

- HTTPS usa el puerto 443 por defecto (HTTP usa el 80)
- El cliente se conecta con el servidor e inicia el "handshake"
- El servidor le responde enviando su certificado público (su clave pública más información de la autoridad certificadora), así como parte de la información para calcular una pre-clave
- El cliente valida el certificado con la autoridad certificada (quien emitió el certificado)
- Si se valida, el cliente envía al servidor otra parte de la información para la pre-clave, cifrada con la clave pública del servidor
- El servidor descifra la información con su propia clave privada
- Ambos calculan la clave simétrica para la sesión (con la información de la pre-clave intercambiada), que será la misma en los dos lados. Con esto termina el "handshake"
- A partir de aquí, todos los mensajes HTTP que se envíen mientras dure esta sesión, serán cifrados y descifrados automáticamente por la capa de seguridad, con esta clave simétrica.

## Certificado de seguridad para el servidor

Para poder trabajar con HTTPS es imprescindible que el servidor cuente con un certificado de seguridad, y hay varias alternativas disponibles:
- En primer lugar, existen varias soluciones pagadas. Si tiene su sitio alojado en algún servicio comercial, por ejemplo GoDaddy, Amazon, ..., directamente le ofrecerán este servicio. Hay también empresas que le ofrecerán el servicio independientemente de dónde tenga alojado su servidor. Si prefiere esta opción encontrará mucha información en el internet para que pueda elegir.
- Si prefiere alternativas gratuitas, dependerá un poco de la ubicación de su servidor y de ciertas configuraciones:
    - Si ya tiene un nombre de dominio propio ligado a su servidor, puede conseguir un certificado oficial gratuito con [Let's Encrypt](https://letsencrypt.org/) ([aquí un tutorial](https://noviello.it/es/como-instalar-lets-encrypt-para-apache-en-ubuntu-20-04-lts/)).
        - Si no tiene un nombre de dominio propio, pero tiene una IP fija, puede conseguir un dominio gratuitamente en [FreeNom](https://www.freenom.com/) y luego usar Let's Encrypt.
    - Si su IP es variable, como sucede en una red personal o en su casa, puede conseguir un dominio con [No-Ip](https://www.noip.com/), quienes también le ofrecen un certificado gratuito.
    - Puede generar un certificado "self-signed". Esta opción da problemas con clientes que se conectan desde el internet, ya que estos certificados no se pueden validar, al no ser generados por una autoridad reconocida. Para redes locales o pruebas internas, sin embargo, funciona muy bien. Esta es la alternativa que veremos a continuación, por que adicionalmente nos permite revisar los detalles de todo el proceso que generalmente se sigue si desea obtener un certificado directamente de una autoridad certificadora como Digicert.
    
### Certificado "self-signed" (autofirmado)

Los certificados de seguridad TLS/SSL no solo permiten cifrar la comunicación entre las partes, sino también validar que las partes, principalmente el servidor, es quien dice ser. Esta validación se hace, primero, revisando que el certificado es **oficial**, es decir emitido por una autoridad certificadora o *Certificate Authority* (CN). También se los llama certificados raíz.

Los certificados self-signed los emitió usted mismo, por lo tanto no hay ninguna CA que los pueda validar, y esto ocasionará que los clientes (browsers) los rechacen.

> Si usted va a un banco y dice que es Pedro Pérez, le van a pedir su certificado de identidad **oficial**, emitido por el Registro Civil, para validar que en efecto es Pedro Pérez. Un certificado self-signed, al no ser oficial, es como un carné que usted mismo se hizo. Ningún banco se lo va a aceptar!

Ahora, sabiendo esto, como queremos nuestro certificado para uso local y de pruebas, para lo cual funcionan tan bien como los oficiales, vamos a verificar que tengamos las herramientas o software necesario: [OpenSSL](https://www.openssl.org/). OpenSSL se lo puede instalar fácil y gratuitamente en cualquier sistema. A partir de ahora vamos a asumir que estamos trabajando en un sistema Ubuntu (será muy parecido para otras distribuciones Linux), por la línea de comando.

Si no tenemos todavía OpenSSL (aunque suele venir cargado por defecto), podemos instalarlo con:

```sh
sudo apt install openssl
```

#### Certificado raíz o de CA

El primer paso será crear nuestra propia CA. Una CA está representada por su certificado, de manera que vamos a crear un certificado raíz de CA, que luego podremos usar para crear y validar certificados para otros servidores en nuestra red local o entorno de pruebas. Use el siguiente comando:

```sh
openssl req -x509 -newkey rsa:2048 -days 3650 -keyout CA_privkey.pem -out CA_cert.pem
```

>- `openssl` es la utilidad genérica que se utiliza para la adminitración y manejo de certificados
>- `req` es el comando que efectúa la petición (request) de generación de un certificado
>- `-x509` es la opción que genera el certificado autofirmado, en este caso un certificado raíz para una CA
>- `-newkey` genera la nueva clave con el cifrado indicado: `rsa:2048` (recomendado)
>- `-days` el número de días durante los cuales el certificado será válido
>- `-keyout` indica el nombre del archivo donde se guardará la clave privada generada
>- `-out` indica el nombre del archivo donde se escribirá el certificado

>Al usar este comando el sistema nos hará una serie de preguntas, a las que en general se puede simplemente responder con `Enter` para que el sistema use el valor por defecto, o con '.' para dejar el campo en blanco.
>- El valor que sí es necesario dar es un password para la clave privada de la CA (es la primera pregunta que nos hace el sistema). Guarde bien este password ya que lo necesitará para firmar los certificados que genere luego.
>- En el ejemplo verán que se respondió con valores a dos preguntas: *OrganizationalName=Pollos Refritos* y *Common Name=pollos.com*. Para nuestra CA, ninguno de estos valores es realmente significativo, puede dejarlo en blanco si desea.

```
Generating a 2048 bit RSA private key
......................+++
.........+++
writing new private key to 'CA_privkey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:.
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:Pollos Refritos
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server's hostname) []:pollos.com
Email Address []:.
```

Como producto de esta ejecución obtendrá dos archivos: CA_privkey.pem, que es la clave privada de su CA y CA_cert.pem, que es el certificado de seguridad de su CA.

>La extensión .pem se refiere a [Privacy Enhanced Mail (PEM)](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) que es el formato comúnmente aceptado de este tipo de archivos.

Estos dos archivos se usarán cada vez que desee generar un certificado para algún equipo o programa servidor (o cliente, que también pueden tenerlos).

#### Generar el certificado para el servidor

Cada vez que necesite generar un nuevo certificado para un servidor deberá seguir estos pasos:

Primero es necesario generar la clave privada del servidor:

```
openssl genrsa -out pollos_privkey.pem
```

>- `genrsa` genera una nueva clave privada
>- `out` indica el nombre del archivo donde se guardará la clave

Luego, es necesario generar un Certificate Signing Request (CSR). Este es un archivo digamos temporal (luego de generar el certificado puede borrar este archivo .csr), que se va a enviar a la CA con la información para que se emita el certificado del servidor (en este caso la CA es usted mismo):

```
openssl req -new -key pollos_privkey.pem -out pollos.csr
```

>- `req -new` genera el CSR
>- `key` indica el archivo con la clave privada. En este caso lo que generó en el paso anterior
>- `-out` indica el archivo donde se guardará el CSR, en este caso `pollos.csr`

>Al usar este comando el sistema nos hará una serie de preguntas (como cuando generamos el certificado raíz), a las que en general se puede simplemente responder con `Enter` para que el sistema use el valor por defecto, o con '.' para dejar el campo en blanco.
>- **El valor que sí es necesario dar ahora es el *Common Name*. Este valor debe contener el nombre o IP con que se contactará a su equipo.**
>   - si por ejemplo está usando DNS dinámico (avahi o bonjour), y su equipo se llama pollos, deberá poner **pollos.local**
>   - puede poner la IP de su equipo, ej:192.168.0.100
>   - cualquiera sea su decisión, siempre deberá contactar a su equipo con este nombre o el certificado no validará la petición
>   - si decide utilizar *localhost* o *127.0.0.1*, que es posible, entonces su certificado solo validará cuando la conexión se hace desde el interior del mismo equipo!

```
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:.
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:Pollos Refritos
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server's hostname) []:pollos.com
Email Address []:.

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Ahora le toca a la CA procesar este CSR para generar el certificado de su servidor:

```
openssl x509 -req -days 365 -in ocda.csr -CA CA_cert.pem -CAkey CA_privkey.pem -CAcreateserial -out ocda_cert.pem
```

>- `x509 -req` indica que se generará un certificado a partir de un CSR
>- `-days` el número de días de validez del certificado generado
>- `-in` aquí se debe indicar el archivo con el CSR
>- `-CA` el certificado de la CA
>- `-CAkey` la clave privada de la CA
>- `-CAcreateserial` crea un número serial necesario para generar certificados. El serial se crea en un archivo .srl que también se puede considerar como temporal y borrar luego de generar el certificado
>- `-out` aquí se debe indicar el archivo donde se guardará el certificado generado para el servidor

> Al ejecutar el comando el sistema nos pedirá el password de la clave privada de la CA, y generará el certificado

```
Signature ok
subject=/O=Pollos Refritos/CN=pollos.com
Getting CA Private Key
Enter pass phrase for CA_privkey.pem:
```

#### Archivos generados

Recapitulando un poco sobre los archivos relevantes generados y su utilidad (los nombres, claro, serán los que usted puso):

- *CA_privkey.pem* y *CA_cert.pem* son la clave privada y el certificado raíz de su CA. Estos archivos le servirán solo para generar nuevos certificados autofirmados
- *pollos_privkey.pem* y *pollos_cert.pem* son la clave privada y el certificado para su servidor. Con ellos deberá configurar HTTPS en su equipo



[Aquí tiene una guía para aplicaciones Java](https://github.com/daoc/TLS-certificates).

