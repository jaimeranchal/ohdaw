# Objetos nativos de JavaScript

No dependen del navegador; es decir, son **siempre accesibles**. Son cinco:

- _Date_
- _Math_
- _Number_
- _Boolean_
- _String_

## Objeto _Date_
Se utiliza para trabajar con fechas y horas.

```javascript
// Ejemplo de instanciación
var d = new Date();
var d = new Date(miliseconds);
var d = new Date("date string");
var d = new Date(year, month, day, hours, minutes, seconds, miliseconds);
// Nota: los meses son un array y empiezan en 0 (Enero = 0)
```

| Propiedad     | Descripción                                    |
|---------------|------------------------------------------------|
| `constructor` | Devuelve la función que creó el objeto         |
| `prototype`   | Permite añadir propiedades y métodos al objeto |


| Método                | Descripción                                              |
|-----------------------|----------------------------------------------------------|
| `getFullYear()`       | Get the year as a four digit number (yyyy)               |
| `getMonth()`          | Get the month as a number (0-11)                         |
| `getDate()`           | Get the day as a number (1-31)                           |
| `getHours()`          | Get the hour (0-23)                                      |
| `getMinutes()`        | Get the minute (0-59)                                    |
| `getSeconds()`        | Get the second (0-59)                                    |
| `getMilliseconds()`   | Get the millisecond (0-999)                              |
| `getTime()`           | Get the time (milliseconds since January 1, 1970)        |
| `getTimezoneOffset()` | Get the different between local time and GMT, as minutes |
| `getDay()`            | Get the weekday as a number (0-6)                        |
| `Date.now()`          | Get the time. ECMAScript 5.                              |

## Objeto _Math_

Permite realizar operaciones matemáticas. **No tiene constructor**.

```javascript
//Ejemplo
let numPi = Match.PI; //devuelve número PI
let raizC = Math.sqrt(16); // devuelve la raíz cuadrada de 16
```

| Constante      | Valor                               |
|----------------|-------------------------------------|
| `Math.E`       | returns Euler's number              |
| `Math.PI`      | returns PI                          |
| `Math.SQRT2`   | returns the square root of 2        |
| `Math.SQRT1_2` | returns the square root of 1/2      |
| `Math.LN2`     | returns the natural logarithm of 2  |
| `Math.LN10`    | returns the natural logarithm of 10 |
| `Math.LOG2E`   | returns base 2 logarithm of E       |
| `Math.LOG10E`  | returns base 10 logarithm of E      |


| Método                 | Valor                                                                         |
|------------------------|-------------------------------------------------------------------------------|
| `abs(x)`               | Returns the absolute value of x                                               |
| `acos(x)`              | Returns the arccosine of x, in radians                                        |
| `acosh(x)`             | Returns the hyperbolic arccosine of x                                         |
| `asin(x)`              | Returns the arcsine of x, in radians                                          |
| `asinh(x)`             | Returns the hyperbolic arcsine of x                                           |
| `atan(x)`              | Returns the arctangent of x as a numeric value between -PI/2 and PI/2 radians |
| `atan2(y, x)`          | Returns the arctangent of the quotient of its arguments                       |
| `atanh(x)`             | Returns the hyperbolic arctangent of x                                        |
| `cbrt(x)`              | Returns the cubic root of x                                                   |
| `ceil(x)`              | Returns x, rounded upwards to the nearest integer                             |
| `cos(x)`               | Returns the cosine of x (x is in radians)                                     |
| `cosh(x)`              | Returns the hyperbolic cosine of x                                            |
| `exp(x)`               | Returns the value of Ex                                                       |
| `floor(x)`             | Returns x, rounded downwards to the nearest integer                           |
| `log(x)`               | Returns the natural logarithm (base E) of x                                   |
| `max(x, y, z, ..., n)` | Returns the number with the highest value                                     |
| `min(x, y, z, ..., n)` | Returns the number with the lowest value                                      |
| `pow(x, y)`            | Returns the value of x to the power of y                                      |
| `random()`             | Returns a random number between 0 and 1                                       |
| `round(x)`             | Rounds x to the nearest integer                                               |
| `sin(x)`               | Returns the sine of x (x is in radians)                                       |
| `sinh(x)`              | Returns the hyperbolic sine of x                                              |
| `sqrt(x)`              | Returns the square root of x                                                  |
| `tan(x)`               | Returns the tangent of an angle                                               |
| `tanh(x)`              | Returns the hyperbolic tangent of a number                                    |
| `trunc(x)`             | Returns the integer part of a number (x)                                      |

## Objeto _Number_
No se suele instanciar sino que corre _de fondo_ cuando **asignamos valores numéricos** a una variable:

```javascript
let num = new Number(23);
// es lo mismo que:
let num2 = 23;
```

