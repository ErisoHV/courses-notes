# Curso Git Platzi

## Clase 1

GIT:
- creado por Linus Torvalds

- `git init`: inicia un repositorio en una carpeta
- `git add biografia.txt`: la base de datos de control de versiones ahora sabe del archivo. Ahora está en staging
- `git commit`: añade los cambios a la BD de control de versiones

-----------------------------------------------------

## Clase 2

### Diferencias entre la estructura de archivos de Windows, Mac o Linux.

La ruta principal en Windows es C:\, en UNIX es solo /.

Windows no hace diferencia entre mayúsculas y minúsculas pero UNIX sí.

Recuerda que GitBash usa la ruta /c para dirigirse a C:\ (o /d para dirigirse a D:\) en Windows. Por lo tanto, la ruta del usuario con el que estás trabajando es /c/Users/Nombre de tu usuario

**Comandos básicos en la terminal:**

`pwd`: Nos muestra la ruta de carpetas en la que te encuentras ahora mismo.
	
`mkdir`: Nos permite crear carpetas (por ejemplo, mkdir Carpeta-Importante).
	
`touch`: Nos permite crear archivos (por ejemplo, touch archivo.txt).
	
`rm`: Nos permite borrar un archivo o carpeta (por ejemplo, rm archivo.txt). Mucho cuidado con este comando, puedes borrar todo tu disco duro.
    
`cat`: Ver el contenido de un archivo (por ejemplo, cat nombre-archivo.txt).
	
`ls`: Nos permite cambiar ver los archivos de la carpeta donde estamos ahora mismo. Podemos usar uno o más argumentos para ver más información sobre estos archivos (los argumentos pueden ser -- + el nombre del argumento o - + una sola letra o shortcut por cada argumento).
- `ls -a`: Mostrar todos los archivos, incluso los ocultos.
- `ls -l`: Ver todos los archivos como una lista.

`cd`: Nos permite navegar entre carpetas.
- `cd /`: Ir a la ruta principal:
- `cd o cd ~`: Ir a la ruta de tu usuario
- `cd carpeta/subcarpeta`: Navegar a una ruta dentro de la carpeta donde estamos ahora mismo.
- `cd .. (cd + dos puntos)`: Regresar una carpeta hacia atrás.
- Si quieres referirte al directorio en el que te encuentras ahora mismo puedes usar `cd . (cd + un punto)`.

`history`: Ver los últimos comandos que ejecutamos y un número especial con el que podemos repetir su ejecución.

`! + número`: Ejecutar algún comando con el número que nos muestra el comando history (por ejemplo, !72).

`clear`: Para limpiar la terminal. También podemos usar los atajos de teclado `Ctrl + L` o `Command + L`.

Todos estos comandos tiene una función de autocompletado, o sea, puedes escribir la primera parte y presionar la tecla Tab para que la terminal nos muestre todas las posibles carpetas o comandos que podemos ejecutar. Si presionas la tecla Arriba puedes ver el último comando que ejecutamos.

Recuerda que podemos descubrir todos los argumentos de un comando con el argumento `--help` (por ejemplo, `cat --help`).


----------------------------------------------------

## Clase 3

`git init`:

1. Crea un área en memoria RAM que se llama staging
2. Crea  el repositorio .git/ (con nombre por defecto "master")
3. El archivo no está rastreado (untracked)

`git add`: El archivo pasa a "vivir" en staging y queda en espera de formar parte del repositorio (tracked)

`git commit`: El archivo va a al repositorio


----------------------------------------------------

## Clase 4-5

Master: Rama por defecto

`git rm <archivo>`: lo deja en cache pendiente de commit

`git rm --cache <archivo>`: lo deja en cache pendiente de add

`git config`:

- `git config --list`: lista todas las configuraciones por defecto y las cosas que faltan
- `git config --list --show-origin`: muestra la ubicacion de las configuraciones
- `git config --global`: configuraciones globales

`git config --global user.name "<nombre>"`

`git config --global user.email "<correo>"`

