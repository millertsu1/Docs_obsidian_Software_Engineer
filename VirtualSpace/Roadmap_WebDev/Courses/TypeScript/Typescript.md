#Learning 

### [Conceptos Iniciales de TypeScript](https://docs.google.com/presentation/d/1m5HRJzak6AX4W1Oj0oPIvibWq0RQNFHHKCsUuHxSmlo/edit?usp=sharing)
## Booleans

En TypeScript, al igual que en JavaScript y la mayoría de los lenguajes de programación, el tipo de dato **booleano** (`boolean`) se utiliza para representar valores de verdad o falsedad. Solo tiene dos posibles valores: `true` (verdadero) y `false` (falso).

### ¿Qué son los booleanos y por qué son importantes?

Los booleanos son la base de la lógica condicional y el control de flujo en cualquier programa. Permiten que tu código tome decisiones basadas en si una condición es verdadera o falsa.

**Propósito principal:**

- Representar un estado binario (sí/no, encendido/apagado, válido/inválido).

- Controlar la ejecución de bloques de código a través de sentencias `if`, `else`, `while`, etc.

- Retornar el resultado de operaciones lógicas o comparaciones.

### Sintaxis y Declaración

Declarar una variable booleana en TypeScript es muy sencillo. Puedes hacerlo de forma explícita o dejar que TypeScript infiera el tipo.

1. **Declaración explícita:** Usando el tipo `boolean`.

```TypeScript
let estaActivo: boolean = true;
let esMayorDeEdad: boolean = false;
let tienePermiso: boolean; // Declarada sin inicializar, su tipo es 'boolean'
  
estaActivo = false; // Puedes reasignar un valor booleano
// estaActivo = "true"; // Error: El tipo 'string' no es asignable al tipo 'boolean'
```

2. **Inferencia de tipo:** Si inicializas la variable directamente con un valor booleano, TypeScript inferirá automáticamente que es de tipo `boolean`.

```TypeScript
let hayErrores = false; // TypeScript infiere: boolean
let procesoFinalizado = true; // TypeScript infiere: boolean

// hayErrores = 1; // Error: El tipo 'number' no es asignable al tipo 'boolean'
```

> *Aunque la inferencia es conveniente, para mayor claridad, especialmente en declaraciones de funciones o APIs públicas, es una buena práctica tipar explícitamente.*


### Operadores Booleanos (Lógicos)

TypeScript soporta los mismos operadores lógicos que JavaScript para combinar o manipular valores booleanos:

- **AND lógico (`&&`):** Devuelve `true` si ambos operandos son `true`.

```TypeScript

let esAdministrador = true;
let tieneLicencia = true;
let puedeAcceder = esAdministrador && tieneLicencia; // true
console.log(puedeAcceder);

let esAnonimo = false;
let tieneCuenta = true;
let puedeComentar = esAnonimo && tieneCuenta; // false
console.log(puedeComentar);
```

- **OR lógico (`||`):** Devuelve `true` si al menos uno de los operandos es `true`.

```TypeScript
let tieneDinero = false;
let tieneTarjeta = true;
let puedeComprar = tieneDinero || tieneTarjeta; // true
console.log(puedeComprar);
 
let llueve = false;
let haceFrio = false;
let salirConAbrigo = llueve || haceFrio; // false
console.log(salirConAbrigo);
```

- **NOT lógico (`!`):** Invierte el valor booleano (de `true` a `false`, y de `false` a `true`).

```TypeScript
let estaConectado = true;
let estaDesconectado = !estaConectado; // false
console.log(estaDesconectado);

let isLoading = false;
if (!isLoading) {
  console.log("Los datos han terminado de cargar.");
}
```

### Operadores de Comparación (que devuelven booleanos)

La mayoría de las operaciones de comparación en TypeScript (y JavaScript) resultan en un valor booleano:

- `==` (igualdad suelta)

- `!=` (desigualdad suelta)

- `===` (igualdad estricta)

- `!==` (desigualdad estricta)

- `>` (mayor que)

- `<` (menor que)

- `>=` (mayor o igual que)

- `<=` (menor o igual que)

```TypeScript
let num1 = 10;
let num2 = 5;

let esIgual = (num1 === num2); // false
let esMayor = (num1 > num2);   // true
let esDiferente = (num1 !== num2); // true

console.log(esIgual);
console.log(esMayor);
console.log(esDiferente);
```

### Contextos Booleanos (Truthiness y Falsiness)

Al igual que en JavaScript, TypeScript respeta el concepto de "truthiness" y "falsiness". Esto significa que muchos valores no booleanos pueden ser tratados como `true` o `false` en contextos donde se espera un booleano (como en condiciones `if`).

**Valores "falsy" (que se evalúan a `false`):**

- `false` (el booleano literal)

- `0` (el número cero)

- `-0` (el número cero negativo)

- `""` (la cadena de texto vacía)

- `null`

- `undefined`

- `NaN` (Not a Number)


**Todos los demás valores son "truthy" (se evalúan a `true`), incluyendo:**

- Cadenas no vacías (e.g., `"hola"`)

- Números distintos de cero (e.g., `1`, `-5`)

- Arrays (incluso vacíos, `[]`)

- Objetos (incluso vacíos, `{}`)

- Funciones


**Ejemplo:**

