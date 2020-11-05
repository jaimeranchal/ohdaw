# Funciones

A menudo es necesario realizar la misma acción en varias partes del script, como imprimir un mensaje cuando un usuario inicia o cierra sesión.

Las funciones son los _bloques de construcción_ de un programa. JavaScript incluye muchos "bloques" ya predefinidos, como `alert(mensaje)`, `prompt(mensaje, porDefecto)` y `confirm(pregunta)`. Pero también podemos crear nuestras **propias funciones**.

### Declaración

=== "Sintaxis"
    ```javascript
    function nombre(parametros) {
        // código
    }
    ```
=== "Ejemplo"
    ```javascript
    function showMessage(){
        alert('Hola a todos!');
    }
    ```
=== "Llamada"
    ```javascript
    // Imprime el mensaje dos veces
    showMessage();
    showMessage();
    ```

### Variables

=== "Locales"
    Sólo son visibles dentro de la función:
    ```javascript
    function showMessage() {
      let message = "Hello, I'm JavaScript!"; // local variable

      alert( message );
    }

    showMessage(); // Hello, I'm JavaScript!

    alert( message ); // <-- Error! The variable is local to the function
    ```
=== "Externas"
    ```javascript
    let userName = 'John';

    function showMessage() {
      let message = 'Hello, ' + userName;
      alert(message);
    }

    showMessage(); // Hello, John
    ```

    La función no solo tiene acceso a la variable externa, sino que la puede modificar también:

    ```javascript
    let userName = 'John';

    function showMessage() {
      userName = "Bob"; // (1) changed the outer variable

      let message = 'Hello, ' + userName;
      alert(message);
    }

    alert( userName ); // John before the function call

    showMessage();

    alert( userName ); // Bob, the value was modified by the function
    ```
=== "Sobreescritas"
    Las variables locales tienen prioridad sobre las externas.

    Si existe una variable local con el mismo nombre que una externa, la sobreescribe:

    ```javascript
    let userName = 'John';

    function showMessage() {
      let userName = "Bob"; // declare a local variable

      let message = 'Hello, ' + userName; // Bob
      alert(message);
    }

    // the function will create and use its own userName
    showMessage();

    alert( userName ); // John, unchanged, the function did not access the outer variable
    ```

!!! info "Globales"
    Las variables declaradas fuera de cualquier función se llaman _globales_.

    Las variables globales son visibles por cualquier función (a menos que estén sobreescritas por una o varias locales).

    Se considera una buena práctica **minimizar el uso de globales**. El código actual tiene pocas o ninguna; la mayoría de las variables residen dentro de sus funciones. No obstante, resultan útiles para almacenar información a nivel de todo el proyecto.

### Parámetros
Podemos pasar información arbitraria a una función usando parámetros (también llamados _argumentos_). En el siguiente ejemplo, la función tiene dos parámetros: `from` y `text`:

```javascript
function showMessage(from, text) { // arguments: from, text
  alert(from + ': ' + text);
}

showMessage('Ann', 'Hello!'); // Ann: Hello! (*)
showMessage('Ann', "What's up?"); // Ann: What's up? (**)
```

Cuando se invoca la función en las líneas `*` y `**`, los valores indicados se copian en dos variables locales llamadas `from` y `text`; luego son utilizadas por la función.

En el siguiente ejemplo la función cambia el valor de `from`, pero ese cambio no se refleja fuera de la función, porque las funciones siempre obtienen una _copia_ del valor:

```javascript
function showMessage(from, text) {

  from = '*' + from + '*'; // make "from" look nicer

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// the value of "from" is the same, the function modified a local copy
alert( from ); // Ann
```

### Valores por defecto
Si no se proporciona un parámetro, su valor es _indefinido_.

Por ejemplo, si invocamos la función `showMessage(from, text)` con solo un argumento:

```javascript
showMessage("Ann");
```

La función devolverá `"*Ann*: undefined"`, sin dar error. Como no hay valor para `text`, se asume que `text === undefined`.

Si queremos usar un valor "por defecto" podemos especificarlo con `=`:

```javascript
function showMessage(from, text = "no text given") {
  alert( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given
```

Se puede _llamar a otra función_ para indicar el valor por defecto, en lugar de asignarlo directamente:

```javascript
function showMessage(from, text = otraFuncion()) {
    // otraFuncion() solo se ejecuta si no se le da valor a 'text'
    // su resultado se convierte en el valor de 'text'
}
```

!!! info "Evaluación de los parámetros por defecto"
    En JavaScript un parámetro por defecto se evalúa **cada vez** que se llama a la función sin indicar el parámetro en cuestión.

    En el ejemplo anterior, `otraFuncion()` se procesaría cada vez que se llama a `showMessage()` sin indicar valor para `text`.

#### Valores por defecto alternativos

A veces es mejor definir los valores por defecto _dentro de la función_, durante su ejecución:

=== "Con if"
    ```javascript
    function showMessage(text) {
      if (text === undefined) {
        text = 'empty message';
      }

      alert(text);
    }

    showMessage(); // empty message
    ```
