# Notas curso Docker - Platzi - gvilarino

## Clase 1

## Problemáticas del desarrollo de software profesional

A la hora de hacer aplicaciones y proyectos de software nos podemos encontrar con varios problemas, 
estos problemas los podemos agrupar en tres categorías:

1. Construir
    - dependencias de desarrollo
    - versiones de entornos de ejecucion
    - equivalencia con entornos de desarrolo
    - equivalencia con entornos productivos
    - versiones de compatibilidad 3rd party (ej: bd mysql)

2. Distribuir
    - output de build heterogéneo
    - acceso a servidores productivos
    - ejecución nativa vs. virtualizada
    - serveless (cómo llevar el código al servidor)

3. Ejecutar.
    - dependencias de aplicación
    - compatibilidad SO
    - disponibilidad de servicios externos (ej: donde corre la BD)
    - recursos de hardware

> Docker promete ser la solución a todo nuestros problemas de una manera simple y sencilla. "Build, ship and run your code Anywhere"

----------------------------------

## Clase 2

## Qué es Docker: Containerization vs virtualization

**Virtualización**

Máquinas virtuales *"una computadora que corre dentro de otra computadora"*.

* Pesadas - En el orden de los GB - SO entero, muchas dependencias.
Muchas VMs en el mismo host suelen repetirse en lo que contienen - Mismo SO, 
mismas dependencias por cada VM (son independientes).

* Administración costosa - Una VM tiene que ser administrada como cualquier otra computadora: patching, 
updated, etc. Hay que administrar la seguridad interna entre apps.

* Lentas - Correr nuestro código en una VM implica no sólo arrancar nuestras aplicaciones, 
sino también esperar el boot de la VM en sí.

**Contenedores**

Agrupaciones de procesos

* Versátiles - En el orden de los MB. Tienen todas las dependencias que necesitan para funcionar correctamente. Funcionan igual en cualquier lado.

* Eficientes - Comparten archivos inmutables con otros contenedores. Sólo se ejecutan procesos, no un SO completo.

* Aislados - Lo que pasa en el contenedor queda en el contenedor. No pueden alterar su entorno de ejecución (a menos que explícitamente se indique lo contrario).

----------------------------------

## Clase 4

## Primeros pasos: Hola mundo y Docker Engine

`docker run hello-world`

Si es la primera vez que se intenta correr algo, el irá a descargar el contenedor, si ya tiene el contenedor, lo iniciará 

**Arquitectura de docker**

- Cliente-servidor

El servidor es un daemon que corre en la maquina.

El cliente es otro programa, cuando se ejecuta un comando es a través del cliente, éste se 
comunica con el servidor

No necesariamente el cliente está en el mismo lugar del servidor

----------------------------------

## Clase 5

## Contenedores

Un contenedor es una entidad lógica, una agrupación de procesos que se ejecutan de forma nativa como cualquier otra aplicación en la máquina host.

- Pieza fundamental de docker
- Es una agrupación de procesos
- Es una entidad lógica
- Ejecuta los procesos de forma nativa (no necesita que esté sobre un SO virtualizado)
- Los contenedores se limitan a ejecutar los procesos que están dentro del contenedor
- No tienen forma de consumir más recursos de los que están asignados en el contenedor

----------------------------------

## Clase 6

## Explorar el estado de docker

NOTA: Go es el lenguaje en el que está hecho docker

* Ver los contenedores

`docker ps`

`docker ps -a` (Muestra todos los contenedores estén iniciados o no)