!!! note "Valores no numéricos"
    Si se le pasa un parámetro que no se puede convertir devuelve _NaN_:

    ```javascript
    let num = new Number("a23"); //NaN
    ```

| Propiedad           | Descripción                              |
|---------------------|------------------------------------------|
| `constructor`       | Función que creó el objeto               |
| `MAX_VALUE`         | Número más alto disponible en JavaScript |
| `MIN_VALUE`         | Número más bajo disponible en JavaScript |
| `NEGATIVE_INFINITY` | Infinito negativo, en caso de _overflow_ |
| `POSITIVE_INFINITY` | Infinito positivo, en caso de _overflow_ |
| `prototype`         | Permite añadir propiedades y métodos     |

| Método                        | Descripción                                        |
|-------------------------------|----------------------------------------------------|
| `toExponential(x)`            | Convierte el número a notación exponencial         |
| `toFixed(x)`                  | Formatea el número con _x_ decimales tras el punto |
| `toPrecision(x)`              | Formatea a **longitud _x_**                        |
| `toString([notation:2|8|16])` | Convierte un número en cadena                      |
| `valueOf()`                   | Devuelve el valor primitivo                        |

## Objeto _Boolean_
Se usa para **convertir** un valor no booleano en booleano:

```javascript
let bool = new Boolean(1);
let semaforo = true;
console.log(bool.toString()); // true
console.log(semaforo.toString()); // true
```

| Propiedad           | Descripción                              |
|---------------------|------------------------------------------|
| `constructor`       | Función que creó el objeto               |
| `prototype`         | Permite añadir propiedades y métodos     |

| Método       | Descripción                     |
|--------------|---------------------------------|
| `toString()` | Convierte un booleano en cadena |
| `valueOf()`  | Devuelve el valor primitivo     |

## Objeto _String_

Permite crear y manipular **cadenas**.

### Instanciación

```javascript
let texto = new String("JavaScript");
let texto = "JavaScript";
// comillas dobles o simples
let texto = '<input type="checkbox" name="coche" />Audi A6';
let texto = "<input type='checkbox' name='coche' />Audi A6";
```

### Concatenar cadenas

```javascript
// Con el operador +=
let texto="";
texto += "JavaScript";
texto += " <b>, lenguaje interpretado</b>";

// Con variables
let nombreEquipo = prompt("Introduce el nombre de tu equipo");
let mensaje = "El" + nombreEquipo + " ha sido el campeón de la liga";
```

| Propiedad | Descripción           |
|-----------|-----------------------|
| `length`  | Longitud de la cadena |

| Método                | Descripción                                                                                                                        |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------|
| `charAt()`            | Returns the character at the specified index (position)                                                                            |
| `charCodeAt()`        | Returns the Unicode of the character at the specified index                                                                        |
| `concat()`            | Joins two or more strings, and returns a new joined strings                                                                        |
| `endsWith()`          | Checks whether a string ends with specified string/characters                                                                      |
| `fromCharCode()`      | Converts Unicode values to characters                                                                                              |
| `includes()`          | Checks whether a string contains the specified string/characters                                                                   |
| `indexOf()`           | Returns the position of the first found occurrence of a specified value in a string                                                |
| `lastIndexOf()`       | Returns the position of the last found occurrence of a specified value in a string                                                 |
| `localeCompare()`     | Compares two strings in the current locale                                                                                         |
| `match()`             | Searches a string for a match against a regular expression, and returns the matches                                                |
| `repeat()`            | Returns a new string with a specified number of copies of an existing string                                                       |
| `replace()`           | Searches a string for a specified value, or a regular expression, and returns a new string where the specified values are replaced |
| `search()`            | Searches a string for a specified value, or regular expression, and returns the position of the match                              |
| `slice()`             | Extracts a part of a string and returns a new string                                                                               |
| `split()`             | Splits a string into an array of substrings                                                                                        |
| `startsWith()`        | Checks whether a string begins with specified characters                                                                           |
| `substr()`            | Extracts the characters from a string, beginning at a specified start position, and through the specified number of character      |
| `substring()`         | Extracts the characters from a string, between two specified indices                                                               |
| `toLocaleLowerCase()` | Converts a string to lowercase letters, according to the host's locale                                                             |
| `toLocaleUpperCase()` | Converts a string to uppercase letters, according to the host's locale                                                             |
| `toLowerCase()`       | Converts a string to lowercase letters                                                                                             |
| `toString()`          | Returns the value of a String object                                                                                               |
| `toUpperCase()`       | Converts a string to uppercase letters                                                                                             |
| `trim()`              | Removes whitespace from both ends of a string                                                                                      |
| `valueOf()`           | Returns the primitive value of a String object                                                                                     |
