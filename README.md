# Fundamentos y usos prácticos de Docker

[![Docker](https://badgen.net/badge/icon/docker?icon=docker&label)](https://docker.com/)
[![Supported by](https://img.shields.io/badge/Supported%20by-CFL410-green.svg)](https://centro410laplata.edu.ar/)
[![Supported by](https://img.shields.io/badge/Supported%20by-IDEP-green.svg)](https://idepba.com.ar/)
[![Powered](https://img.shields.io/badge/Powered%20by-ATE-green.svg)](https://atepba.org.ar/)
![Version](https://img.shields.io/badge/Version-1.4-orange)

## Trabajo integrador 🐳


En el presente trabajo integrador se evaluará:

- Conteinerizar una aplicación simple.
- Buildear y correr una imágen como un contenedor.
- Compartir imágenes en Docker Hub.
- Deployar aplicaciones Docker usando multiples contenedores usando una base de datos que corre en un contenedor aparte.
- Correr la aplicación usando docker compose.

> [!IMPORTANT]  
> Fecha límite de entrega 10/11/2025.

## Prerequisitos

- Tener instalado Docker Desktop o Docker CLI
- Tener instalado un editor de texto, como <a href="https://notepad-plus-plus.org/downloads/" target="_blank">Notepad++</a>, <a href="https://code.visualstudio.com/Download" target="_blank">VSCode</a> o <a href="https://vscodium.com/" target="_blank">VSCodium</a>.
- Tener instalado un cliente Git (opcional).


### Forma de entrega

Se proponen dos formas de entrega:

1) [Forkear](https://github.com/kity-linuxero/docker-integradora/fork) el presente repositorio y completar este archivo `README.md` donde lo pida, incluyendo capturas de pantalla. Luego enviar la URL del repo por mail. (Recomendado si está familiarizado con Git, Github y Markdown).

2) Completar cada uno de los puntos solicitados en el presente documento en este <a href="./DOCKER_TPFINAL_2025.txt" download> archivo `.txt` </a> y enviarlo por e-mail a cgiambruni@gmail.com en un archivo `zip` con las capturas de pantalla solicitadas y el archivo `compose.yml` por e-mail. El archivo zip debe llamarse `APELLIDO.NOMBRE.zip`.



## Parte 1 - Conteinerizar una Aplicación

Para este trabajo integrador, usaremos una app simple **todo list manager** que corre en Node.js y podemos levantarla en nuestro navegador web. Si no estás familiarizado con Node.js, no te preocupes, este trabajo integrador no requiere conocimientos de programación. Solo usaremos una app de ejemplo para poder armar las imágenes y correr los contenedores.

![](./imgs/screenshot.png)

### 1. Obtener la aplicación

Antes de poder correr la aplicación, necesitamos obtener el código fuente y descargarlo.

- Clonar el repositorio usando el siguiente comando:

    ```bash
    git clone https://github.com/kity-linuxero/docker-integradora.git
    ```
- Si no tiene un cliente git instalado, puede descargar el repositorio del siguiente [link](https://codeload.github.com/kity-linuxero/docker-integradora/zip/refs/heads/main). Luego debe descomprimir el archivo zip.

- Una vez descargada la aplicación, deberías ver el código fuente de la misma con la siguiente estructura de directorios dentro de la carpeta `app`:

    ```
        app/
        ├─ spec/
        ├─ src/
        ├─ yarn.lock
        ├─ package.json
        ├─ Dockerfile
        ├─ .dockerignore
    ```


### 2. Buildear imágen

> [!TIP]
> Consulte apuntes de <a href="https://docker.idepba.com.ar/clase3.html#/docker_build" target="_blank">docker build</a>.

- Para buildear la imágen usaremos el archivo `Dockerfile` que está en el repo. Observe y analice el archivo `Dockerfile`.



    ```dockerfile
    # Usamos la imagen base de Alpine Linux
    FROM alpine:latest

    # Actualizamos los paquetes e instalamos Node.js y Yarn directamente desde los repositorios oficiales
    RUN apk add --no-cache nodejs yarn

    # Establecemos el directorio de trabajo
    WORKDIR /app

    # Copiamos los archivos del proyecto al contenedor
    COPY . .

    # Instalamos las dependencias del proyecto
    RUN yarn install --production

    # Exponemos el puerto de la aplicación (ejemplo: 3000)
    EXPOSE 3000

    # Comando por defecto para ejecutar la aplicación
    CMD ["node", "src/index.js"]
    ```

**ENTREGABLE:**

- **1.1)** Ejecute el comando correspondiente para buildear la imagen. Elija un nombre de imagen y un tag acorde. 
    ```bash
    # Escriba acá el comando utilizado
    ```
- **1.2)** ¿Qué espacio ocupa la imagen una vez creada?
    ```bash
    # Espacio utilizado
    ```
- **1.3)** ¿Puede hacer algo para optimizar o mejorar la imagen?. Describa qué modificaciones puede hacer para optimizar la imagen.
    ```bash
    # Describa que podría hacer para mejorar u optimizar la creación de la imágen.
    ```


> [!TIP]
> Consulte apuntes sobre <a href="https://docker.idepba.com.ar/clase2.html#/images_tags" target="_blank">tags</a>.

### 3. Correr la aplicación

Una vez creada la imágen, debería ser capaz de correr la aplicación.

**ENTREGABLE:**

- **1.4)** Ejecute un comando para poder correr la aplicación.
    ```bash
    # Escriba acá el comando
    ```
- **1.5)** Explique el comando de la respuesta anterior y cada parámetro enviado.
    ```bash
    # Escriba la explicación
    ```
- **1.6)** ¿Cómo puede saber si el contenedor está corriendo?
    ```bash
    # Escriba acá el comando
    ```
- **1.7)** Adjunte una captura de pantalla con la aplicación funcionando con la URL utilizada para acceder. Reemplace la imágen siguiente por su captura de pantalla.
    ![](./imgs/broken_img.png)


## Parte 2 - Actualizar aplicación

En esta parte 2, haremos algunos cambios y actualizaremos la aplicación.

### 1. Actualizar el código fuente

- En el archivo `app/src/static/js/app.js` actualizaremos la **línea 56**, con los siguientes cambios: 

   ```diff
   - <p className="text-center">Aún no hay items. ¡Agrega tu primer item arriba!</p>
   + <p className="text-center">No hay nada en la lista! | by: [SU APELLIDO.NOMBRE]</p>
   ```

**ENTREGABLE**

- **2.1)** Modifique el código fuente como se indicó anteriormente.
- **2.2)** Ejecute los comando necesarios para que la aplicación tome los cambios. Realice un etiquetado (tag) coherente respecto a los cambios en la imágen.