=== "Con OR"
    ```javascript
    // if text parameter is omitted or "" is passed, set it to 'empty'
    function showMessage(text) {
      text = text || 'empty';
      ...
    }
    ```
=== "Con `??`"
    ```javascript
    // if there's no "count" parameter, show "unknown"
    function showCount(count) {
      alert(count ?? "unknown");
    }

    showCount(0); // 0
    showCount(null); // unknown
    showCount(); // unknown
    ```

### Devolver un valor

```javascript
function suma(a, b) {
    return a + b;
}

let resultado = suma(1,2);
alert( resultado ); //3
```

La directiva `return` puede aparecer en cualquier lugar de la función. Cuando el programa llega a ese punto, la función se detiene y se devuelve el valor.

Puede haber más de una ocurrencia de `return` en una misma función:

```javascript
function compruebaEdad(edad) {
    if (edad >= 18) {
        return true;
    } else {
        return confirm("¿Tienes permiso de tus padres?");
    }
}

let edad = prompt("¿Qué edad tienes?", 18);

if (compruebaEdad(edad)) {
    alert("Permiso concedido");
} else {
    alert("Permiso denegado");
}
```

!!! info "Directiva `return` vacía"
    Una función con un `return` declarado pero sin valor termina inmediatamente y devuelve _undefined_:

    ```javascript
    function muestraPelicula(edad) {
        if (!muestraPelicula(edad)){
            return;
        }
        // lo siguiente NO se muestra si muestraPelicula() == false
        alert("Mostrando la película"); // (*)
        // ...
    }

    function noHaceNada() { /* vacío */ }
    function noHaceNada(){
        return;
    }
    // en ambos casos
    alert( noHaceNada() === undefined ); // true
    ```

!!! warning "Nunca añadas una nueva línea entre `return`y el valor"
    ```javascript
    return
      (una + expresion + larga + loquesea * f(a) + f(b))
    ```
    JavaScript siempre _presupone_ un punto y coma `;` después de un `return`. Lo anterior sería lo mismo que:

    ```javascript
    return;
      (una + expresion + larga + loquesea * f(a) + f(b))
    ```

    Si necesitamos usar más de una línea, el valor a devolver debe comenzar en la misma línea que el `return` o estar incluido entre paréntesis:

    ```javascript
    return (
        // expresión línea 1
        // expresión línea 2
        )
    ```

    De todas formas, si se requieren muchas operaciones en el return, lo suyo es convertirlo en **otra función**.

### Nombrar funciones
Las funciones representan **acciones**, de modo que lo normal es que sus nombres sean _verbos_. Deben ser breves, precisos y descriptivos.

Está muy extendido el uso de prefijos verbales que describen vagamente la acción o el tipo de acción a realizar. El uso debe estar consensuado por el equipo de programación:

- "get..." - devolver un valor
- "calc..." - calcular algo
- "create..." - crea algo
- "check..." - comprueba algo (condicionales o booleanos)

### Una función - una acción
Una función debe hacer exactamente lo que su nombre indica, y _nada más_.

Dos acciones independientes deben formar dos funciones separadas, aunque habitualmente se invoquen juntas (mediante una tercera función por ejemplo).

Tomemos por ejemplo una función que calcula números primos:

=== "Una única función"
    ```javascript
    function muestraPrimos(n) {
        // Declara una "etiqueta" (label)
        siguientePrimo: for (let i = 2; i < n; i++) {
            for (let j = 2; j < i; j++) {
                if (i % j == 0) continue siguientePrimo;
            }
            alert(i); //un número primo
        }
    }
    ```
=== "Funciones separadas"
    ```javascript
    function muestraPrimos(n) {
        for(let i = 2; i < n; i++){
            if (!esPrimo(i)) continue;
            alert(i); // Un número primo
        }
    }

    function esPrimo(n) {
        for(let i = 2; i < n; i++){
            if (n % i == 0) return false;
        }
        return true;
    }
    ```

### Resumen
La sintaxis para declarar una función es así:

```javascript
function nombre(parametros, delimitados, por, coma) {
    /* código */
}
```

- Los valores que se pasan como parámetros son _copiados como variables locales_.
- Una función puede acceder a _variables externas_, pero sólo desde dentro hacia fuera. El código fuera de la función no puede ver las variables locales.
- Una función puede _devolver_ un valor. Si no lo hace, su resultado es _undefined_.

Para hacer el código más limpio y fácil de entender, se recomienda usar variables locales y parámetros en lugar de variables externas.

Siempre es más fácil entender una función que obtiene parámetros, hace algo con ellos y devuelve un valor que otra que no obtiene parámetros pero modifica una variable externa como efecto secundario.

Nombres de funciones:

- Un nombre debe describir claramente lo que hace la función. Al verla, debemos saber o intuir inmediatamente lo que hace y devuelve.
- Una función es una acción, de modo que suelen ser verbos
- Hay muchos prefijos verbales habituales para funciones (en inglés): `create`, `show`, `get`, `check` etc.

