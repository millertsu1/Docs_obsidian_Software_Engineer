![[Pasted image 20251202085825.png]]
* Es el proceso de pensar cosas del mundo real y pasarlas al mundo logico. Como representamos el mundo real al mundo logico.

* El modelado es el proceso que nos permite lograr la asbtraccion o sea traer los datos

* Modelo relacional es el modelo que gestiona la relaciones entre datos, permite que cada tabla se pueda relacionar con otras.

* Antes de pasar al modelo relacional se debe planificar la creacion de este modelo, suele ser grafico de **modelo entidad relacion**.

Las relaciones tienen una caracteristica  conocida como |**cardinalidad**  la cual indica el sentido y la cantidad de relaciones existentes entre una entidad y otra. Estas  relaciones pueden ser:

* **1 a N:** representa una relacion uno a muchos, por ejemplo: una persona puede tener muchos autos(N autos) y muchos auots pueden pertenecer a un personal(notese que se puede leer al reves).
![[Pasted image 20251208103714.png]]

* 1 a 1: Es un tipo de relacion en la cual una tabla se relaciona exclusivamente con otra tabla. Por ejemplo:  a un alumno le pertenece una libreta de apuntes y viceversa una libre pertenece unicamente a un alumno.
![[Pasted image 20251208104004.png]]

* **N a n:** Es un tipo de relacion en la cual dos tablas pueden estar relacionadas con varios elementos de  la tabla 1 con la tabla 2, por ejemplo: muchos alumnos pueden tener muchas materias y muchas materias puede ser asignadas(cursadas), por un alumno.
![[Pasted image 20251208104300.png]]

El siguiente es el ejemplo es una reprensentacion de las tablas usando el diagrama entidad relacion, esta nos permite entender las relaciones vistar arriba.

![[Pasted image 20251208105047.png]]

### Cómo relacionamos las tablas.

Para poder lograr las relaciones entre las tablas requerimos que cada  elemento de cada tabla deba identificarse de forma unica para que no haya confusiones entre valores repetidos, para ello utilizamos 2 tipos de claves:

1. **Las claves primarias(Primary Key)**: Son valores que identifican de manera unica  a cada fila  o registro de la tabla es un identificador unico que no se repita en ningun momento por ejemplo un numero de cliente, el pasaporte podria estar relacionado.
2. **Una clave foranea(Foreign Key)**: es un campo de una tabla cualquiera que sirve para relacionar entre si  con otra  tabla diferente por ejemplo una relacion de la tabla X con la tabla Y. Donde la clave primaria puede entenderse como  que la llave foranea es la Primary Key de otra tabla, asi en una relacion de 2 tablas la Primary Key de la tabla 1 es la Foreign Key de la tabla 2.

## SQL

![[Pasted image 20251209112718.png]]

Al final nosotros vamos a ver esto como una consulta en español, y como su nombre lo indica es consultar con la base de datos en busqueda de información. Con las consultas preguntamos a la base de datos por algun tipo de dato o informacion  usando el Lenguaje SQL para consultarla. Las consultas se clasifican de acuerdo a lo que se quiere obtener al consultar la base de datos.

![[Pasted image 20251209113211.png]]