```TypeScript
let nombreUsuario: string = "";
if (nombreUsuario) { // nombreUsuario es "falsy" aquí, la condición es false
    console.log("El usuario tiene un nombre.");
} else {
    console.log("El nombre de usuario está vacío."); // Se ejecuta
}

let listaItems: string[] = [];
if (listaItems) { // listaItems es "truthy" aquí (aunque esté vacía), la condición es true
    console.log("La lista existe."); // Se ejecuta
}
if (listaItems.length > 0) { // Esta es la forma correcta de verificar si un array tiene elementos
    console.log("La lista tiene elementos.");
} else {
    console.log("La lista está vacía."); // Se ejecuta
}
```

Es importante entender `truthiness` y `falsiness`, pero para claridad y evitar errores, a menudo es mejor hacer comparaciones explícitas (`if (nombreUsuario !== '')` o `if (listaItems.length > 0)`).

### Uso de Booleanos en Funciones

Los booleanos se utilizan comúnmente como parámetros o tipos de retorno en funciones para indicar el resultado de una operación o una bandera de estado.

```TypeScript
// Función que retorna un booleano
function usuarioExiste(id: number): boolean {
    // Lógica para buscar usuario
    return id === 123; // Devuelve true si el id es 123, false en caso contrario
}

let existe = usuarioExiste(456);
if (existe) {
    console.log("El usuario ha sido encontrado.");
} else {
    console.log("El usuario no existe.");
}

// Función que toma un booleano como parámetro
function toggleTema(modoOscuro: boolean) {
    if (modoOscuro) {
        console.log("Cambiando a modo oscuro.");
        // Aplicar estilos oscuros
    } else {
        console.log("Cambiando a modo claro.");
        // Aplicar estilos claros
    }
}

toggleTema(true);
```

### Booleanos Literales (Narrowing)

TypeScript puede inferir tipos booleanos literales. Esto ocurre cuando se usa `const` o cuando el tipo de un valor es estrictamente `true` o `false`. Esto es parte del "narrowing" (estrechamiento de tipos) que TypeScript realiza.

```TypeScript
const isAdmin = true; // Tipo inferido: true (literal, no solo boolean)

function checkAccess(userRole: "admin" | "guest", isActive: boolean) {
    if (isActive) {
        // En este bloque, TypeScript sabe que 'isActive' es estrictamente true
        console.log("Usuario activo.");
    }
    if (!isActive) {
        // En este bloque, TypeScript sabe que 'isActive' es estrictamente false
        console.log("Usuario inactivo.");
    }
}
```

>*Esto es poderoso porque TypeScript puede hacer un seguimiento más preciso del flujo de control y las posibles propiedades/métodos disponibles.*

### Resumen Clave

- El tipo `boolean` solo puede ser `true` o `false`.

- Es fundamental para la lógica condicional y el control de flujo.

- Puedes declararlos explícitamente (`: boolean`) o dejar que TypeScript los infiera.

- Utiliza operadores lógicos (`&&`, `||`, `!`) y de comparación para trabajar con booleanos.

- Entiende el concepto de `truthiness` y `falsiness` de JavaScript, pero prefiere comparaciones explícitas para mayor claridad en TypeScript.

- Son muy comunes como parámetros o valores de retorno en funciones.
___
## Numbers


En TypeScript, el tipo de dato **número** (`number`) es fundamental y representa valores numéricos. A diferencia de otros lenguajes de programación que distinguen entre enteros, flotantes, etc., TypeScript (y JavaScript) maneja todos los números como **valores de punto flotante de doble precisión (64 bits)**. Esto significa que no hay una separación explícita entre `int`, `float`, `double`, etc.; todo es simplemente `number`.

### ¿Qué son los números y por qué son importantes en TypeScript?

Los números son esenciales para realizar cálculos matemáticos, representar cantidades, precios, edades, coordenadas, y cualquier otro dato cuantitativo.

**Propósito principal:**

- Realizar operaciones aritméticas (suma, resta, multiplicación, división, módulo).

- Representar magnitudes y valores cuantitativos.

- Utilizarlos en comparaciones y condiciones lógicas.

### Sintaxis y Declaración

Declarar una variable numérica en TypeScript es directo. Puedes especificar el tipo `number` explícitamente o confiar en la inferencia de tipos.

1. **Declaración explícita:** Usando el tipo `number`.

```typescript
let edad: number = 30;
let precio: number = 99.99;
let cantidad: number; // Declarada sin inicializar, su tipo es 'number'

cantidad = 150; // Puedes reasignar un valor numérico
// cantidad = "veinte"; // Error: El tipo 'string' no es asignable al tipo 'number'
 ```

2. **Inferencia de tipo:** Si inicializas la variable directamente con un valor numérico, TypeScript inferirá automáticamente que es de tipo `number`.

```typescript
let puntos = 1000;      // TypeScript infiere: number
let temperatura = 23.5; // TypeScript infiere: number
  
// puntos = "cien"; // Error: El tipo 'string' no es asignable al tipo 'number'
 ```

> *Como siempre, aunque la inferencia es útil, para mayor claridad en APIs o cuando el contexto no es obvio, explicitar el tipo es una buena práctica.*


### Diferentes Formatos de Números

TypeScript soporta los mismos formatos de números que JavaScript:

- **Decimal:** El formato más común.

```typescript
let dec: number = 25;
let pi: number = 3.14159;
```

