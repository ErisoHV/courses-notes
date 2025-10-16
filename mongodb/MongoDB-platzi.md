# MongoDB - Platzi

## Clase 1

Tipo de bases de datos no-sql:

1. **Key-Value stores**

Estructura llave-valor

Estructura muy simple por lo que complica modelar muchos casos de la vida real

- Ejemplo: Redis

2. **Graph databases**

Permiten establecer relaciones entre de las entidades

Ejemplo: Neo4j

3. **Wide-column stores**

Poseen 2 llaves: 1 de fila y una de columna

Para sistemas de recomendación

Complejo modelar datos con esta estructura

- Ejemplo: Cassandra, HBase

4. **Document databases**

Permiten guardar estructuras llamadas documentos, dentro de colecciones

Permiten modelar casos de la vida real de manera más sencilla

Se guardan los datos con una estructura JSON

- Ejemplo: MongoDB, CouchBase

------------------------------------------------------------------

## Clase 2

### Definición de MongoDB y su ecosistema (herramientas)

MongoDB:

- No relacional
- Basada en documentos
- Los documentos son almacenados en una estructura llamada BSON (parecido a JSON)
- Distribuida (muchos servidores - cluster MongoDB)
- Escalable de forma horizontal (agregando servidores)
- Schema less: no necesariamente los documentos tienen la misma estructura
- Lenguaje para queries: JS
- Gratis y de código abierto 

NOTA: el cubo de escala tiene 3 ejes:

Y: componentes o módulos de la aplicación

X: copias de los módulos, a través de más servidores o crearción de nuevas instancias

Z: particionamiento de datos. Ejemplo: distintas bases de datos para distintos países.

> ODM = Object document mapper

----------------------------------------------------------------------

## Clase 3

### MongoDB Atlas

- Proveedor de MongoDB
- Alta disponibilidad y escalabilidad, seguro
- Disponible en AWS, GCP y Azure

Importante:
- Crear el o los usuarios para acceder a la base de datos
- Agregar a la lista de IP's las que podrán acceder a la BD

> Security > Network Access > IP Whitelist

-------------------------------------------------------------------

## Clase 4

Se instala desde: https://www.mongodb.com/download-center/community

MongoDB Community Server

Incluir Compass en la instalación

- C:\Program Files\MongoDB\Server\4.0\bin\mongo.exe: Consola de Mongo 

- `show dbs`: Para ver las BD locales

- `use <nombre>`: Crea una BD con el nombre espcificado

- `db`: Muestra la BD usada actualmente

- Inserta una nueva colección:

```JavaScript
db.usuarios.insertOne({name:"Erika", age:29, city:"Sabaneta"})
{
        "acknowledged" : true,	<< ----- Indica que se inserto correctamente
        "insertedId" : ObjectId("5cf88313c2dbe2d1545ef110")
}
```

- `show collections`: Muestra las colecciones

- Buscar:

```JavaScript
db.usuarios.findOne()
{
        "_id" : ObjectId("5cf88313c2dbe2d1545ef110"),
        "name" : "Erika",
        "age" : 29,
        "city" : "Sabaneta"
}
```

--------------------------------------------------------------

## Clase 5

1. Ir a Atlas https://cloud.mongodb.com/ > Clusters y hacer click en connect. Desde ahi copiar el comando de conexión.

2. WINDOWS: Se debe agregar esto a las variables de entorno: `ubicacion_instalacion_mongoDB\bin`

Luego desde un cmd se puede ejecutar el comando de conexión a la BD:

`mongo "mongodb+srv://curso-platzi-0hwob.mongodb.net/test" --username admin`

Va a pedir la contraseña de admin

`show dbs`: Muestra las BD por defecto

```JavaScript
db.inventory.insertOne(
        { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

### MongoDB Compass es la interfaz gráfica de MongoDB.

Hacer click en new connection

Copiar la URL de esta forma: mongodb+srv://curso-platzi-0hwob.mongodb.net/test

Al volver a la interfaz de Compass, detectará que una URL de Mongo está en el portapapeles, si aceptamos completará los campos.

- Seleccionamos SVR record para conexiones seguras

- Authentication con username/password 

### Instalación de los drivers de MongoDB en diferentes lenguajes:

Python: `python -m pip install pymongo`

Node.js: `npm install mongodb --save`

Ruby: `gem install mongoid`

GO: `dep ensure -add go.mongodb.org/mongo-driver/mongo`

- Java con build.gradle:

`compile'org.mongodb:mongo-java-driver:2.12.3'`

- Java con maven:

```xml
<dependencies>
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongo-java-driver</artifactId>
        <version>2.12.3</version>
    </dependency>
