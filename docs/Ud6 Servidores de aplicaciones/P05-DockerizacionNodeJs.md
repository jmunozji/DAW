---
title: 'Práctica 5 - Dockerización del despliegue de una aplicación con Node.js'
---

# Práctica 5 - Dockerización del despliegue de una aplicación Node.js

## Introducción

En este caso vamos a Dockerizar la aplicación que ya desplegamos en la [Práctica 4 - Despliegue de aplicaciones con Node Express](P04-NodeJS-Express.md).

### ¿Por qué *dockerizar*?

[Si uno trata de informarse](https://www.campusmvp.es/recursos/post/los-beneficios-de-utilizar-docker-y-contenedores-a-la-hora-de-programar.aspx), encontrará [múltiples y variadas razones](https://developerexperience.io/practices/dockerizing) para dockerizar nuestras aplicaciones y servicios.

Por citar sólo algunas:

**1. Configuración rápida del entorno en local para el equipo de desarrollo:** si todos los servicios están implementados con contenedores, es muy rápida la configuración de dicho entorno.

**2. Evita el clásico "en mi máquina funciona":** gran parte de los problemas de desarrollo provienen de la propia configuración que los integrantes del equipo de desarrollo tienen de su entorno. Con los servicios en contenedores, esto queda solucionado en gran medida.

**3. Despliegues más rápidos**

**4. Mejor control de versiones:** como ya sabéis, se puede etiquetar (tags), lo que ayuda en el CONTROL DE VERSIONES.

**5. Rollbacks más fáciles:** puesto que se tienen las cosas mas controladas por la versión, es más fácil revertir el código. A veces, simplemente apuntando a su versión de trabajo anterior.

**6. Fácil configuración de múltiples entornos:** como hacen la mayoría de los equipos de desarrollo, se establece un entorno local, de integración, de puesta en escena (preprod) y de producción. Esto se hace más fácil cuando los servicios están en contenedores y, la mayoría de las veces, con sólo un cambio de VARIABLES DE ENTORNO.

**7. Apoyo de la comunidad:** existe una fuerte comunidad de ingenieros de software que continuamente contribuyen con grandes imágenes que pueden ser reutilizadas para desarrollar un gran software. ¿Por qué reinventar la rueda, no?

## Despliegue con Docker

En primer lugar, si eliminastéis el repositorio en su momento, debéis volver a clonarlo en vuestra Debian, en caso contrario obviad este paso:

```sh
git clone https://github.com/contentful/the-example-app.nodejs.git
```
Ahora, puesto que la aplicación ya viene con el `Dockerfile` necesario dentro del directorio para construir la imagen y correr el contenedor, vamos a estudiar su contenido.

!!!task "Tarea"
    Completa este Dockerfile con las opciones/directivas adecuadas, leed los comentarios y podéis apoyaros [en la teoría](https://jmunozji.github.io/DAW/Unidad%207/Ud7_04_images/), en [este cheatsheet](https://devhints.io/dockerfile), en [este otro](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index) o en cualquiera que encontréis.

```yaml
_____ node #(1) 

_____ /app #(2)

_____ npm install -g contentful-cli #(3)

_____ package.json . #(4)
_____ npm install #(5)

_____ . . #(6)

_____ node #(7)
_____ 3000 #(8)

_____ ["npm", "run", "start:dev"] #(9)
```



1. Con `_____` indicamos que vamos a utilizar la imagen de Docker Hub oficial de Node. En el Dockerfile pone node:9 para usar la versión 9, pero así no funciona. Deja solo "node" para que use la versión "latest". 
2. `_____` define el directorio sobre el que se ejecutarán las subsiguientes instrucciones del `Dockerfile`   
3. `_____` ejecuta un comando en una nueva capa de la imagen (podemos tener varios comandos `_____`)
4. `_____` como su nombre indica, copia los archivos que le indiquemos dentro del contenedor, en este caso `package.json`    
    !!!info
        Recordemos que `package.json` cumplía ciertas funciones importantes:
           * Centraliza la forma de interactuar con la aplicación por medio de definición de scripts (indica comandos que podemos correr dentro de nuestro proyecto, asociándolos a una palabra clave para que npm (o yarn) los reconozca cuando queramos ejecutarlos.)
           * Gestiona de una forma clara y sencilla las dependencias necesarias para que la aplicación pueda funcionar correctamente.
5. Con otro `_____` ejecutamos el ya conocido comando que nos instala las dependencias que se indican en el archivo que hemos copiado en el paso anterior, el `package.json`
6. Copiamos todos los archivos de nuestro directorio de trabajo al contenedor
7. Con `_____` le indicaremos el usuario con el que correrá el contenedor
8. `_____` nos permite documentar que puertos están expuestos o a la escucha en el contenedor (sólo será accesible desde otros contenedores)
9.  Y finalmente `_____` nos permite ejecutar un comando **<u>dentro</u>** del contenedor. En este caso iniciamos la aplicación.

!!!note "Nota"
    En Linux, cuando queremos hacer referencia al directorio actual, lo hacemos con un punto `.` 

    Si dentro de nuestro directorio actual tenemos una carpeta llamada `prueba`, podemos hacer referencia a ella como `./prueba`, ya que el `.` hace referencia precisamente al directorio donde nos encontramos

Así pues, tener nuestra aplicación corriendo es cuestión de un par de comandos.

Hacemos un build de la imagen de Docker. Le indicamos que ésta se llama `the-example-app.nodejs` y que haga el build con el contexto del directorio actual de trabajo, así como del Dockerfile que hay en él:

```sh
docker build -t the-example-app.nodejs .
```

Y por último, iniciamos el contenedor con nuestra aplicación. Ahora sí, con la opción `-p`, le indicamos que escuche conexiones entrantes de cualquier máquina en el puerto 3000 de nuestra máquina anfitrión que haremos coincidir con el puerto 3000 del contenedor (`-p 3000:3000`). Y con la opción `-d` lo haremos correr en modo demonio, en background:

```sh
docker run -p 3000:3000 -d the-example-app.nodejs
```

Tras esto sólo queda comprobar que, efectivamente, desde nuestra máquina podemos acceder a: `http://IP_Maq_Virtual:3000` y que allí está nuestra aplicación en funcionamiento.

## Mejora la imagen

Muy probablemente en algún momento de la creación de la imagen recibas un mensaje similar a este:

    Step 3/9 : RUN npm install -g contentful-cli
    ---> Running in 8f00b40eaca1

    added 616 packages in 48s

    133 packages are looking for funding
    run `npm fund` for details
    npm notice 
    npm notice New minor version of npm available! 10.2.4 -> 10.4.0
    npm notice Changelog: <https://github.com/npm/cli/releases/tag/v10.4.0>
    npm notice Run `npm install -g npm@10.4.0` to update!
    npm notice 
    Removing intermediate container 8f00b40eaca1
    ---> b81619303488

Ya nos sucedió en la **Práctica 4 - Despliegue de aplicaciones con Node Express**, ¿recuerdas? Aunque has podido hacer funcionar la imagen pese a esta advertencia, lo cierto es que sería mejor que npm funcionara con la última versión disponible. 

Borra el contenedor creado y la imagen que habías creado. Modifica el Docker file para que la imagen se cree actualizando npm, no a la versión 10.2.4 sino a la última disponible en el momento de crear la imagen. Busca en Internet o consula a ChatGPT para obtener el comando adecuado. ¿Qué comando incluyes en el Dockerfile y en qué posición?

Vuelve a crear la imagen y el contenedor y comprueba su correcto funcionamiento.

## Referencias

[Los beneficios de utilizar Docker y contenedores a la hora de programar ](https://www.campusmvp.es/recursos/post/los-beneficios-de-utilizar-docker-y-contenedores-a-la-hora-de-programar.aspx)

[Dockerizing](https://developerexperience.io/practices/dockerizing)

[Github](https://github.com/contentful/the-example-app.nodejs)