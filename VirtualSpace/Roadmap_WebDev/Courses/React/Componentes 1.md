Los componentes se pueden  relacionar como un conjunto de elementos que cumplen una funciona especifica. Por ejemplo.

* Una card
* Un formulario
* Una título

Los componentes también tienen unas ventajas asociadas  que son:

1. Reutilizan código.
2. Favorecen la separacion de responsabilidades.
3. El código es mas facil de entender.
4. El rendimiento de la aplicacion mejora.

También los componentes tienen unas caracteristicas asociadas, como:

* Renderizan un unico elemento: Esto por ejemplo sugiere que solo hay una etiqueta que se mostrar, veamos un ejemplo:

![[Pasted image 20251219102037.png]]
 
Notese que las etiquetas <h1> y <p> estan estan envueltos en la etiqueta <div>, esto nos indica que  todos los elementos deben estar anidados sobre un solo elemento en este caso un DIV, pero puede ser cualquier otro elemento que sea de apertura y cierre. En react podemos cambiar ese elemento que envuelve  a los demas, por una etiqueta vacia.<></>, entre estas podemos ahora anidar el resto de etiquetas.

