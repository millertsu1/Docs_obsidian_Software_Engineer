#Learning  #JavaScript #HowToDo 

El camino a React puede parecer intimidante, sobre todo sin conocer el camino de que debes saber para aprender sobre JavaScript para dar el paso a React, en este mani-curso podrás aprender lo básico necesario para pasar a React sin presentar problemas.

1. `módulos`: los módulos nos ayudan a conectar archivos en JavaScript , traer una funcionalidad hecha en otro archivo y llevarla a donde nosotros necesitemos, así pues al cambiar a React debemos tener en cuenta los módulos para comunicar funcionalidades entre componentes. En JavaScript, los módulos son una forma de dividir el código en partes mas pequeñas y manejables. Cada modulo puede tener su propio ámbito, lo que significa que las variables y funciones definidas en un modulo no interfieren con las de otros módulos a menos que se exporten e importen explícitamente.

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

En el caso de las importaciones nombradas la funcionalida