----------------------------------------------------

## Clase 6-7

`git show`: muestra los cambios detallados hechos en un archivo

`git diff <SHA_1> <SHA_2>`: compara 2 commits, en el orden en el que lo pongamos

**Volver el tiempo:**

`git log`

>Copiamos el SHA del commit

`git reset <SHA> --hard`: Todo vuelve al estado anterior
`git reset <SHA> --soft`: Vuelve al estado anterior, pero lo que está en staging se mantiene (git add)

`git log --stat`: Permite ver los cambios con más detalle por archivo que se han hecho por archivo y commit

`git checkout <SHA> <archivo>`: Devuelve el archivo al commit especificado

`git checkout <rama> <archivo>`: Coloca el archivo en el HEAD de la rama

>`Git reset` y `git rm` son comandos con utilidades muy diferentes, pero aún así se confunden muy fácilmente.

#### git rm

`git rm`: Este comando nos ayuda a **eliminar archivos de Git sin eliminar su historial del sistema de versiones**. 
Esto quiere decir que si necesitamos recuperar el archivo solo debemos “viajar en el tiempo” y recuperar el último commit antes de borrar el archivo en cuestión.

Recuerda que `git rm` no puede usarse así nomás. Debemos usar uno de los flags para indicarle a Git cómo eliminar los archivos que ya no necesitamos en la última versión del proyecto:

`git rm --cached`: Elimina los archivos del área de Staging y del próximo commit pero los mantiene en nuestro disco duro.

`git rm --force`: Elimina los archivos de Git y del disco duro. Git siempre guarda todo, por lo que podemos acceder al registro de la existencia de los archivos, de modo que podremos recuperarlos si es necesario (pero debemos usar comandos más avanzados).

#### git reset

Este comando nos ayuda a volver en el tiempo. Pero no como git checkout que nos deja ir, mirar, pasear y volver. 

Con `git reset` volvemos al pasado sin la posibilidad de volver al futuro. Borramos la historia y la debemos sobreescribir. No hay vuelta atrás.

Este comando es muy peligroso y debemos usarlo solo en caso de emergencia. Recuerda que debemos usar alguna de estas dos opciones:

Hay dos formas de usar git reset: con el argumento --hard, borrando toda la información que tengamos en el área de staging (y perdiendo todo para siempre). O, un poco más seguro, con el argumento --soft, que mantiene allí los archivos del área de staging 
para que podamos aplicar nuestros últimos cambios pero desde un commit anterior.

`git reset --soft`: Borramos todo el historial y los registros de Git pero guardamos los cambios que tengamos en Staging, así podemos aplicar las últimas actualizaciones a un nuevo commit.

`git reset --hard`: Borra todo. Todo todito, absolutamente todo. Toda la información de los commits y del área de staging se borra del historial.

**¡Pero todavía falta algo!**

`git reset HEAD`: Este es el comando para sacar archivos del área de Staging. No para borrarlos ni nada de eso, solo para que los últimos cambios de estos archivos no se envíen al último commit, a menos que cambiemos de opinión y los incluyamos de nuevo en staging con git add, por supuesto.


-----------------------------------------------------

## Clase 8-9

> `git pull` = `git fetch` + `git merge`

`HEAD` = commit más reciente de la rama

`git commit -am "<MENSAJE>"` = Hace add y commit de archivos ya existentes en nuestro repositorio

Git commit con editor vim:

"i" o ins para editar

se escribe el commit

Esc + ":" + "wq"



--------------------------------------------------

## Clase 10

Agregar origen remoto:

`git remote add origin <URL_REPO>`

`git remote -v`: Muestra los origins

Forzar: Fusionar lo remoto con lo local

`git pull origin master --allow-unrelated-histories`

----------------------------------------------------

## Clase 11

LLaves públicas y llaves privadas. Están vinculadas matemáticamente

Lo que yo cifre con la llave publica solo se puede descifrar con la privada

