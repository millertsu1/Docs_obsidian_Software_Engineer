#Articles #HowToDo


Las type assertions (aserciones de tipo) en TypeScript son una forma de decirle al compilador: “Confía en mí, sé que este valor es de un tipo específico”. No cambian el tipo en tiempo de ejecución, solo ayudan al compilador a tratar una variable como un tipo concreto.

### **Hay dos formas de escribirlas**

1. Usando la palabra clave AS:  valor as Tipo
2. usando los signos de mayor y menor que: `<Tipo>valor` (solo fuera de archivos JSX)


Ejemplo:

```typescript
avenger = 'Hulk';
console.log((avenger as string).charAt(0));

avenger =123.5677887978;
console.log(<number>avenger.toFixed(2));
```


En tu archivo, se usan para decirle a TypeScript que `avenger` es un string o un number en ese momento, aunque su tipo real sea any.

### **Cuando usar las aserciones**

Si sabes de antemano el tipo de una variable, lo ideal es declararla con ese tipo (por ejemplo, `let avenger: string`). Así TypeScript te ayuda a evitar errores y a tener un código más seguro.

El uso de type assertion tiene sentido en estos casos:

- Cuando recibes datos de una fuente externa (por ejemplo, una API o un formulario) y sabes que el valor es de cierto tipo, pero TypeScript no puede inferirlo.
- Cuando migras código JavaScript a TypeScript y aún tienes variables de tipo `any`, pero necesitas usar métodos o propiedades específicas de un tipo.
- Cuando por diseño una variable puede cambiar de tipo, pero en un momento puntual necesitas tratarla como un tipo específico.

Lo mejor es declarar el tipo desde el inicio siempre que sea posible. Las type assertions son útiles solo en situaciones donde el tipo no es claro para TypeScript, pero tú como programador sabes que sí lo es.
