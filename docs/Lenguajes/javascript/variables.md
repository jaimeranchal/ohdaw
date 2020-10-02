# Variables

Por recordar un poco, una variable es una posición de memoria que _reservamos_ con un nombre identificativo para poder usar el valor almacenado más adelante.

## Declaración y asignación

Podemos usar **tres palabras clave** para declarar una variable:

- `let`: la variable solo es accesible dentro del bloque en el que se declara.
- `var`: accesible en todo los lugares de la función (no sólo en su bloque); _fuera_ de una función, es accesible por **todo el código**.
- _No declaradas_ : directamente asignando un valor a un nombre.

!!! note "Variables en modo estricto"
    Si usamos `use strict`, siempre tendremos que declarar las variables con `let` o `var`.

```javascript
var edad;
let edad1, edad2, edad3;

edad = 15;
nombreApellidos = "María";

var edad4 = 15;
let edad1, edad2, edad3 = 23;
```

### Reglas de formación

- Sólo caracteres alfanuméricos, el signo del dólar (`$`) y guión bajo (`_`).
- No pueden empezar  por un número.
- No se pueden usar _palabras reservadas_.
- Empezar por **minúscula** (reservamos la mayúscula inicial para las **Clase**).

```javascript
let $ = 1; 
let _ = 2;

alert($ + _); // 3

let 1a; // Error: no puede empezar por un número
let my-name; // Tampoco se permiten guiones 
```

!!! warning "Bis, no, gracias"
    No se puede **declarar dos veces** la misma variable. 

    ```javascript
    var mensaje = "Hey";

    var mensaje = "Hou"; // SyntaxError: la variable 'mensaje' ya está definida
    ```

!!! note "CamelCase"
    No es una norma estricta, pero se recomienda usar el estilo _CamelCase_ para nombrar variables con más de una palabra, en lugar de la alternativa con guiones bajos (Camel_Case).

## Constantes

Para declarar una variable con un valor inmutable usamos la palabra `const`:

```javascript
const myBirthday = '18.04.1982';

myBirthday = '01.01.2001'; // error, can't reassign the constant!
```
!!! note "Mayúsculas"
    Usamos mayúsculas y `_` con constantes cuyo valor asignamos de antemano (_hard-coded_); si es un valor que se calculará durante la ejecución, y no volverá a cambiar después, se nombra como una variable normal

```javascript
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";

// ...when we need to pick a color
let color = COLOR_ORANGE;
alert(color); // #FF7F00

const pageLoadTime = /* time taken by a webpage to load */;
```