- **Hexadecimal:** Prefijo `0x`.

```typescript
let hex: number = 0x19; // Representa 25 en decimal
```

- **Binario:** Prefijo `0b`. (Requiere ES6 o superior para compilación)


```typescript
let bin: number = 0b11001; // Representa 25 en decimal
```

- **Octal:** Prefijo `0o`. (Requiere ES6 o superior para compilación)
  
```typescript
let oct: number = 0o31; // Representa 25 en decimal
```

- **Notación exponencial:** Para números muy grandes o muy pequeños.

```typescript
let billones: number = 1.2e9;  // 1,200,000,000
let nanometros: number = 5e-7; // 0.0000005
```

- **Separadores de números (`_`):** Para mejorar la legibilidad de números grandes.


```typescript
let poblacionMundial: number = 8_000_000_000; // 8,000,000,000
```


### Operadores Numéricos (Aritméticos)

Puedes usar todos los operadores aritméticos estándar de JavaScript con números:

- `+` (Suma)

- `-` (Resta)

- `*` (Multiplicación)

- `/` (División)

- `%` (Módulo - resto de la división)

- `**` (Exponenciación - ES7/ES2016)

- `++` (Incremento)

- `--` (Decremento)


```typescript
let a = 10;
let b = 3;

console.log(a + b); // 13
console.log(a - b); // 7
console.log(a * b); // 30
console.log(a / b); // 3.3333...
console.log(a % b); // 1
console.log(a ** b); // 1000 (10^3)

a++; // a ahora es 11
b--; // b ahora es 2
```

### Propiedades y Métodos del Objeto `Number`

El tipo `number` en TypeScript (y JavaScript) también tiene acceso a propiedades y métodos estáticos del objeto global `Number`, así como métodos de instancia.

**Propiedades estáticas útiles:**

- `Number.MAX_VALUE`: El número más grande representable.

- `Number.MIN_VALUE`: El número positivo más pequeño representable.

- `Number.NaN`: Representa "Not a Number".

- `Number.POSITIVE_INFINITY`: Representa el infinito positivo.

- `Number.NEGATIVE_INFINITY`: Representa el infinito negativo.


**Métodos estáticos útiles:**

- `Number.isNaN(value)`: Verifica si un valor es `NaN` de forma más robusta que `isNaN()`.

- `Number.isFinite(value)`: Verifica si un valor es un número finito.

- `Number.isInteger(value)`: Verifica si un valor es un entero.

- `Number.parseFloat(string)`: Convierte una cadena a un número de punto flotante.

- `Number.parseInt(string, radix)`: Convierte una cadena a un entero.


```typescript
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
console.log(Number.isInteger(123));     // true
console.log(Number.isInteger(123.5));   // false
console.log(Number.isNaN(0 / 0));       // true
console.log(Number.isNaN("hello"));     // false (¡Importante! parseInt("hello") devolvería NaN, pero Number.isNaN() verifica si el valor *ya es* NaN)
```

**Métodos de instancia comunes:**

- `toFixed(digits)`: Formatea un número usando notación de punto fijo.

```typescript
let num = 123.456;
console.log(num.toFixed(2)); // "123.46" (retorna un string)
```

- `toPrecision(precision)`: Formatea un número a una longitud especificada.
   
```typescript
let grande = 12345.678;
console.log(grande.toPrecision(5)); // "12346" (redondea)
```

- `toString(radix)`: Convierte un número a su representación de cadena, opcionalmente en una base diferente.

```typescript
let valor = 255;
console.log(valor.toString());    // "255"
console.log(valor.toString(16));  // "ff" (hexadecimal)
console.log(valor.toString(2));   // "11111111" (binario)
```


### El Problema de `NaN` (Not a Number) e Infinito

Es importante estar consciente de que las operaciones numéricas pueden resultar en `NaN` o `Infinity`. TypeScript te permite asignar estos valores a variables de tipo `number` porque son parte del dominio del tipo `number`.


```typescript
let resultadoInvalido: number = 0 / 0; // NaN
let resultadoInfinito: number = 1 / 0; // Infinity
let resultadoNegativoInfinito: number = -1 / 0; // -Infinity

console.log(resultadoInvalido);         // NaN
console.log(resultadoInfinito);         // Infinity
console.log(resultadoNegativoInfinito); // -Infinity

// Siempre usa Number.isNaN() para verificar NaN de forma segura
if (Number.isNaN(resultadoInvalido)) {
    console.log("El resultado es Not a Number.");
}
```

### Precisión de Punto Flotante

Un punto crucial a recordar es que, debido a la forma en que las computadoras representan los números de punto flotante, pueden ocurrir imprecisiones en cálculos con decimales.


```shell
console.log(0.1 + 0.2); // Salida: 0.30000000000000004 (no exactamente 0.3)
```

Para cálculos financieros o que requieren alta precisión, a menudo se usan librerías de terceros (como `decimal.js` o `big.js`) o se trabaja con enteros (e.g., centavos en lugar de dólares).

### Números Literales (Narrowing)

Al igual que con los booleanos, TypeScript puede inferir tipos de números literales, especialmente con `const`.

```typescript
const MAX_INTENTOS = 3; // Tipo inferido: 3 (literal)

function procesarPago(monto: number) {
    if (monto > 1000) {
        // En este bloque, TypeScript sabe que 'monto' es > 1000
        console.log("Monto elevado.");
    }
}
```