</dependencies>
```

------------------------------------------------------------------------

## Clase 6

**Base de datos:**

- Contenedor fisico de colecciones
- Cada BD tiene su archivo propio en el sistema de archivos
- Un cluster puede tener multiples BD's

**Colección:**

- Agrupación de documentos, equivalente a una tabla
- No impone un esquema

**Documentos:**

- Registro dentro de una colección
- Guardado en formato BSON
- Unidad básica de MongoDB

> Un BSON soporta mas tipos de datos que un JSON

---------------------------------------------------------------

## Clase 7

- Insertar un documento en la Coleccion "Inventario":

`db.inventario.insertOne({ item:"canvas", qty:100, tags:["cotton"], size: {h:28,    w:35.5, uom:"cm"}})`

Mongo DB nos indica si se insertó correctamente:

```JavaScript
{
        "acknowledged" : true,     <<<------ Bandera que indica si se insertó
        "insertedId" : ObjectId("5d1038ceeba4e381e627edce") <<<---- Si no especificamos un _id Mongo crea uno autmáticamente
}
```

- Obtener un documento

```JavaScript
db.inventario.findOne()
{
        "_id" : ObjectId("5d1038ceeba4e381e627edce"),
        "item" : "canvas",
        "qty" : 100,
        "tags" : [
                "cotton"
        ],
        "size" : {
                "h" : 28,
                "w" : 35.5,
                "uom" : "cm"
        }
}
```

- Insertar un documento con un _id:

`db.inventario.insertOne({_id:"id1", item:"canvas", qty:100, tags:["cotton"], size: {h:28,    w:35.5, uom:"cm"}})`

`{ "acknowledged" : true, "insertedId" : "id1" }`

> El ID debe ser UNICO

- Mongo tiene atomicidad dentro de los documentos.
 
Si escribimos un documento se realiza una operación, si no es exitosa se hace rollback

- Insertar multiples documentos

```JavaScript
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] )
```

- Conocer todas las operaciones que se pueden realizar sobre una colección: `db.invetory.help()`

- Buscar los documentos que cumplan con el filtro

`db.inventory.find({item:"canvas"}).pretty()`

> NOTA: el pretty() es para que muestre el JSON mas estructurado en la consola

- Contar los elementos que retorna una operacion

`db.inventory.find({item:"canvas"}).count()`

- Retorna un elemento: el primer elemento por orden natural que se da como Mongo guarda los documentos

`db.inventory.findOne()`

- Encontrar el primero que cumpla con el criterio de búsqueda:

`db.inventory.findOne({_id: ObjectId("5d03101fd2181332cfb8f9de")})`

> Si MongoDB fue quien definió el ID se usa el ObjectID, si fue un ID generado por nosotros mismos se puede usar directamente entre ""

- Operacion AND. Se agrega condición al filtro

`db.inventory.findOne({_id: ObjectId("5d03101fd2181332cfb8f9de"), _____AQUI______})`

Se pueden usar operadores para comparar y agregar condiciones a la consulta

Los operadores comienzan con $

- Comparison Query Operators:

`$eq` -> Matches values that are equal to a specified value.

`$gt` -> Matches values that are greater than a specified value.

`$gte` -> Matches values that are greater than or equal to a specified value.

`$in` -> Matches any of the values specified in an array.

`$lt` -> Matches values that are less than a specified value.

`$lte` -> Matches values that are less than or equal to a specified value.

`$ne`	-> Matches all values that are not equal to a specified value.

`$nin` -> Matches none of the values specified in an array.

`db.inventory.findOne({_id: ObjectId("5d03101fd2181332cfb8f9de"), qty:{$lte:1000}})`

- Actualizar un elemento con con criterio de busqueda:

`db.inventory.updateOne({_id: ObjectId("5d1038ceeba4e381e627edce")}, {$set: {qty:130}})`

Se usa el operador SET

El resultado es: `{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }`


- Borrar 1

`db.inventory.deleteOne({status:"A"})`

Resultado: `{ "acknowledged" : true, "deletedCount" : 1 }`

- Borrar todos 

`db.inventory.deleteMany({status:"A"})`

----------------------------------------------------------------

## Clase 8

Creando un entorno virtual para python con venv:
C:\Users\Usuario\AppData\Local\Programs\Python\Python35\python.exe -m venv C:\Users\Usuario\workspace\venv-platzi-mongo

Para activarla:
cd <UBICACION>\Scripts\activate.bat
C:\Users\Usuario\workspace\venv-platzi-mongo\Scripts\activate.bat

Al activar la consola va a mostrar algo así:
(venv-platzi-mongo) C:\<UBICACION_VENV>

Desde ahi moverse a la carpeta del proyecto MongoDB e instalar las dependencias:
pip install -r requirements.txt

Establecer variables de entorno:
set FLASK_APP=platzi-api
set FLASK_ENV=development
seT PLATZI_DB_URI=mongodb+srv://admin:HqcSK2obm8zztMWs@curso-platzi-0hwob.mongodb.net/test

Ejecutar:
flask run

Si se quiere conectar a esta BD desde consola:
mongo "mongodb+srv://admin:HqcSK2obm8zztMWs@curso-platzi-0hwob.mongodb.net/test"

------------------------------------------------------------------------

## Clase 9-10

- Tipos de datos:

	- String -> Texto
	- Boolean -> True / False
	- ObjectId -> existen en Bson y no JSON, siempre son unicos
	- Date -> fechas
	- Numéricos:
		- double
		- integers (32 / 64 bits)
		- decimal (128 bits)
	- Documentos embebidos: 
		- Se pueden establecer relaciones almacenando un documento dentro de otro 
		- Se pueden tener arreglos con documentos dentro 
		- Los documentos no pueden ser > 16MB


------------------------------------------------------------------

## Clase 11

*Decisión de diseño para relación de carreras con cursos:*

- Colocar un arreglo de id's de cursos dentro de las carreras: es más costoso porque las carreras se van a estar consultando mucho, entonces por cada carrera se haría 1 join con cursos para conocer los nombres de los cursos relacionados.

- Colocar un arreglo con los id's y nombres de cursos: se obtiene directamente el nombre del curso al consultar la carrera. Al actualizar un curso, se requiere actualizar el nombre en las carreras con las que esté relacionado, pero esta actualización es menos frecuente que la consulta, lo que nos garantiza que no va a ser tan costoso.

*Decisión de diseño para relación de cursos con clases:*

- Se guardan las clases como array dentro del curso, porque son finitas y no sobrepasarían el límite de 16MB para los documentos de MongoDB

### Operadores para realizar queries y proyecciones

* Filtros: 

`db.inventory.find({status : "A"})`

Se pueden agregar operadores al filtro de la siguiente forma: `{<field> : {<operator1> : <value1> }, ....}`

* Proyecciones:

`db.inventory.find({status : "A"})` - > Esto nos trae toda la información del documento

Para especificar los campos/valores que queremos traer:

`db.inventory.find({status : "A"}, {item: 1, status: 1})`

> Si colocamos 1 quiere decir que el campo se va a traer, en caso contrario colocar 0

El campo _id siempre se va a traer, a menos que explícitamente le indiquemos que no: `{_id: 0}`

* Operadores de comparación:

`$eq`     =

`$gt`     >

`$gte`   >=

`$lt`     <

`$lte`    <=

`$ne`     !=

`$in`     valores dentro de un rango

`$nin`    valores que no estan dentro de un rango

`$and`    Une queries con un AND logico

`$not`    Invierte el efecto de un query

`$nor`    Une queries con un NOR logico

`$or`     Une queries con un OR logico

`$exist`  Docuemntos que cuentan con un campo especifico

`$type`   Docuemntos que cuentan con un campo de un tipo especifico

`$all`    Arreglos que contengan todos los elementos del query

`$elemMatch`    Documentos que cumplen la condicion del $elemMatch en uno de sus elementos

`$size`         Documentos que contienen un campo tipo arreglo de un tamaño especifico.


--------------------------------------------------------------------

## Clase 12-13

La función dumps del driver de Python: Convierte el Bson en Json 

* Consulta un curso por id:

`dumps(db.cursos.find_one({'_id' : ObjectId(id_curso)}))`

* Actualiza y devuelve la cantidad de actualizados:

`db.cursos.update_one({'_id' : ObjectId(curso['_id'])}, {'$set' : {'nombre' : curso['nombre'], 'descripcion' : curso['descripcion'], 'clases' : curso['clases']}}).modified_count`

* Borra un curso:

`db.cursos.delete_one({'_id' : ObjectId(curso_id)}).deleted_count`

* Borrar un curso de una carrera:

Operador `$pull`: The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

* Agregar un curso a una carrera 

`db.carreras.update_one({'_id': ObjectId(json['id_carrera'])}, {'$addToSet': {'cursos': curso}})`

> $addToSet lo que hace es agregar el objeto curso al arreglo cursos, si el arreglo cursos no existe lo crea.

* skip() y limit()

Si tenemos 100 carreras y solamente queremos las primeras 10 podemos ejecutar `db.carreras.find({}).limit(10)` esta nos traerá las primeras 10 carreras.

Ahora si queremos las carreras ubicadas en los puestos 40 y 50 lo que debemos hacer es `db.carreras.find({}).skip(40).limit(10)`

Ejemplos:

```JavaScript
// Arreglo de ejemplo
> use test