`docker ps -aq` (Muestra solo los id's de los contenedores)`

> Todos los contenedores tienen un ID único

* Revisar un contenedor específico 

`docker inspect <ID_CONTENEDOR o NOMBRE_CONTENEDOR>`

- Podemos aplicar un filtro:

`docker inspect -f '{{<FILTRO>}}' <ID_CONTENEDOR o NOMBRE_CONTENEDOR>` (Linux)

`docker inspect -f "{{ json .Config.Env }}"  hello-world` (Windows)

* Renombrar contenedor

`docker rename <NOMBRE> <NUEVO_NOMBRE>`

`docker run --name <NUEVO_NOMBRE> <NOMBRE>`

> No puede haber 2 contenedores con el mismo nombre

* Ver el output de un contenedor (incluso si está apagado)

`docker logs <NOMBRE_CONTENEDOR>`

* Eliminar contenedor

`docker rm <NOMBRE>`

`docker rm $(docker ps -aq)` (Elimina TODOS los contenedores)


----------------------------------

## Clase 7

## El modo interactivo

`docker run ubuntu`

Descargará un contenedor ubuntu, pero no lo iniciará porque el contenedor no es un entorno gráfico.

`docker run -it ubuntu`

Con el `-it` le indicamos que ejecute el contenedor en un modo interactivo dentro de la terminal.

----------------------------------

## Clase 8

## Ciclo de vida de un contenedor

* Indicar al contenedor que ejecute un comando al levantar

`docker run <NOMBRE_CONTENEDOR> <COMANDO>`

Ej: `docker run ubuntu tail -f /dev/null`

Nota: se quedará esperando el contenido de `/dev/null`, pero todo lo que cae en `/dev/null` desaparece

Al hacer `docker ps` veremos que está ejecutandose la imagen ubuntu con un comando

NOTA: docker siempre le va a asignar el PID "1" al proceso que se inició con el contenedor

* Para ejecutar un proceso dentro de un contenedor que ya se está ejecutando:

`docker exec -it <NOMBRE_CONTENEDOR> <LO_QUE_QUIERO_EJECUTAR>`

Ej: `docker exec -it hungry_tereshkova bash`

* Para "matar" el contenedor (desde otra terminal)

`docker kill <NOMBRE_CONTENEDOR>`

> También se puede matar el contenedor eliminandolo

* Para eliminar el contenedor

`docker rm -f <NOMBRE_CONTENEDOR>`

NOTA: el `-f` es para forzar el borrado


----------------------------------

## Clase 9

## Exponiendo contenedores al mundo exterior

Los contenedores están aislados del sistema y a nivel de red, cada contenedor tiene su propia stack de net y sus propios puertos.

Debemos redirigir los puertos del contenedor a los de la computadora y lo podemos hacer al utilizar este comando:

`docker run -d --name server -p 8080:00 nombreDelContenedor`

* Ejecutar contenedor que tiene algun proceso con output, ignorar la salida con -d o --detach, es decir, iniciar el contenedor en segundo plano:

`docker run -d --name server nginx`

NOTA: adicionalmente se le está asignando el name "server" al contenedor con el flag "--name". En este ejemplo se descarga el servidor nginx

NOTA: Los contenedores están aislados a nivel de recursos y a nivel de red. Si un contenedor tiene un puerto abierto no quiere decir que también lo esté en la máquina.

* Para "redirigir" el puerto de la máquina al del contenedor:

`docker run -d --name server -p 8080:80 nginx`

*puerto de la maquina (8080)*      :  puerto del contenedor (80)

NOTA: Si al acceder a localhost:8080 no abre el servidor, debemos ver cuál es la ip del contenedor a donde está levantado el servidor con: `docker-machine ls`

----------------------------------

## Clase 10

## Datos en Docker

`docker run -d --name db mongo`

Iniciamos bash en mongo:

`docker exec -it db bash`

Si por ejemplo creamos una colección "usuarios" e insertamos un usuario:

```bash
> use platzi
> db.users.insert({"name":"test"})
> db.users.find()
> exit
```

Luego salimos del contenedor: `exit`

Luego matamos al contenedor: `docker rm -f dn`

Luego iniciamos uno nuevo: `docker run -d --name db mongo`

y consultamos nuestra colección de usuarios:

```
> use platzi
switched to db platzi
> db.users.find()
```

Los datos se perdieron

¿Cómo hacemos que los datos sobrevivan a la vida del contenedor?
1. Armar un lugar donde el contenedor pueda guardar sus datos `mkdir mongodata`

Este archivo lo vamos a montar como una especie de "volumen" al contenedor

2. Asignar ese lugar al contenedor

Con el flag `-v` seguido de la ruta, con la siguiente estructura:

 Lugar en la maquina                      :  lugar en el contenedor 

/c/Program Files/Docker Toolbox/mongodata : /data/db

`docker run --name db -d -v "/c/Program Files/Docker Toolbox/mongodata:/data/db" mongo`

NOTA: se coloca entre "" por estar en windows

3. si revisamos el contenedor:

`docker inspect db`

veremos en "Mounts" el volumen configurado

```
"Mounts":...
            {
                "Type": "bind",
                "Source": "/c/Program Files/Docker Toolbox/mongodata",
                "Destination": "/data/db",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
....
```

> La desventaja de esto es que una parte de mi sistema está expuesto al contenedor


----------------------------------

## Clase 11

## Datos con Docker: Volumes

A pesar de que no es lo más divertido que tiene Docker, esta herramienta nos permite recuperar datos que podíamos dar por perdido.

Existen tres maneras de hacer permanencia de datos:

1. FileSystem
2. Bind mount (tiene la desventaja de que da acceso a los datos de la maquina host)
3. Volume (docker sugiere usar este) 

Los volumes:

- Funciona como bind mount pero el lugar donde van a estar los datos es un lugar manejado por docker

- Se le puede especificar un driver (por ejemplo para el almacenamiento en la nube)

`docker volume ls` ---> Para ver los volumenes existentes

`docker volume prune` ---> Borra los volumenes que no estén en uso por un contenedor

`docker volume create <NOMBRE_VOLUMEN>` ---> Crea un volumen

`docker run -d --name <NOMBRE_CONTENEDOR> --mount src=<ORIGEN>,dst=<DESTINO> <CONTENEDOR_A_EJECUTAR>`

ORIGEN : volumen origen (nombre)

DESTINO : ruta destino dentro del contenedor

Ejemplo: 
`docker run -d --name db --mount src=dbdata,dst=/data/db mongo`

----------------------------------

## Clase 12

## Conceptos fundamentales de Docker: imágenes

Las imágenes son un componente fundamental de Docker y sin ellas los contenedores no tendrían sentido. 

Estas imágenes son fundamentalmente plantillas o templates.

Algo que debemos tener en cuenta es que las imágenes no van a cambiar, es decir, una vez este realizada no la podremos cambiar.

- A partir de las imágenes generamos los contenedores
- Se pueden distribuir de manera simple

`docker pull <IMAGEN>` Descarga la imagen pero no la iniciados

- Una imagen no es un "archivo", es un conjunto de **capas**

`docker image ls` Lista todas las imagenes que tenemos, junto con su versión (tag) y tamaño

IMAGEN = repository

- Las imágenes vienen desde: `hub.docker.com`

`docker pull <IMAGEN>:<VERSION>`

Ejemplo: 

`docker pull ubuntu:18.04`

descarga la versión especificada de la imagen

> Si tenemos una imagen de ubuntu, y descargamos otra versión, docker solo descargará las capas que no tengan en común.


----------------------------------

## Clase 13

## Construyendo nuestras propias imágenes

Vamos a crear nuestras propias imágenes, necesitamos un archivo llamado DockerFile que es la “receta” que utiliza Docker para crear imágenes.

>Es importante que el DockerFile siempre empiece con un “FROM” sino no va a funcionar.

El flujo para construir en Docker siempre es así:

`Dockerfile – **build **–> Imagen – run --> Contenedor`

* Para crear un dockerfile:

1. Creamos un archivo llamado "Dockerfile"
2. Configuramos nuestra imagen en el archivo, comenzando por la palabra

`FROM <ULTIMO_LAYER_IMAGEN_BASE>`

3. Hacemos build de la imagen

`docker build -t <NOMBRE_IMAGEN>:<VERSION> <PATH>`

`-t` es para especificar el nombre de la imagen

PATH : contexto del build. Docker puede utilizar todo lo que está en el contexto o path

`docker build -t  ubuntu:platzi "C:\Users\Usuario\Desktop\Cursos Platzi\Cursos Docker"`


`docker run -it ubuntu:platzi` 
Se ejecuta la nueva imagen.

`docker push ubuntu:platzi` Publicar la imagen. Pero dará error porque se está tratando de publicar al repositorio oficial de docker. Para poder subirla tiene que ser en el repositorio personal y para esto cambiamos el tag de la imagen

`docker tag <NOMBRE_IMAGEN>:<VERSION> <USUARIO_DOCKER_HUB>/<NOMBRE_IMAGEN>:<VERSION>` Creamos una nueva imagen con el tag especificado

`docker tag ubuntu:platzi user/ubuntu:platzi`

`docker push user/ubuntu:platzi`

----------------------------------

## Clase 14

## Comprendiendo el sistema de capas

- En dockerhub podemos ver los dockerfiles de las distintas imagenes
- FROM scratch - Parte de 0 para hacer la imagen (kernel de linux)

`docker history <IMAGEN>` Vemos todo el historial de las capas de la imagen. El historial de la capas sale resumido

`docker history --no-trunc <IMAGEN>` Para que no trunque el output

- Una herramienta util para revisar las imágenes y los layers: [dive](https://github.com/wagoodman/dive)

- Los layers son inmutables

----------------------------------

## Clase 15

## Usando docker para desarrollar aplicaciones

Archivo Dockerfile:

* COPY : copia lo que está en el contexto de build a la ubicación que se le especifique en la imagen

`COPY [".", "/usr/src/"]`

Cuando se pone "." es copiar todo

* WORKDIR : equivale a "cd"

* EXPOSE : exponer en el puerto especificado 

* CMD : command, se especifica el comando que ejecuta por defecto el  contenedor al levantar

`cd "C:\Users\Usuario\Desktop\Cursos Platzi\Cursos Docker\docker"`

* "Compilar" la imagen: `docker build -t platziapp .`

* Una vez compilada podremos verla con: `docker image ls`

* Iniciar el contenedor: `docker run --rm -p 3000:3000 platziapp`

`--rm:` cuando termine de correr el contenedor se borrará

----------------------------------

## Clase 16

## Entendiendo el cache de layers para estructurar correctamente tus imágenes

* Actualizar el contenido del contenedor sin hacer build (NodeJS)

Herramienta nodemon (para nodejs)

`CMD ["npx", "nodemon", "index.js"]`

* Levantar la imagen para que los cambios realizados en el index.js se vean reflejados automáticamente:

Modificar el CMD por:

`CMD ["npx", "nodemon", "--legacy-watch", "index.js"]`

`docker run --rm -p 3000:3000 -v "/c/Users/Usuario/Desktop/Cursos Platzi/Cursos Docker/docker":/usr/src platziapp`

NOTAS:
	- Se coloca la primera ruta entre "" por ser Windows
	
	- Si al hacer run da el error: 
	
	```internal/modules/cjs/loader.js:638
    throw err;
    ^
	```
	
Hay que instalar las dependencias localmente con `npm install` ya que al hacer `-v` se estan sobreescribiendo las dependencias de la carpeta `/user/src`.

De esta manera cuando se monte el contenido en dicha carpeta, se llevará las dependencias instaladas.

[Repositorio con ejemplo del curso](https://github.com/gvilarino/docker-testing)

----------------------------------

## Clase 17

## Docker networking: colaboración entre contenedores

Conectando el contenedor actual con uno aparte que contiene MongoDB

- Docker nos permite utilizar redes virtuales para conectar contenedores

* Para ver las redes que tiene docker `docker network ls`

Tiene 3 redes por defecto:

- bridge: red por defecto, o red puente, se solia usar con la instrucción link, la cual permitia enlazar contenedores a traves de la red. Sin embargo esta red esta en desuso.
- host: ADVERTENCIA el uso de esta interfaz es de cuidado. Permite que el contenedor use la red por defecto de la maquina host. Es sensible a que si el contendor abre algun puerto, esto se ve replicado en la maquina host, generando asi posibles vulnerabilidades.
- none: es similar al dev/null o hoyo negro de los sistemas unix. En este caso nos permite especificar que el contenedor no tiene salida o permiso para acceder o ser accedido por red.

**Crear una nueva red:**

1. Crear la red

`docker network create`

`docker network create --attachable <NOMBRE_RED>`

`--attachable`: indica que se va a permitir que otros contenedores usen la red

2. Verificar que esté creada con: `docker network ls
3. Levantar el contenedor de mongo `docker run -d --name db mongo`
4. Conectar el contenedor de Mongo con l red: `docker network connect platzinet db`
5. Podemos ver cuáles contenedores están conectados a una red con: `docker network inspect <NOMBRE_RED>` 

```
"Containers": {
"ece9f211b6b6c2ee6dec25a8d4c8d0b65b5d5e5b35b80bf2788f3811d0f94595": {
	"Name": "db",
	"EndpointID": "b39950d6f778e98700297de1d43af097915ee6350ecce530f05fbe886735c2cd",
	"MacAddress": "02:42:ac:12:00:02",
	"IPv4Address": "172.18.0.2/16",
	"IPv6Address": ""
}
},
```

Notese que la red tiene su propia IP

6. Levantar el otro contenedor que queremos que se comunique con el de mongo a través de la red

`docker run -d --name app -p 3000:3000 --env MONGO_URL=mongodb://db:27017/test platziapp`

`--env` : se indica variables de entorno para el contenedor

NOTA: Si 2 contenedores están conectados a la misma red, se pueden ver entre si utilizando como hostmane el nombre del contenedor , por eso se usa el mongodb://db:PUERTO/NOMBRE_BD

7. Conectar a la red `docker network connect platzinet app`


----------------------------------

## Clase 18

## Docker-compose: la herramienta todo-en-uno

Eliminar la red: `docker network rm platzinet`

Docker compose: es una herramienta que nos permite describir de forma declarativa la arquitectura de nuestra aplicación.

Utiliza un compose file, en formato yml  (docker-compose.yml)

Un servicio puede tener más de un contenedor

* Iniciar todos los servicios declarados en el docker-compose:

`docker-compose up`

----------------------------------

## Clase 19

## Trabajando con docker-compose

`docker-compose up -d`

* Ver el estado `docker-compose ps`

- Normalmente docker asignará como nombre de los contenedores el nombre del directorio + el nombre del servicio + un correlativo

Ej: docker_app_1

- docker-compose siempre asignará un network para los contenedores que estén en el archivo

* Ver el log de un contenedor `docker-compose logs <NOMBRE_SERVICIO>`

* Ejecutar algo en el contenedor `docker-compose exec <NOMBRE_SERVICIO> bash`

* Detener `docker-compose down`

----------------------------------

## Clase 20

## Docker-compose como herramienta de desarrollo

* Para hacer un build de la imagen desde 0 colocamos en el servicio en el archivo docker-compose "build":

```
services:
  app:
    build: .  <<<---- Buscará en esta ruta un Dockerfile
```
	
* Hacemos build con: `docker-compose build`

* Vigilar los logs `docker-compose logs -f <NOMBRE_SERVICIO>`

* Configurar el volumen: Agregar al docker-compose la configuración del volumen

```
services:
  app:
    build: .
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
    volumes:
        - .:/usr/src	<<- Copia lo de la ruta actual a usr..
        - /usr/src/node_modules  <<- No reemplaces lo que esta en esta ruta
```
		
Al hacer `docker-compose up -d` automaticamente hace build

* Para hacer clusters de un servicio, o levantar varios servidores

Se puede simular con

`docker-compose scale <NOMBRE_SERVICIO>=<NUMERO_CONTENEDORES>`

- En este caso va a dar error porque todos los contenedores intentaran levantar sobre el mismo puerto, para solucionar esto hay que configurar en el docker-compose file el rango de puertos del servicio:
```    
	ports:
      - "3000-3010:3000"
```

Luego: 

`docker-compose up -d`

`docker-compose scale app=4`

Cuando hacemos: `docker-compose logs -f app`

Mostrará los logs de todos los contenedores levantados

----------------------------------

## Clase 21

## Conceptos para imágenes productivas

Problemas:

1. No quiero copiar todo lo de mi espacio de trabajo a la imagen que va a produccion: 

Para excluir partes del contexto de build:

Archivo .dockerignore: parecido al gitignore, de esta manera al hacer build se excluyen esos archivos

2. Multistage builds: Usar dockerfiles que tienen varias fases de build, y una fase puede usar la salida de otra.

Por ejemplo, para hacer build de un entorno de desarrollo se especifica el docker file de desarrollo:

`docker build -t platziapp -f build/development.Dockerfile .`

* Ejemplo de multi stage build, ver build/production.Dockerfile

`docker build -t platziapp -f build/production.Dockerfile .`

Vemos que ese dockerfile está dividido en dos parte, la primera se llama "builder", y monta un ambiente de desarrollo.
La segunda depende de la primera, y todos los archivos y rutas a los que hace referencia provienen del resultado de builder

----------------------------------

## Clase 22

## Conceptos para imágenes productivas

Manejando docker desde un contenedor

Docker in docker:
- Manejar docker desde contenedores
- Tenemos un contenedor que tiene el cliente de docker y éste se comunica al daemon de docker de la máquina host, pero sin tocar el resto de la máquina

* Para esto descargar la imagen "docker"

`docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock docker:18.06.1-ce`

OJO: verificar la ultima version disponible
`-v /var/run/docker.sock:/var/run/docker.sock :` Con esto estamos montando el socker de docker al contenedor donde está escuchado el docker daemon. Se monta dentro del cliente

- La conexion entre el cliente de docker y el daemon ocurre con un socket de linux

* Despues de que se incie el contenedor, podemos hacer docker ps -a y ver las imagenes/contenedores que ya tenemos en la maquina local

* Con esto montamos ademas el binario del cliente de docker

`docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $(witch docker):/bin/docker wagoodman/dive:latest platziapp`

----------------------------------

## Comandos

| Descripción                              | Comandos                                                                                                                             |
|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar contenedor                       | docker start <nombre>                                                                                                                |
| Ver imágenes creadas                     | docker images                                                                                                                        |
| Ver contenedores creados,  con su estado | docker ps -a                                                                                                                         |
| Login en docker hub                      | docker login                                                                                                                         |
| Subir imagen a docker hub                | docker push <nombre imagen>                                                                                                          |
| Iniciar contenedor imagen                | docker run -d --name <nombre conteneder> --add-host=<nombre_host_parametro_yaml>:<ip> -p <puerto>:<puerto> <nombre imagen>:<version> |
| Ver logs de un contenedor                | docker logs -f <nombre>                                                                                                              |
| Detener contenedor                       | docker stop <nombre contenedor>                                                                                                      |
| Reiniciar contenedor                     | docker restart <nombre contenedor>                                                                                                   |
| Eliminar imagen                          | docker image rm <nombre de la imagen>                                                                                                |
| Eliminar contenedor                      | docker ps -a  --- Copiar ID del container docker container rm <ID>                                                                   |