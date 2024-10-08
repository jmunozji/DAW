# 4. Modos de conexión del cliente FTP

El modo de transferencia se establece al inicio de las comunicaciones FTP y es dependiente de cómo es el sistema de ficheros del servidor.

La mayoría de los sistemas hacen uso de la estructura de ficheros binarios, antiguos sistemas Unix y mainframe y pueden utilizar la estructura de ficheros ASCII. En cualquier caso, este modo se decide dentro del servidor FTP y el cliente automáticamente detectará cuál de los dos modos usará.

El protocolo FTP se basa en el protocolo TCP, con dos canales de datos con diferentes puertos, uno para enviar los datos y el otro para enviar las órdenes.

## Puertos de conexión
Los puertos que se usan por defecto son:
- Puerto número 20, para el canal de datos de transmisión
- Puerto número 21, para el canal de control

Dentro del protocolo FTP se definen dos modos de conexión que se configuran dentro del servicio, el modo ftp activo y el modo ftp pasivo.

## Modo FTP activo
En el modo FTP activo, el cliente conecta aleatoriamente por un puerto más grande de 1024 (denominemos N) hacia el puerto 21 de órdenes del servidor. 

El cliente inicia la escucha al puerto (N+1) y envía el orden FTP al puerto (N+1) del servidor FTP. El servidor conecta de nuevo al cliente por el puerto de datos especificado por parte del cliente, que es el puerto 20.

Cuando trabajáis en modo activo se tiene en cuenta el cortafuego del sistema. El cortafuego tiene que tener los puertos abiertos de trabajo del servidor y del cliente, para poder establecer las comunicaciones.

Puertos que se abrirán en modo activo dentro del servidor:

1. El cliente conectará al puerto 21 del servidor FTP con un puerto más grande de 1024 del cliente. (Iniciación de la conexión del cliente)
2. El puerto 21 del servidor FTP conectará a un puerto más grande de 1024. (El servidor responde al puerto de control del cliente.)
3. El puerto 20 del servidor FTP conectará a un puerto más grande de 1024. (El servidor inicia la conexión de datos hacia el puerto de cliente de datos.)
4. El cliente conectará con un puerto más grande de 1024 hacia el puerto 20 del servidor FTP. (El cliente envía la confirmación de conexión al puerto de datos del servidor FTP.)



FiguraGràfic de funcionamiento del sistema FTP activo

## Modo FTP pasivo
Para evitar que el servidor inicie la conexión al cliente hay otro método de conexión denominado pasivo.

En el método FTP pasivo el cliente inicia las dos conexiones al servidor, resolviendo el problema de control del cortafuego o configuraciones NAT en el filtraje del puerto de datos del servidor hacia el cliente. Como contrapartida hay que tener más rango de puertos abiertos en el servidor.

El cliente, al iniciar la conexión FTP, coge un puerto aleatorio más grande de 1024 N y el siguiente como N + 1.
El primer puerto N hace la conexión por el puerto 21 del servidor y evita hacer la conexión al puerto de datos 20. El cliente hará uso de una orden PASV. 
El servidor abre un puerto aleatorio P más grande de 1024 y le devuelve al cliente con la orden PASV. 
El cliente inicia el canal de datos del puerto (N + 1) al puerto P.

Para controlar el cortafuego en el servidor FTP en modo pasivo abriremos los puertos siguientes:

1. El cliente conectará al puerto 21 del servidor FTP con un puerto más grande de 1024 del cliente. (Iniciación de la conexión del cliente.)
2. El puerto 21 del servidor FTP conectará a un puerto más grande de 1024 del cliente. (El servidor responde al puerto de control del cliente.)
3. Un puerto más grande de 1024 del cliente conectará a un puerto más grande de 1024 del servidor. (El cliente inicia el canal de datos a un puerto aleatorio del servidor.)
4. Un puerto más grande de 1024 del servidor conectará a un puerto más grande de 1024 del cliente. (El cliente inicia el canal de datos a un puerto aleatorio del servidor.)

En la figura al paso 1 el cliente contacta con el servidor por el puerto 21 pidiendo una conexión pasiva con la orden PASV. El servidor responde en el paso 2 con un puerto aleatorio, en el ejemplo 2024, pidiendo al cliente qué puerto es el que usará para abrir el canal de datos. En el paso 3 el cliente inicia el canal de datos del puerto de datos del cliente 1027 al puerto que ha abierto el servidor 2024. En el paso 4 el servidor confirma la conexión.


FiguraGràfic de funcionamiento del sistema FTP pasivo



Con el modo pasivo se resuelven muchos problemas del cliente, pero se amplían los problemas del servidor. Uno de los principales problemas es la apertura de un gran rango de puertos en el servidor para poder iniciar canales de datos.

Una de las ventajas actualmente es que las implementaciones de servidores FTP permiten escoger el rango de puertos que se usarán.

– **práctica configurar vsftpd como pasivo**

Para realizar la configuración dentro de ProFTPD en el modo pasivo, añadís dentro de la configuración del fichero /etc/proftpd/proftpd.conf la directiva:

PassivePorts 1024 2000
El rango de puertos de configuración irá mínimo del puerto número 1024 fines, como máximo, al 65536.

