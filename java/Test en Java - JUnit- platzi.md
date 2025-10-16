# Test en Java: JUnit - Platzi

## Clase 1

### Tipos y beneficios de los tests

#### Beneficios:
- Comprobar los requerimientos de nuestra aplicación.
- Documentación y ejemplos de nuestras clases.
- Mediante Test Driven Development (TDD) nos puede ayudar en el diseño de clases.
- Confianza al desarrollar.
- Confianza para refactorizar nuestro código.
- Es una habilidad que se solicita cada vez más en el mercado.

Existen test automáticos y manuales, los automáticos van a requerir tiempo de desarrollo y algunas veces no serán tan viables, pero de ser posible siempre trata de hacer test automáticos ya que:

- Son más rápidos.
- Son más fiables.
- Son incrementales.

#### Tipos de test:
- Unitario: realizan pruebas a una función o clase muy concreta de nuestro proyecto.
- Integración: prueban cómo se conectan diferentes componentes de nuestro proyecto.
- Funcionales: prueban una funcionalidad de nuestro proyecto, pueden involucrarse varias clases.
- Inicio a fin (end to end): prueba todo un proceso del proyecto.
- Estrés: útil para probar si nuestra aplicación puede soportar grandes cantidades de procesos y peticiones a la vez.

-------------------------------------------------------------

## Clase 2-3-4

IntelliJ IDEA -> Pararse en el nombre de la clase, pulsar alt + enter y aparecerá la opción de crear test a partir de la clase actual

alt + fn + supr -> para poder generar contructores

- `@Test` : anotacion que se agrega a un metodo para indicar que es una prueba unitaria

- Cada método debe probar 1 cosa, y el nombre debe expresar lo que se está probando en ese test

- Para indicarle a JUnit que esperamos una excepción lo debemos hacer de la siguiente forma:

`@Test(expected = IllegalArgumentException.class)`

-------------------------------------------------------------

## Clase 5

### Mockito:

Es una libreria que se utiliza para simular clases

Por ejemplo: 
- si la clase tiene que usar una pasarela de pagos, se puede simular un pago para no realizar un pago real
- Si se están manejando valores aleatorios, para controlar los valores que retorna y así poder hacer las pruebas

> NOTA: en las dependencias de maven colocamos "scope" para indicar en que fase del desarrollo se va a utilizar la libreria

```xml
<dependency>
	<groupId>org.mockito</groupId>
	<artifactId>mockito-core</artifactId>
	<version>2.23.4</version>
	<scope>test</scope>   <<<<<----------- Se utiliza solo en los tests
</dependency>
```

* Para simular la clase:

`Dice dice = Mockito.mock(Dice.class);`

* Para forzar que la llamada a un metodo retorne un valor:

`Mockito.when(dice.roll()).thenReturn(2);`

* Equivalentes de assert:

```Java
assertEquals(false, player.play()); = assertFalse(player.play())
assertEquals(true, player.play()); =  assertTrue(player.play());
```

-------------------------------------------------------------

## Clase 6-7 

```Java
Mockito.when(paymentGateway.requestPayment(Mockito.any()))
                .thenReturn(new PaymentResponse(PaymentResponse.PaymentStatus.OK));
```

> el `Mockito.any()` indica que "al recibir cualquier requestPayment" thenReturn...

Los test normalmente se dividen en tres partes:
1. Preparación del escenario (objetos que van a ser utilizados en el test).
2. Llamada al método que se quiere probar.
3. Comprobación de que el resultado es el esperado.

- La inicialización de los objetos que se realiza en el paso 1 se puede unificar en un método aparte "setup" (declaramos los objetos como atributos de la clase). Este método debe ir anotado con `@Before`, de esta manera le indicamos a jUnit que ejecute ese método antes de cada test

-------------------------------------------------------------

## Clase 8 

### TDD -> Test Driven Development

* El desarrollo está guiado por los tests
* Creada por Kent Beck

El Test Driven Development (TDD) o desarrollo guiado por test, creado por Kent Beck, consiste en escribir primero los test antes que las clases permitiéndote ver si el diseño de una clase es la adecuada.

- El ciclo del TDD
	- Red: escribe un test que falle.
	- Green: escribe el código necesario para que pase el test.
	- Refactor: mejora el código.

- Reglas:
	- Sólo escribirás código de test hasta que falle.
	- Sólo escribirás código de producción para pasar el test.
	- No escribirás más código de producción del necesario.

Puedes combinar las reglas del TDD con su ciclo tal como hizo el profesor:
- Red: Escribirás el mínimo de código test que falle.
- Green: Escribirás el mínimo de código de producción que pase el test.
- Refactor: sólo cuando los tests estén pasando.

-------------------------------------------------------------

## Clase 9-10

assertThat ( < llamada_función_que_se_quiere_probar > , < Matcher > )

Matcher (comprobador) : CoreMatchers viene de la librería Hamcrest

Ej: 

`CoreMatchers.is(true)`

--------------------------------------------------------------

## Clase 11-12

Interfaz -> Negocio -> Datos -> BD

Por lo general una aplicación se divide en:

- Interfaz: Se encarga de la comunicación con el exterior o un usuario.
- Negocio: Es la lógica de nuestra aplicación.
- Datos: Se encarga de guardar los datos de nuestra aplicación.

Cada capa se puede comunicar con otra, pero no conoce los detalles de implementación.

`@After`: El metodo anotado con esto se ejecutará después de cada prueba.

Sirve por ejemplo para reiniciar una base de datos en memoria


--------------------------------------------------------------

## Clase 13

Normalmente, el desarrollo de los proyectos comienza a partir de requerimientos muy bien especificados. Sin embargo, habrá veces donde el programador debe definirlos o acabar de concretarlos. No importa cuál sea el caso, solo podemos empezar a escribir los tests una vez tenemos los requerimientos.

Hacer tests para distintos escenarios:
- Escenario tipico: Ruta feliz
- Escenarios extremos
- Escenarios incorrectos
- Escenarios no previstos (bugs)
