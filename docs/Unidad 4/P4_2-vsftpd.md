# Práctica 4.2 - Desplegar un servidor FTP con vsftpd

Para ello vamos a partir de la instancia AWS creada con Debian que realizamos en la [P1.1. Linux Server en AWSAcademy](https://jmunozji.github.io/DAW/Unidad%201/P1_1/)). En el caso de que hayas eliminado esa instancia, vuelve a crearla.

Para esta práctica hemos elegido como servidor SFTP **vsftpd** frente al servidor **proFTPD**


## Instalación del servidor vsftpd

En primer lugar, actualizaremos los repositorios de Ububtu y a continuación instalaremos el **servidor vsftpd** :

```sh
sudo apt-get update
sudo apt-get install vsftpd
```
Ahora vamos a crear una carpeta en nuestro *home* en Debian que llamaremos ftp:

```sh
mkdir /home/nombre_usuario/ftp
```
En la configuración de *vsftpd* indicaremos que este será el directorio al cual vsftpd se cambia después de conectarse el usuario.

Dentro de la carpeta /home/nombre_usuario crea un archivo que utilizaremos para luego poder descargarlo, de la siguiente forma

```sh
sudo nano /home/nombre_usuario/descargar.txt
```

Escribe tu nombre y apellidos dentro del archivo **descargar.txt**

### Certificados de Seguridad con OpenSSL

Ahora vamos a crear los certificados de seguridad necesarios para aportar la capa de cifrado a nuestra conexión (algo parecido a HTTPS) y para ello utilizaremos OpenSSL

```sh
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
```

## Configuración del servidor vsftpd

Y una vez realizados estos pasos, procedemos a realizar la configuración de *vsftpd* propiamente dicha. Para ello buscamos el archivo de configuración y guardamos una copia de él por si acaso. Haz una copia: 

```sh
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.confinicial
```

Pasamos a modificar el archivo de configuración de este servicio **vsftpd.conf** utilizando un editor como puede ser *nano* o *vim* :

```sh
sudo nano /etc/vsftpd.conf
```

1. En primer lugar, buscaremos las siguientes líneas del archivo y las **eliminaremos por completo**:

```linuxconfig
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
```

2. Tras ello, **añadiremos** estas líneas en su lugar;

```linuxconfig 
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH

local_root=/home/nombre_usuario/ftp
```

## Iniciar el servicio y habilitamos para que se inicie automáticamente al arrancar:

```sh
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

## Agrega usuarios FTP

Para que los usuarios se puedan conectar al servidor FTP se deben crear credenciales, para ello debes crear cuentas de usuario y asignarles permisos en el sistema de archivos. Puedes usar el comando **useradd** para crear usuarios. En nuestro caso, vamos a crear el usuario **usuftp** con contraseña la misma, **usuftp**.

```sh
sudo adduser usuftp
```

A continuación crearemos un archivo nuevo : 

```sh
sudo nano /etc/vsftpd.userlist
```
Y añade dentro de este archivo el usuario **usuftp** y guardalo.

## Reinicia el servicio 

Finalmente **reiniciamos el servicio** para que coja la nueva configuración realizada en todos estos pasos.

```sh
sudo systemctl restart --now vsftpd
```

## Comprobar la Conexión FTP al servidor vsftpd

> Para poner realizar una conexión FTP al sercidor FTP, debemos tener en cuenta si el modo de acceso es 
*Mediante el puerto por defecto del protocolo <u>inseguro</u> FTP*, el **puerto 21**, pero utilizando certificados que cifran el intercambio de datos convirtiéndolo así en <u>seguro</u> o *haciendo uso del protocolo SFTP*, que es un protocolo dedicado al intercambio de datos mediante una conexión similar a SSH, utilizando de hecho el **puerto 22**.

!!!task "Tarea"
    Configura un nuevo dominio (nombre web) para el .zip con el nuevo sitio web que os proporcionado. 
    **En este caso debéis transferir los archivos a vuestra Debian mediante SFTP.**

Tras acabar esta configuración, ya podremos acceder a nuestro servidor mediante un cliente FTP (como FileZilla, WinSCP o la línea de comandos) para conectarte al servidor FTP utilizando la dirección IP del servidor y las credenciales del usuario FTP que creaste.

La que vamos a intentar es realizar una transferencia de archivos entre nuestro servidor FTP en Debian y el cliente FTP (nuestro ordenador). 
Tras acabar esta configuración, ya podremos acceder a nuestro servidor mediante un **cliente FTP** adecuado, como por ejemplo 

    
#### Acceso con Cliente FTP de consola

1.Abre una terminal en tu sistema. 
- Desde Linux/Mac, abre el terminal del sistema
- Desde Windows, abre el "Símbolo del sistema" o "PowerShell". Puedes hacerlo buscando "cmd" o "PowerShell" en el menú de inicio o escribiendo "cmd" en la barra de búsqueda.

2.En la terminal, escribe el siguiente comando para iniciar una sesión FTP. Debes reemplazar *nombre_de_host_ftp* con la dirección IP PÚBLICA o el nombre de dominio del servidor FTP al que deseas conectarte:

```
ftp nombre_de_host_ftp
```

3. Una vez que ingreses el comando, el cliente FTP intentará establecer una conexión con el servidor. Si la conexión es exitosa, verás un mensaje similar a este:

```
Connected to nombre_de_host_ftp.
220 (nombre_del_servidor_ftp) FTP server ready
Name (nombre_de_host_ftp:tu_nombre_de_usuario_ftp):
```

4. A continuación, el cliente FTP te pedirá que ingreses un usuario (en nuestro caso recuerda que era **usuftp**)  y presiona "Enter". Luego, se te pedirá que ingreses la contraseña (recuerda que era **usuftp**). Si las credenciales son correctas, deberías obtener acceso al servidor FTP. Verifica que el prompt a cambiado a ftp> (quiere decir que has conectado correctamente).

5. *Una vez conectado*, puedes utilizar **comandos FTP** para realizar diversas acciones, como listar directorios, descargar archivos, cargar archivos, eliminar archivos y más. Aquí tienes algunos comandos FTP básicos, :

> `help`  (Utilizando el comando help podemos contar con ayuda interactiva acerca de todos los comandos disponibles.)  
> `ls`  (Lista los archivos y directorios en el directorio remoto.)  
> `cd`  (Cambia de directorio en el servidor remoto.)  
> `get`  (Descarga un archivo desde el servidor remoto.)  
> `put`  (Carga un archivo en el servidor remoto.)  
> `delete` (Elimina un archivo en el servidor remoto.)  
> `quit` (Cierra la sesión FTP y sale del cliente FTP.) 


Descarga el archivo **descargar.txt** que habías creado en /home/nombre_usuario 

Modifícalo en tu ordenador añadiendo la frase "lo he modificado en mi ordenador" y lo guardas como **descargar_modificado.txt**
Y posteriormente lo subes a la misma ubicación.

Cuando hayas terminado, puedes usar el comando quit para salir de la sesión FTP.

#### Acceso con Cliente FTP gráfico 

Vamos a utilizar como **cliente FTP** con entorno gráfico a [Filezilla](https://filezilla-project.org/), que dispone de versiones para GNU/Linux, Mac OS X y Windows. Tras descargar <U>**el cliente FTP**</u> en nuestro ordenador, introducimos los datos necesarios para conectarnos a nuestro servidor FTP en Debian:

![](../img/ftp1.png)

+ La IP de Debian (recuadro rojo)
+ El nombre de usuario de Debian (recuadro verde)
+ La contraseña de ese usuario (recuadro fucsia)
+ El puerto de conexión, que será el 21 para conectarnos utilizando los certificados generados previamente (recuadro marrón)

Tras darle al botón de *Conexión rápida*, nos saltará un aviso a propósito del certificado, le damos a aceptar puesto que no entraña peligro ya que lo hemos genrado nosotros mismos:

![](../img/ftp2.png)

Nos conectaremos directamente a la carpeta que le habíamos indicado en el archivo de configuración `/home/raul/ftp`

Una vez conectados, buscamos la carpeta de nuestro ordenador donde hemos descargado el *.zip* (en la parte izquierda de la pantalla) y en la parte derecha de la pantalla, buscamos la carpeta donde queremos subirla. Con un doble click o utilizando *botón derecho > subir*, la subimos al servidor.

![](../img/ftp3.png)

Si lo que quisiéramos es conectarnos por **SFTP**, exactamente igual de válido, haríamos:

![](../img/ftp5.png)

Fijáos que al utilizar las claves de SSH que ya estamos utilizando desde la Práctica 1, no se debe introducir la contraseña, únicamente el nombre de usuario.

Puesto que nos estamos conectando usando las claves FTP, nos sale el mismo aviso que nos salía al conectarnos por primera vez por SSH a nuestra Debian, que aceptamos porque sabemos que no entraña ningún peligro en este caso:

![](../img/ftp6.png)

![](../img/ftp7.png)

Y vemos que al ser una especie de conexión SSH, nos conecta al `home` del usuario, en lugar de a la carpeta `ftp`. A partir de aquí ya procederíamos igual que en el otro caso.

Recordemos que debemos tener nuestro sitio web en la carpeta `/var/www` y darle los permisos adecuados, de forma similiar a cómo hemos hecho con el otro sitio web. 

El comando que nos permite descomprimir un *.zip* en un directorio concreto es:

```sh
unzip archivo.zip -d /nombre/directorio
```

Si no tuvieráis unzip instalado, lo instaláis:

```sh
sudo apt-get update && sudo apt-get install unzip