Las llaves públicas y privadas nos ayudan a cifrar y descifrar nuestros archivos de forma que los podamos compartir archivos sin correr el riesgo de que sean interceptados por personas con malas intenciones.

La forma de hacerlo es la siguiente:
- Ambas personas deben crear su llave pública y privada.
- Ambas personas pueden compartir su llave pública a las otras partes (recuerda que esta llave es pública, no hay problema si la “interceptan”).
    
La persona que quiere compartir un mensaje puede usar la llave pública de la 
otra persona para cifrar los archivos y asegurarse que solo puedan ser descifrados con la llave privada de la persona con la que queremos compartir el mensaje.

El mensaje está cifrado y puede ser enviado a la otra persona sin problemas en caso de que los archivos sean interceptados.

La persona a la que enviamos el mensaje cifrado puede usar su llave privada para descifrar el mensaje y ver los archivos.

> Puedes compartir tu llave pública pero nunca tu llave privada.

En github:

- Primer paso: Generar tus llaves SSH. Recuerda que es muy buena idea proteger tu llave privada con una contraseña.

`ssh-keygen -t rsa -b 4096 -C "email@email.com"`

- Segundo paso: Terminar de configurar nuestro sistema.

En Windows y Linux:

**Encender el "servidor" de llaves SSH de tu computadora:**

`eval $(ssh-agent -s)`

**Añadir tu llave SSH a este "servidor":**

`ssh-add ruta-donde-guardaste-tu-llave-privada`

(la llave privada es la que no termina en .pub)

-----------------------------------------------------

## Clase 12

Conectar llave publica con github:

Ir a GitHub -> Settings SSH and GPG Keys

Add SSH Key

Copiamos y pegamos en el campo key el contenido del archivo "id_rsa.pub" y le damos en agregar. Escribimos la contraseña de github

Luego:
En nuestro repositorio de github, en el boton "Clone or download" cambiamos a "Use SSH"

Copiamos la url

vamos a nuestro repositorio local y cambiamos el remote con: 

`git remote set-url origin <URL_SSH_GITHUB>`


------------------------------------------------------

## Clase 13

### Tags y versiones en Git y GitHub

`git log --all --graph --decorate --oneline`

Muestra la historia de manera grafica como un arbol

- Para crear alias para el comando anterior (y asi no tener que escribirlo todo):

`alias log-graph="git log --all --graph --decorate --oneline"`

Luego se puede ejecutar el comando escribiendo simplemente: `log-graph`

Luego para ver los alias creados: `alias`

- Para crear un tag de un commit:

`git tag -a <NOMBRE_DEL_TAG> -m "<MENSAJE_DEL_TAG>" <SHA_DEL_COMMIT>`

- Para ver los tags:

`git tag`

- Para ver los commits está conectado un tag:

`git show-ref --tags`

- Para enviar el tag al remoto:

`git push origin --tags`

- Para borrar un tag:

```bash
git tag
git tag -d <NOMBRE_DEL_TAG>
git push origin --tags
```

Para borrarlo en el remoto:

`git push origin :refs/tags/<NOMBRE_DEL_TAG>` ó  `git push origin --delete "nombre de rama o tag"`

------------------------------------------------------

## Clase 14

### Manejo de ramas en GitHub

- Muestra la historia de todas la ramas

`git show-branch --all`

- Abre gitk (el entorno grafico de git)

`gitk`

-----------------------------------------------------

## Clase 15-16-17

- Archivos binarios no se agregan a proyectos (buena practica). Ej: imagenes
- En un entorno profesional se bloquea la rama Master
- Pull request: estado intermedio antes de hacer merge, propio de la herramienta GitHub. En este estado el equipo puede hacer revisión del código

### Forks:

Para mantener nuestra rama del fork actualizada con los cambios que realizan en el repositorio original (desde donde realizamos el fork):

1. Agregamos un nuevo remote con el repositorio desde donde realizamos el fork:

`git remote add upstream <URL_REPOSITORIO_FORK>`