### 2. Elimine el contenedor e imágen anterior

La actualización del código recientemente realizada deja obsoleta la antigua versión.

**ENTREGABLE**

- **2.2)** Elimine la imágen y el contenedor hecho en el punto anterior: Mostrar comandos utilizados.
    ```bash
    # Escriba acá el comando utilizado
    ```
- **2.3)** ¿Como puede listar las imágenes para comprobar que se ha eliminado la imagen del punto anterior?
    ```bash
    # Escriba acá el comando utilizado
    ```


## Parte 3 - Compartir app

Para compartir la imágen de la aplicación usaremos la registry de [DockerHub](https://hub.docker.com/).

> [!TIP]
> De ser necesario, repase lo realizado en el [Laboratorio 2.4](https://github.com/kity-linuxero/docker_410_practicas/blob/v1.4/labs/02-conceptos-basicos/24-images-push.md).


**ENTREGABLE**

- **3.1)** Comparta la URL de DockerHub para que pueda ser posible probar y descargar su imágen.

    [Inserte la URL de la imágen](https://dockerhub.com/)

> [!IMPORTANT]
> Agregue un _overview_ para el repositorio de Dockerhub con instrucciones para correr la imágen y todo lo que considere necesario para que un tercero pueda ejecutar la imágen.

> [!TIP]
> Utilice el formato [markdown](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) para darle formato al overview.


## Parte 4 - Persistencia de datos

La aplicación, hasta el momento carece de persistencia de datos, y si el contenedor se elimina los datos `to-dos` se pierden.

Los datos en esta APP se guardan en un archivo `/etc/todos/todo.db`.

**ENTREGABLE**

- **4.1)** Escriba los comandos necesarios para persistir la base de datos. Decida que tipo de persistencia es la adecuada para su app ([Bind mounts](https://docker.idepba.com.ar/clase5.html#/bind_mounts) o [volumes](https://docker.idepba.com.ar/clase5.html#/volumes))
    ```bash
    # Escriba el comando necesario
    ```


> [!TIP]
> Repase [volúmenes y persistencia](https://docker.idepba.com.ar/clase5.html#/volumenes) de datos.


## Parte 5 - Aplicaciones multicontainer


Hasta este punto, hemos deployado nuestra aplicación que corre en un único contenedor. A continuación agregaremos un segundo contenedor para que sea de base de datos basada en `MySQL`.

![](./imgs/multi-container.webp)

#### Redes en contenedores

Recordemos que de manera predeterminada, los contenedores se ejecutan de forma aislada y "no ven" otros procesos o contenedores en la misma máquina. Por lo tanto debemos crear una red para que se comuniquen entre ellos.


#### Base de datos

Usaremos una imágen basada en MySQL. La imágen en cuestión será `mysql:8.0`. Para poder iniciar y tener cierta configuración sobre la base de datos, usaremos variables de entorno. Para mas info consulte la sección _variables de entorno_ de [Docker Hub MySQL](https://hub.docker.com/_/mysql/).

A modo de resumen, usaremos las siguientes para el contenedor de base de datos:

- `MYSQL_ROOT_PASSWORD`: La password del usuario root de la base de datos. Utilice la password de su preferencia.
- `MYSQL_DATABASE`: La base de datos que utilizaremos. Elija un nombre de su preferencia, por ejemplo `todos`.

#### Conectar APP a base de datos

En la aplicación también es posible setear variables de entorno para parametrizar su funcionamiento. Utilizaremos las siguientes para especificar lo necesario para la conexión con la base de datos:

- `MYSQL_HOST`: Hostname donde corre el servidor MySQL.
- `MYSQL_USER`: El usuario para la conexión.
- `MYSQL_PASSWORD`: La password utilizada para la conexión.
- `MYSQL_DB`: La base de datos que se utilizará una vez conectada la aplicación.

>Consulte [src/persistence/mysql.js](https://github.com/kity-linuxero/docker-integradora/blob/main/app/src/persistence/mysql.js) para mas información.

**ENTREGABLE:**

- **5.1)** [Crear una red](https://docker.idepba.com.ar/clase4.html#/network_create) para conexión entre los contenedores que servirá también para conectar a la aplicación.
    ```bash
    # Escriba acá el comando utilizado
    ```
- **5.2)** [Crear un nuevo volumen](https://docker.idepba.com.ar/clase5.html#/volume_create) para persistir los datos de la base MySQL. El path donde se almacenan los datos en el contenedor MySQL es `/var/lib/mysql`.
    ```bash
    # Escriba acá el comando utilizado
    ```
- **5.3)** Iniciar el _contenedor de base de datos_ utilizando el comando `docker run` y enviando las variables de entorno necesarias.
    ```bash
    # Escriba acá el comando utilizado
    ```
- **5.4)** Iniciar el _contenedor de la aplicación_ utilizando el comando `docker run` enviando las variables de entornos necesarias para la conexión con la base de datos.
    ```bash
    # Escriba acá el comando utilizado
    ```

> [!TIP]
> Set environments variables (-e, --env) [Docker Docs](https://docs.docker.com/reference/cli/docker/container/run/#env).




Si todo sale bien, el log de la app debería mostrar lo siguiente:

```
Waiting for db:3306.
Connected!
Connected to mysql db at host db
Listening on port 3000
```

> [!IMPORTANT]
> Conectar ambos contenedores a la misma red. Utilice el parámetro `--name` o `--network-alias` para poder identificar el servidor de base de datos, de manera que el servidor de la app pueda establecer la conexión. La base de datos debe estar previamente iniciada.

> [!IMPORTANT]
> ¿Algo no funciona? Pruebe realizar el troubleshooting de [este documento](./troubleshooting.md)

## Parte 6 - Utilizando Docker Compose

Llegando a este punto y habiendo completado cada punto ya tiene la información necesaria para volcarla en un archivo de Docker Compose para simplificar la ejecución de los contenedores.

#### Cree el archivo de Docker Compose

> [!TIP]
> Puede ser de utilidad el sitio [composerize](https://www.composerize.com/) o con una herramienta de IA de su preferencia.

> [!NOTE]  
> Teniendo en cuenta que la aplicación necesitará que la base de datos esté previamente iniciada, utilice los elementos de compose para explicitar dicha dependencia.

#### Corra los contenedores

Con el siguiente comando debería ser capaz de correr la aplicación junto con la base de datos

```
docker compose up -d
```

#### Imagen de docker hub

Cambie la imágen del `docker compose` para que tome como origen la imágen que ha subido a Docker Hub con su usuario.

**ENTREGABLE**
- **6.1)** Suba el archivo docker compose en este repositorio en la carpeta raíz o adjunte en el `.zip`. El compose debe realizar todo lo necesario para que la aplicación levante con solo ejecutar `docker compose up`. Para probar si realmente funciona correctamente, puede probar su compose en [Play With Docker](https://labs.play-with-docker.com)

--------------


## Referencias:

- [Docker Docs: Docker Workshop](https://docs.docker.com/get-started/workshop/)

----



<p align="center">
  <img src="./imgs/logos.footer.gray.webp">
</p>




