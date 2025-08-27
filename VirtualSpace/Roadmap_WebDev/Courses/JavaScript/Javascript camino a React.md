#Learning  #JavaScript #HowToDo 

[**JavaScript camino a React**](https://docs.google.com/presentation/d/1bxTdEJU93Rc910xn36u453Tfq6E2FGW5oTEL0-6YOEw/edit?slide=id.g364916da781_0_1048#slide=id.g364916da781_0_1048) 
[**Repositorio curso JavaScript**](https://github.com/millertsu1/cursoJavascript)


El camino a React puede parecer intimidante, sobre todo sin conocer a donde lleva ese camino. Que debes saber para aprender sobre JavaScript para dar el paso a React, en este mani-curso podrás aprender lo básico necesario para pasar a React sin presentar problemas.

### 1. módulos: 
los módulos nos ayudan a conectar archivos en JavaScript , traer una funcionalidad hecha en otro archivo y llevarla a donde nosotros necesitemos, así pues al cambiar a React debemos tener en cuenta los módulos para comunicar funcionalidades entre componentes. En JavaScript, los módulos son una forma de dividir el código en partes mas pequeñas y manejables. Cada modulo puede tener su propio ámbito, lo que significa que las variables y funciones definidas en un modulo no interfieren con las de otros módulos a menos que se exporten e importen explícitamente.

```javascript
// Exportacion por defecto

function sumar(a, b) {

  return a + b;

}  

export default sumar; // Exportamos la funcion sumar como el valor por defecto del modulo
```
>Nótese que al final de cada componente exportamos por defecto la función sumar. **Esto no realizara ninguna función**, solo tendrá lista la funcionalidad para que desde otro componente sea llamada dicha funcionalidad.

```javascript
//importacion por defecto

import sumar from './29.modulos.js'; // Importamos la funcion sumar desde el modulo 29.modulos.js
```
>Esta es la sintaxis que nosotros debemos usar para llamar la exportación. En este caso importamos la funcionalidad.

En el caso de las importaciones nombradas la funcionalidad de importar y exportar es igual lo que cambia es la sintaxis al momento de exportar, ya que no solo usaremos una unica  función como en el anterior caso. Sino que podremos exportar las funcionalidades que queramos (mas de una)

```javascript

// Exportacion nombrada

export function sumar2(a, b) {

  return a + b;

}

export function restar(a, b) {

  return a - b;

}

export function multiplicar(a, b) {

  return a * b;

}
```
>Hay 3 funciones que tiene la sintaxis de exportación nombrada al inicio de cada función agregamos la palabra export por cada función a usar el otro lugar de nuestro proyecto.

```javascript

import { sumar2, restar, multiplicar } from './29.modulos.js'; // Importamos las funciones sumar2, restar y multiplicar desde el modulo 29.modulos.js
```
> Aquí en el archivo donde vayamos a usar estas importaciones, llamamos a las funciones, pero nótese que las funciones a usar las ponemos dentro de llaves y separadas por comas. Así es como se importan funcionalidades nombradas

`Importante`: 
- En React las importaciones por defecto son las mas usadas para trabajar con la separación de código, ya que cada componente debe cumplir con una funcionalidad.
- No debemos usar combinaciones de exportaciones  en un mismo archivo, todo debe estar separado.
- Colocar la funcionalidad principal en nuestro archivo con una exportación por defecto.
- Colocar las funcionalidades de utilidades  o variables usando importaciones nombradas.
- En JavaScript puro (sin usar Frameworks), debemos usar el atributo TYPE con el valor MODULE en la etiqueta SCRIPT para que funcionen los módulos.

### 2. Desestructuración

El desestructurado es cómo desempacar una caja 📦. En lugar de llevar la caja entera a todos lados (el objeto o el array), simplemente sacas los elementos que necesitas y los usas de forma individual. este seria un ejemplo simple en lenguaje común del funcionamiento de la desestructuración, pero llevemos esto al siguiente nivel.

Como lo mencionamos anteriormente  podemos usar la desestructuración para los objetos y los arreglos, veamos un par de ejemplos:

1. Desestructuración en Arreglos: imagina que tiene un arreglo que representa tu despensa  con muchos alimentos dentro(carne, arroz, huevos, verduras, pescado), ahora imagina que tienes que hacer una comida con algunos de estos alimentos que tienes en la despensa, solo algunos no todos. La desestructuración es como sacar estos alimentos de la despensa para usarlos para completar tu receta.

```javascript
conts despensa = [carne, arroz, huevos, verduras, pescado]
```
> Al crear el arreglo de despensa, metemos todo en esa despensa, pero al ser una arreglo, no podemos usar individualmente cada alimento dentro en esa lista de alimentos, aunque en la programación podríamos usar otros métodos para desestructurar(sacar los que necesitemos), sin embargo la desestructuración nos evitar escribir mucho código para sacar lo que necesitemos. Veamos un ejemplo sin desestructuración:

```javascript
const numeros =[1,2,3]

let uno = numeros[0],

dos =numeros[1],

tres=numeros[2];

  

console.log(uno,dos,tres);

console.log('muestra los numeros fuera del array');
```