### Resumen Clave

- El tipo `number` en TypeScript abarca todos los valores numéricos, incluyendo enteros y decimales, como **punto flotante de doble precisión**.

- Puedes declararlos explícitamente (`: number`) o dejar que TypeScript los infiera.

- Soporta formatos decimal, hexadecimal, binario, octal y exponencial.

- Todos los operadores aritméticos estándar están disponibles.

- El objeto `Number` global proporciona propiedades y métodos útiles para trabajar con números, como `Number.isNaN()` para verificar `NaN`.

- Sé consciente de `NaN` e `Infinity` como resultados de operaciones numéricas.

- Recuerda la posible **imprecisión de punto flotante** y considera soluciones alternativas para cálculos críticos de precisión.

---
## Strings 

En TypeScript, el tipo de dato **string** (`string`) se usa para representar secuencias de caracteres, es decir, texto. Son fundamentales para manejar cualquier tipo de información textual, desde nombres de usuario y mensajes hasta contenido de documentos y URLs.

### ¿Qué son los Strings y por qué son importantes en TypeScript?

Un string es una secuencia inmutable de caracteres. Inmutable significa que, una vez que un string es creado, no se puede cambiar directamente; cualquier operación que parezca modificar un string en realidad crea un _nuevo_ string.

**La ventaja en TypeScript:** Al igual que con otros tipos, TypeScript te permite especificar que una variable debe contener solo valores de tipo `string`. Esto previene errores comunes donde, por ejemplo, intentarías realizar operaciones numéricas en un valor que en realidad es un texto.

**Beneficios de tipar strings en TypeScript:**

- **Seguridad de tipo:** TypeScript te avisará si intentas asignar un valor no textual a una variable `string`.

- **Autocompletado:** Tu editor de código podrá ofrecerte automáticamente todos los métodos y propiedades relevantes para strings (como `length`, `toUpperCase()`, `slice()`, etc.).

- **Claridad del código:** Hace explícita la intención de que una variable contendrá texto.

### Sintaxis y Declaración

Puedes declarar variables de tipo `string` de forma explícita o dejar que TypeScript infiera el tipo.

1. **Declaración explícita:** Usando el tipo `string`.

```typescript
let nombre: string = "Alicia";
let mensaje: string = "Hola, mundo!";
let pais: string; // Declarada sin inicializar, su tipo es 'string'

pais = "España"; // Puedes reasignar un valor string
// pais = 123; // Error: El tipo 'number' no es asignable al tipo 'string'
 ```

2. **Inferencia de tipo:** Si inicializas la variable directamente con un valor string, TypeScript inferirá automáticamente que es de tipo `string`.

  
```typescript
let ciudad = "París"; // TypeScript infiere: string
let producto = 'Laptop'; // TypeScript infiere: string

// ciudad = true; // Error: El tipo 'boolean' no es asignable al tipo 'string'
```

### Comillas para Strings

En TypeScript, puedes usar tres tipos de comillas para definir strings, heredadas de JavaScript:

1. **Comillas dobles (`"`):**

 
```typescript
let fraseDoble: string = "Esto es una frase con comillas dobles.";
```

2. **Comillas simples (`'`):**

```typescript
let fraseSimple: string = 'Esto es una frase con comillas simples.';
```

> *La elección entre comillas dobles y simples es principalmente una cuestión de estilo personal o de las convenciones de tu equipo.*

