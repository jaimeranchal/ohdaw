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

## Interacción básica
## Funciones como variables
## Funciones _callback_
## Funciones sin nómbre
## _Arrow functions_