## Expresiones de función
En JavaScript una función se considera un tipo especial de _valor_.

La sintaxis usada en el apartado anterior se denomina _declaración de función_; sin embargo, existe otra forma de declarar una función llamada _expresión de función_:

```javascript
let diHola = function() {
    alert("Hola");
};
```

Aquí la función se crea y se asigna a la variable explícitamente, como cualquier otro valor. No importa cómo se defina la función, solo es un valor almacenado en la variable `diHola`.

!!! tip "¿Cuándo usarlas?"
    En general **es mejor declarar las funciones** en lugar de asignarlas con expresiones.

!!! note "Mencionar una función VS ejecutar una función"
    Hay lenguajes donde cualquier mención de una función provoca su ejecución. En JavaScript eso no ocurre.

    Una función es un **valor** (el código que la compone), que se ejecuta solo cuando añadimos los paréntesis.

Es posible imprimir el código de la función:

```javascript
function diHola() {
    alert("Hola");
}

alert(diHola); //muestra el ćodigo de la función, no la ejecuta
```

O copiar una función en otra variable

```javascript
function sayHi() {   // (1) create
  alert( "Hello" );
}

let func = sayHi;    // (2) copy

// (3) run the copy (it works)!
func(); // Hello     
// this still works too (why wouldn't it)
sayHi(); // Hello    
```

!!! info "¿Por qué llevan un punto y coma al final?"
    ```javascript
    function diHola() {
        //...
    }

    let diHola = function() {
        //...
    };
    ```

    JavaScript no exije un punto y coma al final de un **bloque de código** (entre llaves `{}`), pero sí al final de cualquier **sentencia**. Al usar la sintaxis de declaración de variables con `let algo = x`, que _es_ una sentencia, debemos añadirlo.

### Funciones _callback_

Tomemos como ejemplo una función que acepta tres parámetros:

1. **pregunta**: el texto de la pregunta
2. **si**: una _función_ que se ejecuta si la respuesta es sí
3. **no**: una _función_ que se ejecuta si la respuesta es no.

La función debe mostrar la pregunta y llamar a `si()` o `no()` según sea la respuesta:

```javascript
function preguntar(pregunta, si, no) {
    if (confirm(pregunta)) si()
    else no();
}

function showOk() {
    alert( "Has dicho que sí." );
}

function showCancel() {
    alert( "Has cancelado la ejecución." );
}

// Uso
preguntar("¿Estás de acuerdo?", showOk, showCancel);
```

**Los parámetros `showOk` y `showCancel` se llaman funciones de rellamada (_callback functions_) o simplemente _callbacks_.**

La idea es pasar una función y esperar a que le "devuelvan la llamada" en otro momento si hace falta. En el ejemplo, `showOk` es el _callback_ para la respuesta "sí", y `showCancel`para un "no".

!!! info "Abreviar código con funciones _anónimas_"
    ```javascript
    function preguntar(pregunta, si, no) {
        if (confirm(pregunta)) si()
        else no();
    }

    preguntar(
        "¿Estás de acuerdo?",
        function() {alert("Has dicho que sí.");},
        function() {alert("Has cancelado la ejecución.");}
    );
    ```

    Las funciones dentro del `preguntar(...)` no tienen nombre y por eso se llaman _anónimas_; no son accesibles fuera de esa declaración.

### Expresiones de función VS Declaraciones de función

=== "Sintaxis":
    ```javascript
    // Declaración de función
    function suma(a,b) {
        return a + b;
    }

    // Expresión de función
    let suma = function(a,b){
        return a + b;
    };
    ```
=== "Cuándo se crean"
    - Las expresiones de función se crean **cuando la ejecución llega a ese punto**
    - Sólo están disponibles **a partir de ese momento**.
    - Sin embargo, una función declarada puede ser llamada **antes de su definición**.

=== "Visibilidad"
    En **modo estricto**, una declaración de función sólo es visible _dentro del bloque en el que se declara_.

    Si quisiéramos definir una función de forma diferente según una _condición_, sólo funcionaría bien con expresiones de función:

    === "Con declaración"
        ```javascript
        let age = prompt("What is your age?", 18);

        // conditionally declare a function
        if (age < 18) {

          function welcome() {
            alert("Hello!");
          }

        } else {

          function welcome() {
            alert("Greetings!");
          }

        }

    === "Con expresiones"
        // ...use it later
        welcome(); // Error: welcome is not defined
        ```

        ```javascript
        let age = prompt("What is your age?", 18);

        let welcome;

        if (age < 18) {

          welcome = function() {
            alert("Hello!");
          };

        } else {

          welcome = function() {
            alert("Greetings!");
          };

        }

        welcome(); // ok now
        ```

    === "Más breve aún"
        
        ```javascript
        let age = prompt("What is your age?", 18);

        let welcome = (age < 18) ?
          function() { alert("Hello!"); } :
          function() { alert("Greetings!"); };

        welcome(); // ok now
        ```

## Funciones Flecha
