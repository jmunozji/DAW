# Clientes FTP

Un cliente FTP (File Transfer Protocol) es una aplicación de software que se utiliza para conectarse a servidores FTP y realizar operaciones de transferencia de archivos entre el cliente y el servidor. Los clientes FTP permiten a los usuarios:

- Conexión a Servidores FTP: Los clientes FTP se utilizan para establecer conexiones con servidores FTP a través de Internet o redes locales.
- Autenticación: Los usuarios pueden iniciar sesión en el servidor FTP proporcionando su nombre de usuario y contraseña. Algunos clientes FTP también admiten autenticación anónima para acceder como usuario anónimo si el servidor lo permite.
- Transferencia de Archivos: Descargar archivos desde el servidor al cliente: Los usuarios pueden descargar archivos o directorios completos del servidor a su propia computadora local. Cargar archivos desde el cliente al servidor: Los usuarios pueden cargar archivos desde su computadora local al servidor FTP para su almacenamiento o distribución.
- Navegación por Directorios: Los clientes FTP permiten a los usuarios navegar por la estructura de directorios en el servidor, lo que les permite acceder a diferentes archivos y carpetas.
- Renombrar y Eliminar Archivos: Los usuarios pueden renombrar archivos en el servidor, eliminar archivos no deseados y realizar operaciones de administración de archivos.
- Gestión de Permisos: Algunos clientes FTP permiten cambiar los permisos de archivos y directorios en el servidor si se tiene el permiso adecuado.
- Gestión de Sesiones: Los clientes FTP gestionan sesiones de transferencia de archivos, lo que facilita la transferencia de múltiples archivos de forma eficiente.
- Seguridad: Los clientes FTP pueden ofrecer funciones de seguridad, como FTPS (FTP sobre SSL/TLS) o SFTP (SSH File Transfer Protocol) para cifrar las comunicaciones entre el cliente y el servidor.
- Registro de Actividad: Algunos clientes FTP registran las actividades realizadas durante la sesión, lo que facilita la solución de problemas y la auditoría.
- Configuración de Opciones: Los usuarios pueden configurar diversas opciones, como la gestión de contraseñas, la elección entre modos activos y pasivos, y otros ajustes específicos del cliente.

## Herramientas gráficas

Existen numerosos clientes FTP gráficos (clientes FTP con interfaces de usuario basadas en ventanas y gráficos) disponibles para diferentes sistemas operativos que facilitan la transferencia de archivos mediante FTP, SFTP y otros protocolos relacionados. A continuación, te presento una lista de algunos de los clientes FTP gráficos populares para distintas plataformas:

### Clientes FTP para Windows:

- FileZilla: FileZilla es uno de los clientes FTP más populares y ampliamente utilizados. Ofrece una interfaz fácil de usar, es de código abierto y es compatible con FTP, FTPS y SFTP.
- WinSCP: WinSCP es un cliente SFTP y SCP gratuito para Windows. Además de la transferencia de archivos segura, también permite la administración de archivos en servidores remotos.
- Core FTP: Core FTP es un cliente FTP con una interfaz gráfica atractiva y muchas características útiles. Ofrece soporte para FTP, FTPS y SFTP.
- Cyberduck: Aunque inicialmente se diseñó para macOS, Cyberduck también está disponible para Windows. Es un cliente FTP/SFTP de código abierto con una interfaz fácil de usar.

### Clientes FTP para macOS:

- Cyberduck: Cyberduck es una opción popular tanto para macOS como para Windows. Ofrece una interfaz intuitiva y es compatible con una variedad de protocolos, incluyendo FTP, SFTP, WebDAV y más.
- Transmit: Transmit es un cliente FTP y SFTP exclusivo para macOS con una interfaz pulida. Ofrece características avanzadas y herramientas de administración de archivos.
- Fetch: Fetch es un cliente FTP de larga data para macOS, conocido por su simplicidad y facilidad de uso. Ofrece soporte para FTP, SFTP y FTP con seguridad.

### Clientes FTP para Linux:

- FileZilla: Aunque FileZilla se desarrolló inicialmente para Windows, también está disponible para Linux. Es uno de los clientes FTP más populares y es compatible con FTP, FTPS y SFTP.
- gFTP: gFTP es un cliente FTP de código abierto para sistemas Linux. Ofrece una interfaz gráfica sencilla y soporta FTP, SFTP y HTTP.
- KDE Konqueror: El navegador de archivos Konqueror en el entorno de escritorio KDE de Linux incluye funciones de cliente FTP. Puedes usarlo para acceder a servidores FTP de manera sencilla.

# Comandos Básicos ftp
Los comandos FTP son instrucciones que un cliente FTP emite al servidor FTP para realizar diversas operaciones, como conectarse al servidor, listar directorios, cargar y descargar archivos, cambiar permisos y más. Estos son algunos comandos FTP comunes junto con ejemplos de uso:

**Conexión al Servidor FTP:**
- `open` o `open [hostname]`: Establece una conexión con un servidor FTP. Ejemplo: `open ftp.example.com`
- `user [nombre de usuario] [contraseña]`: Inicia sesión en el servidor FTP. Ejemplo: `user myuser mypassword`

**Operaciones de Directorio:**
- `cd [directorio]`: Cambia el directorio en el servidor FTP. Ejemplo: `cd public_html`
- `pwd`: Muestra el directorio de trabajo actual en el servidor. Ejemplo: `pwd`
- `ls` o `dir`: Lista los archivos y directorios en el directorio actual en el servidor. Ejemplo: `ls`

**Transferencia de Archivos:**
- `get [archivo remoto] [archivo local]`: Descarga un archivo del servidor FTP al sistema local. Ejemplo: `get example.txt`
- `put [archivo local] [archivo remoto]`: Carga un archivo desde el sistema local al servidor FTP. Ejemplo: `put localfile.txt remotefile.txt`

**Operaciones en el Sistema de Archivos:**
- `delete [archivo]`: Elimina un archivo en el servidor FTP. Ejemplo: `delete unwanted.txt`
- `chmod [permisos] [archivo]`: Cambia los permisos de un archivo en el servidor FTP. Ejemplo: `chmod 644 myfile.txt`
- `rmdir [directorio]`: Elimina un directorio vacío en el servidor. Ejemplo: `rmdir old_directory`
- `rmd [directorio]`: Elimina un directorio y su contenido de forma recursiva en el servidor FTP. No todos los servidores FTP admiten este comando. Ejemplo: `rmd old_directory`
- `mkdir [directorio]`: Crea un nuevo directorio en el servidor. Ejemplo: `mkdir new_directory`
- `rename [nombre original] [nuevo nombre]`: Cambia el nombre de un archivo o directorio en el servidor FTP. Ejemplo: `rename oldfile.txt newfile.txt`
- `ls -l [archivo]`: Algunos servidores FTP pueden admitir el comando ls -l para mostrar los permisos de un archivo o directorio. Ejemplo: `ls -l myfile.txt`

**Modo Activo y Pasivo: en el cliente**
- `passive`: Cambia al modo pasivo, útil si el servidor FTP está detrás de un firewall. Ejemplo: `passive`
- `active`: Cambia al modo activo, útil si el cliente FTP está detrás de un firewall. Ejemplo: `active`

**Modo Activo y Pasivo: cambiar el modo de transferencia de datos en el servidor en el servidor**
- `quote PASV`: Cambia al modo pasivo, útil si el servidor FTP está detrás de un firewall. Ejemplo: `quote PASV`
- `quote PORT [dirección]`: Cambia al modo activo, útil si el cliente FTP está detrás de un firewall. Ejemplo: `quote PORT 192,168,1,2,5,25`

**Ayuda y Salida:**
- `help` o `?`: Muestra la lista de comandos FTP disponibles. Ejemplo: `help`
- `quit`: Cierra la sesión FTP y sale del cliente. Ejemplo: `quit`

Hay que tener en cuenta que la disponibilidad de comandos y su sintaxis puede variar según el servidor FTP y la implementación. Además, es importante recordar que FTP no es un protocolo seguro, ya que las credenciales y los datos se transmiten sin cifrar. Si se requiere seguridad, se recomienda utilizar FTPS (FTP con SSL/TLS) o SFTP (SSH File Transfer Protocol), que cifran las comunicaciones y proporcionan un nivel adicional de seguridad.
