# Condicionales
Cuando necesitamos ejecutar diferentes acciones dependiendo de que se cumpla o no una condición, podemos usar varios recursos: 

- la sentencia `if`
- el operador condicional `?`

## Bloque _if_

La sintaxis es igual que en **java**:

=== "Sintaxis"

    ```javascript
    if (condicion) {
        // código
    } else if {
        // código
    } else {
        // código
    }
    ```

=== "Ejemplo 1"
    
    ```javascript
    // El segundo argumento de prompt es el valor por defecto en caso de no introducir nada
    let diaSemana = prompt("¿Qué día es hoy?", "");

    if (diaSemana === "domingo") {
        console.log("Hoy es festivo");
    // Sin {} se considera un bloque de una única instrucción
    } else console.log("Como no es domingo, toca currar. ¡Sorry!");
    ```

=== "Ejemplo 2"

    ```javascript
    let edad1, edad2;

    // Convertimos a entero la cadena introducida
    edad1 = parseInt(prompt("Introduce la edad de Ana: ", ""));
    edad2 = parseInt(prompt("Introduce la edad de Luis: ", ""));

    if (edad1 > edad2) {
        console.log("Ana es mayor que Luis");
    } else {
        if (edad1 < edad2) {
            console.log("Ana es menor que Luis");
        } else {
            console.log("Ana y Luis son de la misma edad");
        }
    }

    console.log("Ana tiene " + edad1 + "años y Luis " + edad2);
    ```

## Operador condicional `?`

El signo de interrogación es una alternativa más simple para expresar la misma lógica que un `if`:

=== "Versión con if"

    ```javascript
    let accesoPermitido;
    let edad = prompt("¿Qué edad tienes?", "");

    if (edad > 18) {
        accesoPermitido = true;
    } else {
        accesoPermitido = false;
    }

    alert(accesoPermitido);
    ```

=== "con `?`"

    ```javascript
    let edad = prompt("¿Qué edad tienes?", "");
    let accesoPermitido = (edad > 18) ? true : false;
    // Más simple aún: una comparación ya devuelve boolean
    // let accesoPermitido = edad > 18
    ```

!!! info "Operador ternario"
    El operador `?` es el único con **tres operandos** en JavaScript

!!! note "Uso no tradicional de ?"
    A veces podemos encontrarnos `?` en lugar de `if`; no se recomienda porque **dificulta la legibilidad**:

    === "Versión con if"

        ```javascript
        let company = prompt('Which company created JavaScript?', '');

        if (company == 'Netscape') {
          alert('Right!');
        } else {
          alert('Wrong.');
        }
        ```

    === "Versión con ?"

        ```javascript
        let company = prompt('Which company created JavaScript?', '');

        (company == 'Netscape') ? alert('Right!') : alert('Wrong.');
        ```
    
