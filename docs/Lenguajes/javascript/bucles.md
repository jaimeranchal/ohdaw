# Bucles

En líneas generales es igual que otros lenguajes, particularmente, java:

- Bucles _while_: la condición se comprueba **antes** de cada iteración
- Bucles _do...while_: la condición se comprueba **después** de una primera ejecución
- Bucles _for_: la condición se comprueba antes de cada iteración; permite más ajustes.

Existen unas directivas, `break` y `continue`, que permiten interrumpir y retomar la ejecución de un bucle, ya sea en un punto determinado o cuando se cumplan unas condiciones, gracias al uso de marcas (_label_). Este último es el único recurso para exacar de un bucle anidado.

!!! Warning "Uso de break y continue"
    En general, usar un break para salir de un bucle se considera un fallo en el planteamiento de nuestro algoritmo; no debería ser necesario.

## Bucles _while_

=== "Sintaxis"

    ```javascript
    while (condicion) {
        // código
    }
    ```

=== "Cuenta 0-2"

    ```javascript
    let i = 0;
    while (i < 3) {
        alert(i); // 0, 1, 2
        i++;
    }
    ```

=== "Cuenta regresiva"

    ```javascript
    let i = 3;
    while (i != 0){
        alert(i);
        i--;
    }
    // Abreviando: while ( i != 0) es lo mismo que while (i)
    while (i) { // cuando i = 0, se vuelve "falso"
      alert(i);
      i--;
    }

    // no hacen falta llaves si solo hay una sentencia en el bucle
    let i = 3;
    while (i) alert(i--);
    ```

=== "Suma números"

    ```javascript
    /* Solicita números por ventana:
     * - Si se introduce entre 1 y 20, los suma
     * - Si se sale del rango, error (sigue pidiendo)
     * - Finaliza si el número es 0
     * - Imprime la suma total
    */
    numero = prompt("Entrada de números [1-20]");
    var suma;
    // Delimita el rango primero
    while (numero < 0 || numero > 20) {
        numero = prompt("Error\nVuelva a introducir un número");
    }
    // Si no es 0, suma
    while (numero != 0) {
        suma += parseInt(numero); 
        
        numero = prompt("Entrada de números [1-20]");
        while (numero < 0 || numero > 20) {
            numero = prompt("Error\nVuelva a introducir un número");
        }
    }
    // imprime la suma total
    console.log("La suma es " + `suma`);
    ```


## Bucles _do...while_

=== "Sintaxis"

    ```javascript
    do {
        // código
    } while (condicion);
    ```

=== "Cuenta 0-2"

    ```javascript
    let i = 0;
    do {
      alert( i );
      i++;
    } while (i < 3);
    ```

=== "Pide número (&gt;100)"

    ```javascript
    let num;

    do {
        num = prompt("Escribe un número mayor que 100", 0);
    } while (num <= 100 && num)
    ```
    
    !!! tip "Cuidado con los nulos"
        Si _num_ es nulo la condición `num <= 100` se **cumple**; la segunda condición del _while_ se asegura de que el bucle se detenga si el usuario pulsa CANCELAR.

## Bucles _for_

=== "Sintaxis"

    ```javascript
    for (contador; condición; paso) {
        // código
    }
    ```

=== "Cuenta 0-2"

    ```javascript
    for (let i=0; i < 3; i++) {
        alert(i);
    }
    ```

    !!! note "Omisión de parámetros"
        Se pueden omitir algunas condiciones, o incluso todas, aunque siempre hay que **conservar los `;` dentro del paréntesis)

        ```javascript
        let i = 0; // variable declara fuera del bucle

        let (; i < 3; i++) {
            alert(i);
        }

        // Aumento en el cuerpo
        for (; i < 3;){
            alert(i++);
        }

        // Bucle infinito
        for (;;){ ... }
        ```