> Nota: por standard se coloca como nombre "upstream", pero podemos colocar cualquier nombre 

`git remote -v` (para verificar que haya quedado ok)

2. `git pull upstream master`: Descarga todos los cambios que están en el repo del fork y así mantenemos el nuestro actualizado

-----------------------------------------------------

## Clase 18

### Github pages

https://pages.github.com/

1. crear un repositorio con:

nombre_usuario_github.github.io 

2. Ir al repositorio creado > settings > github pages. Cambiar rama a la que se quiera mostrar en la sección Source

Tambien podemos encontrar esta sección en cualquier repositorio y podemos habilitar la opción en los settings de ese repositorio


------------------------------------------------------

## Clase 19

### git stash

`git stash pop`: aplica el ultimo stash

`git stash list`: vemos los stash 

`git stash drop`: borramos el ultimo stash

### Git Clean: Limpiar tu proyecto de archivos no deseados

Para eliminar los archivos que están en el working copy (no se le ha hecho add) ejecutar:

`git clean --dry-run`

o

`git clean -f`

- Mostrará todos esos archivos borrados en un listado
- No borra las carpetas
- No borra los archivos incluidos en el gitignore

----------------------------------------------------

## Clase 20

### cherry-pick

```bash
git stash
git stash branch <NOMBRE_DE_LA_RAMA>
```

Crea una rama con el ultimo stash realizado

1. Ir a rama donde está el commit que nos interesa "copiar":

`git checkout branch`

`git log --one-line` (copio el SHA del commit)

2. Ir a la rama destino donde queremos "pegar" el commit:

```bash
git checkout destino
git cherry-pick <SHA>
```

---------------------------------------------------

## Clase 21

### Git Reset y Reflog: Úsese en caso de emergencia

1. `git reflog`

Muestra la historia de TODOS los movimiento que se han realizado en git

Ej:

-SHA--  --ID---

00b784f HEAD@{7}: checkout: moving from experimento to master

2. Copiar el ID para hacer el reset con:

`git reset <ID>` (equivalente a un reset --soft)

Esto mueve el apuntador del HEAD al punto a donde le indicamos, pero deja lo que estaba sin trackear 

o 

`git reset --hard <SHA>`

Más recomendado, porque nos devuelve y nos deja tal cual como estabamos en ese commit

----------------------------------------------------

## Clase 22

### Buscar en archivos y commits de Git con Grep y log

`git grep -n <palabra>`: Nos busca en los archivos donde está la "palabra" que le indicamos y con el -n nos indica el número exacto de la linea

`git grep -c <palabra>`: Busca la palabra y la cuenta en todos nuestros archivos

> Nota: para usar etiquetas html en la busqueda, usar ""

`git grep -c "<p>"`

`git log -S "<palabra>"`

Busca todos los commits cuyo mensaje contenga la palabra indicada


----------------------------------------------------

## Clase 23

### Comandos y recursos colaborativos en Git y Github

**Estadísticas**

`git shortlog`: Cuáles commits han hecho los miembros del equipo

`git shortlog -sn`: Cantidad de commits que han hecho los miembros del equipo

`git shortlog -sn --all --no-merges`: Cantidad de commits que han hecho los miembros del equipo, excluyendo los merges

**Guardar un comando:**

`git config --global alias.<NOMBRE_COMANDO> "<COMANDO_SIN_GIT>"`

Ej: `git shortlog -sn -all --no-merges`

`git config --global alias.stats "shortlog -sn --all --no-merges"`

**Ver quién escribió o modificó las lineas de un archivo**

`git blame <ARCHIVO>`

Con esto se coloca un rango de líneas:

`git blame <ARCHIVO> -L<NUMERO_LINEA_INICIO>,<NUMERO_LINEA_FIN>`

**Ver la ayuda de un comando**

`git <COMANDO> --help`

**Ver ramas**

remotas -> `git branch -r`

todas -> `git branch -a` (segun los colores podemos ver cuáles son solo locales)

**Cambiar editor de commits:**

`git config --local core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`