> db.inventory.insertMany(

[{ _id: 1, item: { name: "ab", code: "123" }, qty: 15, tags: [ "A", "B", "C" ] },
{ _id: 2, item: { name: "cd", code: "123" }, qty: 20, tags: [ "B" ] },
{ _id: 3, item: { name: "ij", code: "456" }, qty: 25, tags: [ "A", "B" ] },
{ _id: 4, item: { name: "xy", code: "456" }, qty: 30, tags: [ "B", "A" ] },
{ _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [ [ "A", "B" ], "C" ] }]

)

// $or
> db.inventory.find({$or: [{qty: {$gt: 25}}, {qty: {$lte: 15}}]})

// $gte
> db.inventory.find({qty: {$gte: 25}})

// $size
> db.inventory.find({tags: {$size: 2}})

// Insertemos estos documentos de ejemplo en la colección survey
> db.survey.insertMany([
{ _id: 1, results: [ { product: "abc", score: 10 }, { product: "xyz", score: 5 } ] },
{ _id: 2, results: [ { product: "abc", score: 8 }, { product: "xyz", score: 7 } ] },
{ _id: 3, results: [ { product: "abc", score: 7 }, { product: "xyz", score: 8 } ] }
])

// $elemMatch
> db.survey.find(
   { results: { $elemMatch: { product: "xyz", score: { $gte: 8 } } } }
)