3. **Backticks (`` ` ``) - Template Literals / Plantillas de Cadena (ES6/ES2015):** Estas son las más potentes y versátiles. Permiten:

- **Strings multilínea:** Puedes escribir strings que abarcan múltiples líneas sin caracteres de escape especiales.

- **Interpolación de expresiones:** Puedes incrustar expresiones JavaScript (variables, llamadas a funciones, etc.) directamente dentro del string usando `${}`.


```typescript
let usuario = "Carlos";
let edad = 30;

// String multilínea
let saludoMultilinea: string = `Hola, ${usuario}.
Bienvenido a nuestro sistema.
Tienes ${edad} años.`;
console.log(saludoMultilinea);
/* Salida:
Hola, Carlos.
Bienvenido a nuestro sistema.
Tienes 30 años.
*/

// Interpolación de variables
let saludoInterpolado: string = `El usuario es ${usuario} y su edad es ${edad}.`;
console.log(saludoInterpolado); // Salida: El usuario es Carlos y su edad es 30.

// Interpolación de expresiones
let precioUnitario = 15;
let cantidad = 2;
let total: string = `El total es $${precioUnitario * cantidad}.`;
console.log(total); // Salida: El total es $30.
```

> Los _template literals_ son altamente recomendados por su legibilidad y funcionalidad, especialmente cuando necesitas construir strings complejos o dinámicos.


### Operaciones Comunes con Strings

TypeScript permite todas las operaciones de string estándar de JavaScript:

- **Concatenación (`+`):** Unir dos o más strings.

```typescript
let primerNombre = "Ana";
let apellido = "García";
let nombreCompleto = primerNombre + " " + apellido;
console.log(nombreCompleto); // Salida: "Ana García"
```

- **Propiedad `length`:** Obtener la longitud de un string.
  
```typescript
let texto = "TypeScript";
console.log(texto.length); // Salida: 10```
```

- **Acceso a caracteres:** Aunque los strings son inmutables, puedes acceder a caracteres individuales por su índice (como en un array), pero no modificarlos.

```typescript
let palabra = "Hola";
console.log(palabra[0]); // Salida: "H"
// palabra[0] = "J"; // Error en tiempo de ejecución de JS, no recomendado en TS
```

### Métodos de String

El tipo `string` viene con una rica colección de métodos para manipular texto. Aquí hay algunos de los más comunes:

- `toLowerCase()`: Convierte el string a minúsculas.
  
```typescript
let cadena = "HELLO WORLD";
console.log(cadena.toLowerCase()); // "hello world"
```

- `toUpperCase()`: Convierte el string a mayúsculas.

```typescript
let cadena2 = "hello world";
console.log(cadena2.toUpperCase()); // "HELLO WORLD"
```

- `trim()`: Elimina los espacios en blanco del principio y el final del string.


```typescript
let conEspacios = "   Espacios alrededor   ";
console.log(conEspacios.trim()); // "Espacios alrededor"
```

- `slice(startIndex, endIndex?)`: Extrae una parte de un string.

```typescript
let original = "TypeScript";

console.log(original.slice(0, 4));  // "Type"
console.log(original.slice(4));     // "Script"
```
  
- `substring(startIndex, endIndex?)`: Similar a `slice`, pero maneja los índices negativos de forma diferente.

```typescript
let cadena3 = "Desarrollo";
console.log(cadena3.substring(0, 3)); // "Des"
```

- `indexOf(searchValue, fromIndex?)`: Devuelve el índice de la primera ocurrencia de un valor especificado, o -1 si no se encuentra.
   
```typescript
let frase = "aprender TypeScript es divertido";
console.log(frase.indexOf("TypeScript")); // 9
console.log(frase.indexOf("Java"));      // -1
 ```

- `includes(searchValue, fromIndex?)`: Verifica si un string contiene otro string.

```typescript
let textoGrande = "El camino es largo pero gratificante.";
console.log(textoGrande.includes("largo")); // true
console.log(textoGrande.includes("corto")); // false
```

- `startsWith(searchString, position?)`: Verifica si un string comienza con un valor especificado.

- `endsWith(searchString, length?)`: Verifica si un string termina con un valor especificado.

- `replace(searchValue, replaceValue)`: Reemplaza ocurrencias de un patrón en un string. Puede usar expresiones regulares.

```typescript
let textoViejo = "Hoy es un buen día, un día soleado.";
console.log(textoViejo.replace("día", "mañana")); // "Hoy es una buen mañana, un día soleado." (solo la primera ocurrencia)
console.log(textoViejo.replace(/día/g, "momento")); // "Hoy es un buen momento, un momento soleado." (todas las ocurrencias con regex 'g')
```

- `split(separator, limit?)`: Divide un string en un array de substrings.

``` typescript
let tags = "javascript,typescript,frontend";
let arrayTags = tags.split(",");
console.log(arrayTags); // ["javascript", "typescript", "frontend"]
```

### Strings Literales (Narrowing)

TypeScript puede inferir tipos de strings literales, lo cual es muy útil para escenarios donde el valor de una variable de string es constante y conocido.

TypeScript

```typescript
const STATUS_OK = "OK"; // Tipo inferido: "OK" (literal)
const STATUS_ERROR = "ERROR"; // Tipo inferido: "ERROR" (literal)

type StatusCode = typeof STATUS_OK | typeof STATUS_ERROR; // Tipo: "OK" | "ERROR"

function handleStatus(status: StatusCode) {
    if (status === STATUS_OK) {
        console.log("Todo bien.");
    } else {
        console.log("Hubo un problema.");
    }
}

handleStatus("OK");
// handleStatus("Success"); // Error: El argumento de tipo '"Success"' no es asignable al parámetro de tipo '"OK" | "ERROR"'
```

Este es un ejemplo de cómo TypeScript usa el "narrowing" para refinar los tipos en función de las comprobaciones de flujo de control.

### Strings y Unión de Tipos

Si una variable puede ser un string o algún otro tipo, puedes usar una unión de tipos:

TypeScript

```typescript
let entradaUsuario: string | null = obtenerEntrada(); // Podría ser string o null
if (entradaUsuario !== null) {
    console.log(entradaUsuario.toUpperCase()); // TypeScript sabe que es string aquí
}
```

### Resumen Clave

- El tipo `string` representa secuencias de caracteres (texto).

- Los strings son **inmutables**: las operaciones que los "modifican" en realidad crean nuevos strings.

- Puedes usar comillas dobles (`"`), simples (`'`), o **backticks (`` ` ``)**. Los backticks son recomendados para **strings multilínea** e **interpolación de expresiones (`${}`)**.

- TypeScript te da **seguridad de tipo** y **autocompletado** para todos los métodos y propiedades de string.

- Dispones de una gran cantidad de métodos para manipular strings (cambiar mayúsculas/minúsculas, buscar, reemplazar, cortar, etc.).

- TypeScript puede inferir **strings literales** para un tipado más preciso en ciertos contextos.
---
## Arrays

Los **arrays** en TypeScript, al igual que en JavaScript, son colecciones ordenadas de elementos. La principal diferencia y ventaja en TypeScript es que puedes (y a menudo debes) especificar el tipo de los elementos que contendrá el array. Esto aporta una gran seguridad y claridad a tu código.

### ¿Qué son los Arrays y por qué son importantes en TypeScript?

Un array es una estructura de datos que almacena una lista de valores, donde cada valor tiene una posición (índice) que comienza desde cero.

**La clave en TypeScript:** Mientras que en JavaScript un array puede contener elementos de cualquier tipo (e.g., `[1, "hola", true]`), TypeScript te permite definir que un array solo contenga elementos de un **tipo específico**. Esta es la base de la seguridad de tipos que TypeScript ofrece para los arrays.

**Ventajas de tipar arrays en TypeScript:**

- **Detección temprana de errores:** TypeScript te avisará en tiempo de compilación si intentas añadir un elemento del tipo incorrecto al array.

- **Autocompletado inteligente:** Tu editor de código podrá ofrecerte sugerencias de métodos y propiedades relevantes para el tipo de datos que maneja el array.

- **Legibilidad del código:** Hace que tu intención sea clara para otros desarrolladores (y para ti mismo en el futuro).


### Sintaxis para declarar Arrays

Hay dos formas principales de declarar arrays en TypeScript:

1. **Usando `Tipo[]` (la forma más común y recomendada):**

```typescript
let edades: Array<number>; // Un array que solo puede contener números
edades = [25, 30, 35]; // Válido
// edades = ["cuarenta", 45]; // Error: El tipo 'string' no es asignable al tipo 'number'
```

2. **Usando `Array<Tipo>` (sintaxis genérica):**

```typescript
let edades: Array<number>; // Un array que solo puede contener números
edades = [25, 30, 35]; // Válido
// edades = ["cuarenta", 45]; // Error: El tipo 'string' no es asignable al tipo 'number'
```

Ambas sintaxis son equivalentes y la elección es a menudo una cuestión de preferencia personal, aunque `Tipo[]` es más concisa y ampliamente utilizada.

### Arrays de Tipos Múltiples (Uniones)

Si necesitas un array que pueda contener elementos de _varios_ tipos específicos (pero no cualquier tipo), puedes usar **tipos de unión**.

```TypeScript
let datosMixtos: (string | number | boolean)[]; // Un array que puede contener strings, números o booleanos
datosMixtos = ["Texto", 123, true, "Otro texto"]; // Válido
// datosMixtos = ["hola", {}, 5]; // Error: El tipo '{}' no es asignable al tipo 'string | number | boolean'
```

> *Recuerda que esto es diferente de una tupla. En un array de unión, el orden y la cantidad de elementos no están fijos, solo los tipos permitidos.*

### Arrays Multidimensionales

Puedes crear arrays de arrays (matrices) para representar estructuras multidimensionales, como una cuadrícula o una tabla.


```Typescript
let matrizNumeros: number[][]; // Un array de arrays de números
matrizNumeros = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

console.log(matrizNumeros[0][1]); // Salida: 2

// Ejemplo de array 3D
let cubo: string[][][];
cubo = [
    [
        ["a1", "a2"],
        ["a3", "a4"]
    ],
    [
        ["b1", "b2"],
        ["b3", "b4"]
    ]
];
```

### Métodos de Array

Una vez que has tipado un array en TypeScript, puedes usar todos los métodos estándar de array de JavaScript (`push`, `pop`, `forEach`, `map`, `filter`, `reduce`, etc.). TypeScript te proporcionará inferencia de tipo para los elementos procesados por estos métodos.


```TypeScript
let numeros: number[] = [10, 20, 30];

numeros.push(40); // Válido
// numeros.push("cinco"); // Error: El argumento de tipo 'string' no es asignable al parámetro de tipo 'number'

let dobles = numeros.map(n => n * 2);
console.log(dobles); // [20, 40, 60, 80] (tipo: number[])

let pares = numeros.filter(n => n % 2 === 0);
console.log(pares); // [10, 20, 30, 40] (tipo: number[])
```

### Arrays de Solo Lectura (Readonly Arrays)

TypeScript te permite crear arrays que, una vez inicializados, no pueden ser modificados. Esto es útil para garantizar la inmutabilidad de los datos. Puedes declararlos de dos maneras:

* Usando `readonly Tipo[]`:

```typescript
let soloLecturaNombres: readonly string[] = ["Ana", "Pedro"];
// soloLecturaNombres.push("Maria"); // Error: La propiedad 'push' no existe en el tipo 'readonly string[]'
// soloLecturaNombres[0] = "Juan"; // Error: Index signature in type 'readonly string[]' only permits reading.
```

* ***Usando `ReadonlyArray<Tipo>` (sintaxis genérica):**

```typescript
let soloLecturaEdades: ReadonlyArray<number> = [18, 20, 22];
// soloLecturaEdades.pop(); // Error: La propiedad 'pop' no existe en el tipo 'ReadonlyArray<number>'
```

> **Importante**: Puedes asignar un array mutable a un array de solo lectura, pero no al revés.*


```Typescript
let mutableArray: string[] = ["A", "B"];
let readonlyArray: readonly string[] = mutableArray; // Válido

// mutableArray = readonlyArray; // Error: El tipo 'readonly string[]' no es asignable al tipo 'string[]'
```

### Inferencia de Tipos en Arrays

TypeScript es lo suficientemente inteligente como para inferir el tipo de un array si lo inicializas directamente al declararlo:

```TypeScript
let frutas = ["Manzana", "Pera", "Naranja"]; // TypeScript infiere: string[]
// frutas.push(123); // Error: El argumento de tipo 'number' no es asignable al parámetro de tipo 'string'

let numerosInferidos = [1, 2, 3]; // TypeScript infiere: number[]

let mezclado = [1, "dos", true]; // TypeScript infiere: (number | string | boolean)[]
```

> *Aunque la inferencia es útil, explícitamente tipar tus arrays es una buena práctica para mayor claridad y control.*

### Diferencia clave entre Arrays y Tuplas

Es crucial entender la distinción:

- **Arrays:** Colecciones de elementos **del mismo tipo** (o de tipos de unión compatibles), con **longitud variable**.
  
```TypeScript
let listaNumeros: number[] = [1, 2, 3, 4, 5]; // Puede crecer o decrecer
```

- **Tuplas:** Arrays con un **número fijo de elementos** y donde **cada posición tiene un tipo específico** (pueden ser diferentes).

```TypeScript
let punto: [number, number] = [10, 20]; // Siempre dos elementos, el primero number, el segundo number
let empleadoInfo: [string, number, boolean] = ["Juan", 30, true]; // Siempre tres elementos con tipos específicos
```

### Cuándo usar Arrays

- Cuando tienes una colección de elementos que son **todos del mismo tipo** (o un conjunto limitado de tipos de unión).

- Cuando la **longitud de la colección puede variar**.

- Cuando el **orden de los elementos es importante**, pero no la composición fija de tipos por posición.

- Para la mayoría de las listas de datos homogéneos (listas de usuarios, productos, mensajes, etc.).
___
## Tuplas


Las tuplas en TypeScript son un tipo de array que te permite expresar un array donde el tipo de un número fijo de elementos es conocido, pero no necesariamente el mismo. Son una forma de representar datos heterogéneos pero ordenados. Piensa en ellas como arrays con "posiciones fijas" y "tipos fijos" para cada posición.

### ¿Qué son las tuplas y por qué usarlas?

las tuplas con conjuntos de valores ordenados que se podrían maneja como un array  con valores y posiciones fijas inmutables, a diferencia del array, no son espacios de memoria que cambian su longitud, sino son mas listas fijas e inmutables de valores con un orden.

#### Propósitos de una Tupla

- Representar un conjunto de valores donde el orden y el tipo de cada elemento son importantes y predefinidos.

- Cuando necesitas un número fijo de elementos con tipos posiblemente diferentes.

- Para funciones que devuelven múltiples valores relacionados.

#### Ventajas sobre un array normal (`any[]` o `(number | string)[]`):

- **Seguridad de tipo:** TypeScript te obliga a respetar los tipos y el orden definidos.

- **Claridad de intención:** Deja claro qué tipo de datos se espera en cada posición.

- **Detección de errores en tiempo de compilación:** Atrapa errores de tipo o longitud antes de ejecutar el código.

### Sintaxis

La sintaxis de una tupla es similar a la de una array, pero especificando el tipo para cada posición:

```Typescript
type NombreTupla = [Tipo1, Tipo2, Tipo3, ...];
```

```Typescript

// Ejemplo: Una tupla para coordenadas [x, y]
let coordenada: [number, number];
coordenada = [10, 20]; // Válido
// coordenada = [10, "veinte"]; // Error: Tipo 'string' no es asignable al tipo 'number'
```

```Typescript
// Ejemplo: Información de usuario [id, nombre, activo]
let usuario: [number, string, boolean];
usuario = [1, "Alice", true]; // Válido
// usuario = [1, "Bob", "activo"]; // Error: Tipo 'string' no es asignable al tipo 'boolean'
// usuario = [1, "Charlie"]; // Error: La propiedad '2' falta en el tipo '[number, string]'
```

```Typescript
// Ejemplo: Un color RGB [red, green, blue]
let color: [number, number, number];
color = [255, 0, 128];
```

### Acceso a Elementos de Tupla

Puedes acceder a los elementos de una tupla utilizando la notación de corchetes, como lo harías con un array normal. TypeScript te dará inferencia de tipo correcta para cada posición.

```Typescript
let persona: [string, number];
persona = ["Juan", 30];

console.log(persona[0]); // "Juan" (tipo: string)
console.log(persona[1]); // 30 (tipo: number)

// persona[2]; // Error: La tupla solo tiene 2 elementos, el índice '2' está fuera de los límites.
```

### Tuplas Opcionales (TypeScript 3.0+)

A partir de TypeScript 3.0, puedes marcar elementos de tupla como opcionales usando el operador `?`. Esto significa que esos elementos pueden estar presentes o no.

```Typescript
type Punto3D = [number, number, number?];

let p1: Punto3D = [10, 20];      // Válido (z es opcional)
let p2: Punto3D = [10, 20, 30]; // Válido (z está presente)

// p1[2] podría ser 'number | undefined'

if (p1[2] !== undefined) {
    console.log(p1[2]);
}
```


>***Importante**: Los elementos opcionales **solo pueden ir al final de la tupla**. No puedes tener un elemento opcional seguido de uno requerido.*

```Typescript
// type InvalidTuple = [number?, string]; // Error
```

### Tuplas con Elementos Rest (TypeScript 3.0+)

Las tuplas también pueden incluir elementos "rest" (`...`)  que permiten un número variable de elementos de un tipo determinado al final de la tupla. Esto las hace más flexibles, actuando como una combinación entre una tupla y un array.

```Typescript
type HeaderAndItems = [string, ...number[]];

let lista1: HeaderAndItems = ["Productos", 1, 2, 3]; // Válido
let lista2: HeaderAndItems = ["Usuarios"];// Válido (el array de números puede estar vacío)
let lista3: HeaderAndItems = ["Precios", 10.5, 20.0]; // Válido

// let lista4: HeaderAndItems = [123, "Productos"]; // Error: El primer elemento debe ser string

console.log(lista1[0]); // "Productos" (tipo: string)
console.log(lista1[1]); // 1 (tipo: number)
console.log(lista1[2]); // 2 (tipo: number)
```

>***Importante:** El elemento rest debe ser el último elemento de la tupla y solo puede haber uno.*


### Tuplas de Solo Lectura (Readonly Tuples - TypeScript 3.4+)

Puedes hacer que una tupla sea de solo lectura usando el modificador `readonly`. Esto significa que, una vez asignada, sus elementos no pueden ser modificados.

```Typescript
let r_coordenada: readonly [number, number] = [10, 20];

// r_coordenada[0] = 5; // Error: Index signature in type 'readonly [number, number]' only permits reading.

// Puedes reasignar la tupla completa, pero no modificar sus elementos individuales
r_coordenada = [30, 40]; // Válido
```

>***Importante**: Las tuplas de solo lectura son útiles cuando quieres garantizar la inmutabilidad de un conjunto de valores.*

### Desestructuración de Tuplas

Las tuplas **se pueden desestructurar de la misma manera que los arrays**, lo que es una forma conveniente de extraer sus valores en variables individuales.

```typescript
let empleado: [string, number, string] = ["Ana", 28, "Desarrolladora"];

// Desestructuración
let [nombre, edad, puesto] = empleado;

console.log(nombre); // "Ana"
console.log(edad);   // 28
console.log(puesto); // "Desarrolladora"
```

### Tuplas como Parámetros y Retorno de Funciones

Las tuplas son muy útiles para definir los tipos de parámetros o los tipos de retorno de funciones cuando necesitas pasar o recibir un conjunto fijo y ordenado de valores.

```Typescript
// Función que devuelve una tupla [booleano, string]
function buscarUsuario(id: number): [boolean, string] {
    if (id === 1) {
        return [true, "Usuario encontrado: Alice"];
    } else {
        return [false, "Usuario no encontrado"];
    }
}

let [encontrado, mensaje] = buscarUsuario(1);
console.log(encontrado); // true
console.log(mensaje);   // "Usuario encontrado: Alice"

let [res, msg] = buscarUsuario(5);
console.log(res); // false
console.log(msg); // "Usuario no encontrado"

// Función que acepta una tupla como parámetro
function procesarCoordenada(coord: [number, number]) {
    console.log(`X: ${coord[0]}, Y: ${coord[1]}`);
}

procesarCoordenada([100, 200]);
```

### Cuando NO usar Tuplas

* **Cuando el número de elementos es variable y los tipos son homogéneos:** Usa un `Array<Tipo>` o `Tipo[]`.

```Typescript
// No uses [number, number, number] si quieres un array de números de longitud variable
let numeros: number[] = [1, 2, 3, 4, 5]; // Correcto
```

* **Cuando los elementos no tienen un orden o significado posicional inherente:** Considera usar un objeto con propiedades con nombre para mayor claridad.

```Typescript
// Mal uso de tupla para datos sin un orden fijo:
let userInfo: [string, string, number] = ["Juan", "Perez", 30]; // ¿Qué representa cada string?

// Mejor usar un objeto:
interface User {
    firstName: string;
    lastName: string;
    age: number;
}
let user: User = { firstName: "Juan", lastName: "Perez", age: 30 };
```

> ***Importante**: Los objetos son más legibles cuando la relación entre los datos es por nombre de propiedad, no por posición.*


### Compatibilidad y Asignación de Tuplas

TypeScript maneja la compatibilidad de tuplas basándose en el número de elementos y sus tipos correspondientes en cada posición.

- Una tupla puede ser asignada a otra si tienen la misma longitud y los tipos en cada posición son compatibles.

- Una tupla puede ser asignada a un array si el tipo del array es compatible con _todos_ los tipos de la tupla (generalmente `(Tipo1 | Tipo2 | ... )[]`).

```Typescript
let t1: [number, string] = [1, "hello"];
let t2: [number, string] = [2, "world"];

t1 = t2; // Válido, son del mismo tipo de tupla

let arr: (number | string)[];
arr = t1; // Válido, una tupla es un tipo especializado de array
// arr = [1, "hello", true]; // Error, 'true' no es compatible con 'number | string'
```

### Resumen Clave

- Las tuplas son arrays con un número **fijo** de elementos y tipos **predefinidos** para cada posición.

- Proporcionan **seguridad de tipo** y **claridad** en el código.

- Pueden tener elementos **opcionales** (`?`) al final.

- Pueden tener elementos **rest** (`...`) al final para un número variable de elementos del mismo tipo.

- Pueden ser **de solo lectura** (`readonly`) para inmutabilidad.

- Son ideales para funciones que devuelven múltiples valores relacionados.

- Considera objetos si los datos no tienen un orden posicional estricto.


