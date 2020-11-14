# Expresiones de función
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
        ```

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