> db.survey.find(
   { results: { $elemMatch: { product: "xyz" } } }
```

----------------------------------------------------------------------------

## Clase 14

- Agregaciones: Las agregaciones son operaciones avanzadas que podemos realizar sobre nuestra base de datos con un poco más de flexibilidad en nuestros documentos.

- Pipeline de Agregaciones: Es un grupo de multiples etapas que ejecutan agregaciones en diferentes momentos. Debemos tener muy en cuenta el performance de nuestras agregaciones porque las agregaciones corren en todo el cluster.

`db.collection.aggregate([])`

Tiene las etapas que sean necesarias separadas por `","`

- Map-Reduce: Nos permite definir funciones de JavaScript para ejecutar operaciones avanzadas. 

La función de map nos permite definir o “mappear” los campos que queremos usar y la función reduce nos permite ejecutar operaciones y devolver resultados especiales. 

Por ejemplo: podemos mappear algunos campos y calcular la cantidad de elementos que cumplen ciertas condiciones.

- Agregaciones de propósito único: Funciones ya definidas que nos ayudan a calcular un resultado especial pero debemos tener cuidado porque pueden mejorar o afectar el performance de la base de datos.

Por ejemplo: `count()`, `estimatedDocumentCount` y `distinct`.


--------------------------------------------------------------------------

## Clase 15

### Índices:

Los índices nos ayudan a que nuestras consultas sean más rápidas porque, sin ellos, MongoDB debería escanear toda la colección en busca de los resultados.

#### Tipos de índices:

- De un solo campo: Cuando en nuestra colección solo hay un campo por el cual queremos hacer queries
- Compuestos: Parecido a multi-llaves. Indices sobre múltiples campos
- Multi-llave:
- Geoespaciales: índices sobre longitud y latitud
- De texto: busquedas por texto
- Hashed: convertir valores en hash, las consultas serán más rápidas

**Para ver los indices de una colección:**

`db.cursos.getIndexes()`

**Para crear un indice de texto:**

`db.cursos.createIndex({nombre : "text"})`

**Para buscar por el nombre de curso:**

`db.cursos.find({$text : {$search : "aws"}} , {nombre : 1})`

**Ver las estadísticas del indice utilizado en una consulta:**

`db.cursos.find({$text : {$search : "mongo"}} , {nombre : 1}).explain("executionStats")`

El `“explain(‘executionStats’)”` nos devuelve las estadísticas de la consulta.

https://docs.mongodb.com/manual/tutorial/measure-index-use/


-----------------------------------------------------------------------------

## Clase 16

### Recomendaciones de Arquitectura y Paso a Producción

- Usar proveedores cloud con alta disponibilidad: AWS, Google Cloud o Azure son muy buenas opciones
- No te compliques pensando en administración de servidores con MongoDB, servicios como MongoDB 
> Atlas o mlab son muy buenas opciones
- Guardar las credenciales en variables de entorno o archivos de configuración fuera del proyecto
- Asegura que tu cluster se encuentra en la mis región del proveedor que tu aplicación
- Haz VPC peering entre la VPC de tu aplicación y la VPC de tu cluster
- Cuida la lista de IPs blancas
- Puedes habilitar la autenticación en dos pasos
- Actualiza constantemente tu versión de MongoDB
- Separa los ambientes de desarrollo, test y producción
- Habilita la opción de almacenamiento encriptado
