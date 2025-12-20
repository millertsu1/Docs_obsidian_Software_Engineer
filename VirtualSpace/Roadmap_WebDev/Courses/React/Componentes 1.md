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
 
Notese que las etiquetas h1 y p, estan estan envueltos en la etiqueta div, esto nos indica que  todos los elementos deben estar anidados sobre un solo elemento en este caso un DIV, pero puede ser cualquier otro elemento que sea de apertura y cierre. En react podemos cambiar ese elemento que envuelve  a los demas, por una etiqueta vacia tambien llamada *Fragment*, entre estas podemos ahora anidar el resto de etiqueta.

![[Pasted image 20251219112958.png]]


* Pueden recibir **props (propiedades)**: Las props son objetos que contienen datos que un componente necesita para renderizarse correctamente. Y estas se envian de forma unidireccional de un *componente padre* a un *componente hijo*.

Hemos creado un ejemplo creando un nuevo componente llamado **TituloPrincipal** este es el **componente hijo**, de acuerdo a lo anterior podemos entonces deducir que este componente tendra un parametro que sera las propias *props*. Y asi mismo renderizara la informacion  que nos pase el componente padre.

![[Pasted image 20251219115315.png]]
* El componente renderiza un elemento que es el fragment con dos elementos  anidados, un h1 y un h2, estos son las props que vienen del padre a los hijos.
* Cabe destacar que las props pueden ser varias en este caso el titulo y un subtitulo pero pueden ser otras etiquetas.

Ahora en el componente padre tenemos que hacer  ajustes:

1. Importar el componente hijo.
![[Pasted image 20251219115726.png]]

2. Llamar al componente hijo que se rendizara, recordemos que en un componente la parte que renderiza( que se muestra), es dentro del return. Alli debemos llamar el componente hijo y dentro como si establecieramos  atributos a una etiqueta normal agregamos las props que le pasaremos.

![[Pasted image 20251219122303.png]]