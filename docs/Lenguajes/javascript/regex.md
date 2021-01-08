# Expresiones Regulares en JS

Una expresión regular en un _objeto_ que describe un **patrón de caracteres**. Se compone de:

1. Una _cadena_ de caracteres, números, corchetes, cuantificadores y metacaracteres _entre_ `/`.
2. Uno o varios _modificadores_

Suelen usarse para:

- **Validar** entradas de texto.
- **Buscar** y **reemplazar**.

=== "Sintaxis"
    ```javascript
    // /patrón/modificadores
    let patron = /w3schools/i;
    ```

=== "Propiedades"
    | nombre        | Descripción                                                              |
    |---------------|--------------------------------------------------------------------------|
    | `constructor` | Devuelve la función de creó el prototipo del objeto Regex                |
    | `global`      | Comprueba si se ha usado el modificador `g`                              |
    | `ignoreCase`  | Comprueba si se ha usado el modificador `i`                              |
    | `lastIndex`   | Especifica el índice a partir del cual empezar la siguiente coincidencia |
    | `multiline`   | Comprueba si se ha usado el modificador `m`                              |
    | `source`      | Devuelve el texto del patrón Regex                                       |
=== "Métodos"
    | nombre       | Descripción                                                                          |
    |--------------|--------------------------------------------------------------------------------------|
    | `compile()`  | **Obsolete en la versión 1.5** Compila una expresión regular                         |
    | `exec()`     | Comprueba que haya coindicencias en una cadena. Devuelve la **primera coincidencia** |
    | `test()`     | Comprueba que haya coindicencias en una cadena. Devuelve _true_ o _false_            |
    | `toString()` | Devuelve una cadena con la expresión regular                                         |

## Sintaxis

### Modificadores

| Símbolo | Efecto                                                                       |
|---------|------------------------------------------------------------------------------|
| `g`     | Búsqueda global: devuelve **todas** las coindicencias en lugar de la primera |
| `i`     | Búsqueda sin distinguir entre mayúsculas y minúsculas                        |
| `m`     | Búsqueda de coindicencias que ocupen **más de una línea**                    |

### Corchetes y paréntesis

| Expresión | Resultado                                                  |
|-----------|------------------------------------------------------------|
| `[abc]`   | Cualquier caracter incluido entre los corchetes            |
| `[^abc]`  | Cualquier caracter _excepto_ los incluidos entre corchetes |
| `[0-9]`   | Cualquier caracter dentro del **rango**                    |
| `[^0-9]`  | Cualquier caracter _fuera_ del **rango**                   |
| `(x|y)`   | Alternativa: o _x_ o _y_                                   |

### Metacaracteres

| Símbolo  | Efecto                                                                          |
|----------|---------------------------------------------------------------------------------|
| `.`      | Encuentra un único caracter (cualquiera, excepto salto de línea o fin de línea) |
| `\w`     | Encuentra un carácter de palabra                                                |
| `\W`     | Encuentra un carácter NO de palabra                                             |
| `\d`     | Encuentra un dígito                                                             |
| `\D`     | Encuentra un carácter que NO sea un dígito                                      |
| `\s`     | Encuentra un espacio en blanco                                                  |
| `\S`     | Encuentra un carácter que NO sea un espacio en blanco                           |
| `\b`     | Encuentra un carácter al principio o final de una palabra                       |
| `\B`     | Encuentra un carácter que NO esté al principio o final de una palabra           |
| `\O`     | Encuentra un carácter nulo                                                      |
| `\n`     | Encuentra un salto de línea                                                     |
| `\f`     | Encuentra un caracter de _form feed_                                            |
| `\r`     | Encuentra un caracter de salto de carro                                         |
| `\t`     | Encuentra un caracter de tabulador                                              |
| `\v`     | Encuentra un caracter de tabulador vertical                                     |
| `\xxx`   | Encuentra un caracter (código octal)                                            |
| `\xdd`   | Encuentra un caracter (código hexadecimal _dd_)                                 |
| `\udddd` | Encuentra un caracter Unicode (código hexadecimal _dddd_)                       |

### Cuantificadores

| Símbolo  | Efecto                                                             |
|----------|--------------------------------------------------------------------|
| `n+`     | Cualquier cadena que contenga al menos **una** _n_                 |
| `n*`     | Cualquier cadena que contenga **cero o más** _n_                   |
| `n?`     | Cualquier cadena que contenga **cero o una** _n_                   |
| `n{X}`   | Cualquier cadena que contenga **una secuencia de X** _n_           |
| `n{X,Y}` | Cualquier cadena que contenga **una secuencia de entre X e Y** _n_ |
| `n{X,}`  | Cualquier cadena que contenga **una secuencia de al menos X** _n_  |
| `n$`     | Cualquier cadena con _n_ al final                                  |
| `^n`     | Cualquier cadena con _n_ al comienzo                               |
| `?=n`    | Cualquier cadena seguida de la cadena _n_                          |
| `?!n`    | Cualquier cadena NO seguida de la cadena _n_                       |

## Ejemplos

=== "Test()"
    ```javascript
    /* Realiza una búsqueda global en una cadena */
    let cadena = "Hola mundo!";

    // busca "hola"
    let patron = /Hola/g;
    let resuultado = patron.test(cadena);

    // busca "mundo"
    let patron2 = /mundo/g;
    let resultado2 = patron2.test(cadena); //devuelve true
    ```
=== "Exec()"
    ```javascript
    /* Busca las instancias de "la" en una cadena y devuelve la primera */
    let cadena = "Las mejores cosas en la vida son gratis";
    let patron = /la/i;
    // let patron = new RegExp("la", "i"); // alternativa
    let resultado = patron.exec(cadena); //devuelve "La"
    ```
