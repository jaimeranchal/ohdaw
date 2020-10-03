# Sentencias _switch_

Un bloque de sentencia `switch` permite ejecutar código según el valor de una variable:

=== "Sintaxis"
    
    ```javascript
    switch (variable) {
        case 'valor1':
            // código
            [break;]
        case 'valor2':
            // código
            [break;]
        case 'valor3':
            // código
            [break;]
    }
    ```

=== "Ejemplo 1"

    !!! note "No break"
        Si no se encuentra un `break` la ejecución continúa hasta el siguiente `case`.

    ```javascript
    let a = 2 + 2;

    switch (a) {
      case 3:
        alert( "Demasiado pequeño" ); // No se imprime; no cumple la condición
      case 4:
        alert( "Justo!" ); // Se imprime
      case 5:
        alert( "Demasiado grande" ); // Se imprime SIN comprobar (falta break)
      default:
        alert( "No conozco ese valor" ); // se imprime también
    }
    ```

Hay que tener cuidado, porque `switch` hace una comparación **estricta** de los datos, por lo que es importante convertirlos si es necesario.

```javascript
let arg = prompt("Introduce un número");
switch (arg) {
  case '0': // Entre comillas porque prompt devuelve string
  case '1':
    alert( 'Uno o cero' );
    break;

  case '2':
    alert( 'Dos' );
    break;

  case 3: // 3, sin comillas, es un número
    alert( 'No se ejecuta!' );
    break;
  default:
    alert( 'Valor desconocido' );
}
```

!!! note "Switch condicional"
    Se puede usar un bloque `switch` como una alternativa al `if`, si en lugar de la variable indicamos `true`:

    ```javascript
    let dato = 10;
    switch (true) {
        case dato < 5:
            console.log("El dato es menor que 5");
            break;
        case dato == 5:
            console.log("El dato es igual a 5");
            break;
        case dato > 5 && dato <= 10:
            console.log("El dato está entre 6 y 10");
            break;
    }
    ```
    

