# Tutorial de PHP

- Fuente: https://www.w3schools.com/php/php_intro.asp

## Introducción

!!! note
    El código php se ejecuta en el **lado del servidor**; lo único visible en el lado del cliente es el HTML resultante.

### ¿Qué es un archivo PHP?

- Tienen la extensión `.php`
- Puede contener código HTML, CSS y JavaScript, además de php.

## Instalación

### ¿Qué necesito?

Para usar PHP puedes hacer dos cosas: 

1. Encontrar un _host_ con soporte para PHP y MySQL
2. Instalar un servidor en tu ordenador, y _además_ instalar PHP y MySQL.

#### PHP en un Host Web o propio

Simplemente crea archivos con la extensión `.php` y colócalos en el directorio web del servidor

#### PHP en un editor / compilador online

Por ejemplo, el de [w3schools](https://www.w3schools.com/php/phptryit.asp?filename=tryphp_compiler). Prueba a escritir lo siguiente: 

```php
<?php
$txt = "PHP";
echo "Me encanta $txt!";
?>
```

## Sintaxis

- Un script PHP se **ejecuta en el servidor**, y el navegador muestra un **documento HTML**.
- Se puede **insertar** un script PHP en cualquier lugar, enmarcándolo con `<?php` y `?>`.

```php
<?php
// Tu código PHP
?>
```

```php
<!DOCTYPE html>
<html>
    <body>
        <h1>My first PHP page</h1>
            <?php
            echo "Hola Mundo!";
            ?>
    </body>
</html>
```

!!! note "Mayúsculas ¿o no?"
    PHP es **indiferente** al uso de minúsculas o mayúsculas.
    Los tres ejemplos siguientes son iguales:

    ```php
    <!DOCTYPE html>
    <html>
        <body>
            <?php
            ECHO "Hola mundo<br>";
            echo "Hola mundo<br>";
            EcHo "Hola mundo<br>";
        </body>
    </html>
    ```
 
!!! warning "Cuidado"
    Las variables son **sensibles a las mayúsculas**!

    ```php
    <!DOCTYPE html>
    <html>
    <body>

    <?php
    $color = "red";
    echo "My car is " . $color . "<br>";
    echo "My house is " . $COLOR . "<br>";
    echo "My boat is " . $coLOR . "<br>";
    ?>

    </body>
    </html>
    ```

## Comentarios

Hay tres formas de insertar comentarios:

1. De una sola línea: `//` y `#`
2. Más de una línea: `/* ... */`

```php
<?php
// Esto es un comentario
# Esto también
/*
    Esto son 
    varias líneas
    de comentario
*/
```

## Variables

En PHP las variables empiezan con el **signo del dólar** (`$`):

```php
<?php
$texto = "Bugs Bunny";
$x = 5;
$y = 10.5;
?>
```

!!! note "Reglas para nombrar variables"
    - Siempre empiezan con `$`
    - Debe **empezar** por `[a-z|_]`
    - **No puede** empezar por _número_
    - Sólo puede contener los siguientes caracteres: `[A-z][0-9]_`
    - Las variables en php son **sensibles a las mayúsculas**: `$edad` y `$EDAD` son variables _diferentes_

### Visibilidad

En php se pueden declarar variables **en cualquier lugar** del script.

Exiten **tres ámbitos de visibilidad**:

- **local**: visibles sólo _dentro de una función_
- **global**: visibles _fuera_ de una función
    - Dentro de una función precedidas de `global`
    - php guarda todas las variables globales en un _array_ llamado `$GLOBALS[indice]`. El `indice` es el nombre de la variable. Este array es accesible desde cualquier función y puede usarse para _actualizar directamente_ variables globales. 
- **estático**: normalmente php _borra_ las variables cuando se ejecuta la función. La palabra clave `static` la mantiene en memoria. 

```php
<?php
function myTest() {
  $x = 5; // ámbito local
  echo "<p>Variable x inside function is: $x</p>";
}
myTest();

// Usar $x fuera de la función dará un error
echo "<p>Variable x outside function is: $x</p>";
?>
```

```php
<?php
// Usando la palabra clave "global" 
$x = 5;
$y = 10;

function myTest() {
  global $x, $y;
  $y = $x + $y;
}

myTest();
echo $y; // resultado 15
?>
```

```php
<?php
// Usando el array $GLOBALS
$x = 5;
$y = 10;

function myTest() {
  $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
}

myTest();
echo $y; // resultado 15
?>
```

```php
<?php
// ejemplo de contador muy básico
function myTest() {
  static $x = 0;
  echo $x;
  $x++;
}

// para que funcione, el valor de $x debe guardarse tras cada ejecución
myTest();
myTest();
myTest();
?>
```

!!! info "Superglobales"
    La variable `$GLOBALS` forma parte de un grupo de variables predefinidas en PHP y que son siempre accesibles, de ahí su nombre _super_-globales. Son:

    - `$GLOBALS`
    - `$_SERVER`
    - `$_REQUEST`
    - `$_POST`
    - `$_GET`
    - `$_FILES`
    - `$_ENV`
    - `$_COOKIE`
    - `$_SESSION`

### Superglobales

Almacena todas las variables globales en un _array_.

```php
<?php
$x = 75;
$y = 25;
 
function suma() {
// Además de usar dos variables globales, se declara una nueva, 'z'
  $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}
 
suma();
echo $z;
?>
```

### Superglobales: `$_SERVER` 

Guarda información sobre las _cabeceras_, _rutas_, y _localización de scripts_.

```php
<?php
echo $_SERVER['PHP_SELF'];
echo "<br>";
echo $_SERVER['SERVER_NAME'];
echo "<br>";
echo $_SERVER['HTTP_HOST'];
echo "<br>";
echo $_SERVER['HTTP_REFERER'];
echo "<br>";
echo $_SERVER['HTTP_USER_AGENT'];
echo "<br>";
echo $_SERVER['SCRIPT_NAME'];
?>
```

| Valor del índice                  | Resultado                                                                                                          |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `$_SERVER['PHP_SELF']`            | Nombre del script que se está ejecutando                                                                           |
| `$_SERVER['GATEWAY_INTERFACE']`   | Versión del CGI usado por el servidor                                                                              |
| `$_SERVER['SERVER_ADDR']`         | IP del servidor                                                                                                    |
| `$_SERVER['SERVER_NAME']`         | Nombre del servidor (p.ej. www.w3schools.com)                                                                      |
| `$_SERVER['SERVER_SOFTWARE']`     | Cadena de identificación del servidor (p.ej Apache/2.2.24)                                                         |
| `$_SERVER['SERVER_PROTOCOL']`     | Protocolo del servidor y versión (p.ej. HTTP/1.1)                                                                  |
| `$_SERVER['REQUEST_METHOD']`      | Método de petición de datos (POST, p.ej.)                                                                          |
| `$_SERVER['REQUEST_TIME']`        | Fecha-hora de inicio de la petición (1377687496, p.ej)                                                             |
| `$_SERVER['QUERY_STRING']`        | Cadena de query (si se accede a la página con una query)                                                           |
| `$_SERVER['HTTP_ACCEPT']`         | Cabecera de aceptación de la petición actual                                                                       |
| `$_SERVER['HTTP_ACCEPT_CHARSET']` | Codificación de la petición actual (utf-8,ISO-8859-1...)                                                           |
| `$_SERVER['HTTP_HOST']`           | Returns the Host header from the current request                                                                   |
| `$_SERVER['HTTP_REFERER']`        | URL completa de la página actual (no todos los _user-agents_ lo soportan; NO USAR)                                 |
| `$_SERVER['HTTPS']`               | Si se usa protocolo seguro o no                                                                                    |
| `$_SERVER['REMOTE_ADDR']`         | Dirección IP desde donde accede el usuario                                                                         | | `$_SERVER['REMOTE_HOST']`         | Host desde el que accede el usuario                                                                                | | `$_SERVER['REMOTE_PORT']`         | Puerto en la máquina del usuario por el que se comunica con el servidor                                            | | `$_SERVER['SCRIPT_FILENAME']`     | Ruta absoluta del script ejecutándose                                                                              | | `$_SERVER['SERVER_ADMIN']`        | Valor de la directiva `SERVER_ADMIN` en la config. del servidor (o del host virtual) (alguien@w3schools.com, p.ej) | | `$_SERVER['SERVER_PORT']`         | Puerto en la máquina del servidor usado para que el servidor se comunique (80 p.e)                                 | | `$_SERVER['SERVER_SIGNATURE']`    | Versión del servidor y nombre del host virtual que se añaden a las páginas generadas                               |
| `$_SERVER['PATH_TRANSLATED']`     | Ruta basada en el sistema de archivos del script actual                                                            |
| `$_SERVER['SCRIPT_NAME']`         | Ruta del script actual                                                                                             |
| `$_SERVER['SCRIPT_URI']`          | URI de la página actual                                                                                            |

### Superglobales: `$_REQUEST` |

Reúne datos después de entregar un formulario HTML.

```php
<!-- 
    1. Creamos un formulario con un campo de entrada y un botón de "enviar" 
    2. Los datos se envían al archivo especificado en el atributo "action" 
        del elemento <form>: en este caso, el propio script (PHP_SELF)
    3. Usamos la variable superglobal para recuperar el valor introducido
-->
<html>
<body>

<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  // Recupera el valor introducido por su nombre
  $name = $_REQUEST['fname'];
  if (empty($name)) {
    echo "El nombre está vacío";
  } else {
    echo $name;
  }
}
?>

</body>
</html>
```

### Superglobales: `$_POST` 

Permite recibir datos de formularios después de enviar un formulario HTML con `method="post"`. La variable `$_POST`también se usa mucho para **pasar variables**.

```php
<!--
    1. Creamos un formulario con un campo de entrada y un botón de "enviar" 
    2. Los datos se envían al archivo especificado en el atributo "action" 
        del elemento <form>: en este caso, el propio script (PHP_SELF)
    3. Usamos la variable superglobal para recuperar el valor introducido
-->

<html>
<body>

<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  // Recupera el valor introducido por su nombre
  $name = $_POST['fname'];
  if (empty($name)) {
    echo "El nombre está vacío";
  } else {
    echo $name;
  }
}
?>

</body>
</html>
```

### Superglobales: `$_GET` 

Permite recibir datos de formularios después de enviar un formulario HTML con `method="get"`. La variable `$_GET` también puede recuperar datos enviados **en la URL**.

Si tenemos una página HTML con un enlace con los siguientes parámetros:

```html
<html>
    <body>
        <a href="test_get.php?subject=PHP&web=W3schools.com">Test $GET</a>
    </body>
</html>
```

Cuando un usuario hace click en el enlace `"Test $GET"`, los parámetros _subject_ y _web_ son enviados al archivo `"test_get.php"`; luego puedes acceder a sus valores con `$_GET`.

```php
<!-- contenido del script "test_get.php" -->
<html>
    <body>
        <?php
        echo "Study " . $_GET['subject'] . " at " . $_GET['web'];
        ?>
    </body>
</html>
```

### Constantes

Las constantes son un tipo de variables que **no se pueden modificar** una vez definidas.

- Empiezan por una **letra** o `_` (**sin** el signo de dólar `$`).
- Son **globales** por defecto.

Puedes crear una o varias a la vez (con un _array_) utilizando la función `define()`:

```
define(nombre, valor, ¿sensible a mayúsculas?)
```

```php
<?php
define("SALUDO", "Bienvenido a OhDaw.com");
// define("SALUDO", "Bienvenido a OhDaw.com", true); // sensible a mayúsculas
echo SALUDO;
?>

<!-- Ejemplo de array -->
<?php
define("coches", [
    "Alfa Romeo",
    "Seat",
    "Audi"
]);
echo coches[0];
?>
```

## Tipos de datos

PHP es un lenguaje **débilmente tipado**, lo que significa que asigna _automáticamente_ el tipo de dato a las variables.

Soporta los siguientes tipos:

- Cadenas ( _String_ )
- Enteros ( _Integer_ )
- Float (incluye lo que en Java y otros son _double_)
- Booleano
- Array
- Object
- NULL
- _Resource_

### Cadenas

```php
<?php
$x = "Hello world!";
$y = 'Hello world!';

echo $x;
echo "<br>";
echo $y;
?>
```

### Enteros

!!! info "Rango"
    Un tipo de dato entero es cualquier número entre -2,147483648 y -2,147483648.

Reglas:

- Debe tener _al menos_ un dígito
- No puede ser decimal
- Puede ser positivo _o_ negativo
- Se pueden especificar en:
    - Decimal (base 10)
    - Hexadecimal (base 16)
    - Octal
    - Binario

```php
<?php
$x = 5985;
// la siguiente función devuelve el tipo y valor de la variable
var_dump($x);
?>
```

### Arrays

Un _array_ o vector almacena varios valores en una única variable:

```php
<?php
$cars = array("Volvo", "Seat", "Toyota");
var_dump($cars);
?>
```

#### Arrays "asociativos"

El concepto de un array asociativos es un almacén de pares _clave-valor_, similar a los _TreeSet_ en Java.

Se declaran de dos formas:

```php
<?php
$edad = array("Pedro"=>"35", "Pablo"=>"37", "Juan"=>"43");

// o también
$edad["Pedro"] = "35";
$edad["Pablo"] = "37";
$edad["Juan"] = "43";
?>
```

Las "claves" (primer elemento) se pueden usar en un script para acceder al valor:

```php
<?php
$edad = array("Pedro"=>"35", "Pablo"=>"37", "Juan"=>"43");
echo "Pedro tiene " . $edad["Pedro"] . " años";
?>
```

!!! info "Complicando la cosa"
    También existen [Arrays multidimensionales](https://www.w3schools.com/php/php_arrays_multidimensional.asp)



### Objetos

Un objeto almacena datos e información sobre cómo procesarlos. En PHP deben ser declarados **explícitamente**:

```php
<?php
class Coche {
    function Coche() {
        $this->modelo = "Ibiza";
    }
}

// Crea el objeto
$ibiza = new Coche();

// Muestra las propiedades del objeto
echo $ibiza->modelo;
?>
```

### Nulos

Una variable de tipo NULL no tiene _ningún valor_.

!!! note "Usos del tipo `NULL`"
    - Si se declara una variable sin asignarle un valor, será de tipo NULL.
    - Se puede _vaciar_ una variable asigándole el valor `NULL`.

```php
<?php
$x = "Hola qué tal?";
$x = null;
var_dump($x);
?>
```

### Recursos

Las variables de tipo _Resource_ (Recurso) no son tipos de datos sino **referencias** a funciones o recursos externos a PHP.

!!! note "Consultas"
    Un uso habitual de este "tipo" son las consultas a bases de datos.

## Operadores

Los operadores en PHP son similares a otros lenguajes de programación:

### Aritméticos

| Operador | Nombre         | Ejemplo          | Resultado                      |
|----------|----------------|------------------|--------------------------------|
| `+`      | Suma           | `#!php $x + $y`  | Suma de x e y                  |
| `-`      | Resta          | `#!php $x - $y`  | Resta de x e y                 |
| `*`      | Multiplicación | `#!php $x * $y`  | Multiplicación de x e y        |
| `/`      | División       | `#!php $x / $y`  | División de x e y              |
| `%`      | Módulo         | `#!php $x % $y`  | Módulo de la división de x e y |
| `**`     | Potencia       | `#!php $x ** $y` | Potencia de x elevado a y      |

### Asignación

| Asignación | Equivale a  | Descripción                           |
|------------|-------------|---------------------------------------|
| `x = y`    | `x = y`     | Asigna el valor `y` a la variable `x` |
| `x += y`   | `x = x + y` | Adición                               |
| `x -= y`   | `x = x - y` | Sustracción                           |
| `x *= y`   | `x = x * y` | Multiplicación                        |
| `x /= y`   | `x = x / y` | División                              |
| `x %= y`   | `x = x % y` | Módulo                                |

### Comparación

| Operador | Nombre            | Ejemplo     | Resultado                                                                                                                                  |
|----------|-------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `==`     | Igual             | `$x == $y`  | Devuelve _true_ si `$x` es igual a `$y`                                                                                                    |
| `===`    | Identidad         | `$x === $y` | Devuelve _true_ si `$x` es igual a `$y`, y ambos son del **mismo tipo**                                                                    |
| `!=`     | Desigualdad       | `$x != $y`  | Devuelve _true_ si `$x` no es igual a `$y`                                                                                                 |
| `<>`     | Desigualdad       | `$x != $y`  | Devuelve _true_ si `$x` no es igual a `$y`                                                                                                 |
| `!==`    | No idéntico       | `$x !== $y` | Devuelve _true_ si `$x` no es igual a `$y`, o **no son del mismo tipo**                                                                    |
| `>`      | Mayor que         | `$x > $y`   | Devuelve _true_ si `$x` es mayor que `$y`                                                                                                  |
| `<`      | Menor que         | `$x < $y`   | Devuelve _true_ si `$x` es menor que `$y`                                                                                                  |
| `>=`     | Mayor o igual que | `$x >= $y`  | Devuelve _true_ si `$x` es mayor o igual que `$y`                                                                                          |
| `<=`     | Menor o igual que | `$x >= $y`  | Devuelve _true_ si `$x` es menor o igual que `$y`                                                                                          |
| `<=>`    | Nave Espacial     | `$x <=> $y` | Devuelve un _entero_ menor que, igual o mayor que `0`, si `$x` es menor que, igual o mayor que `$y`.<br>Introducido en PHP7 |

### Incremento / Decremento

| Operador | Nombre          | Descripción                                   |
|----------|-----------------|-----------------------------------------------|
| `++$x`   | Pre-incremento  | Aumenta el valor de `$x` en 1 y lo devuelve   |
| `$x++`   | Post-incremento | Devuelve `$x` y luego aumenta su valor        |
| `--$x`   | Pre-decremento  | Disminuye el valor de `$x` en 1 y lo devuelve |
| `$x--`   | Post-decremento | Devuelve `$x` y luego disminuye su valor      |

### Lógicos

| Operador      | Nombre | Ejemplo                   | Resultado                                   |
|---------------|--------|---------------------------|---------------------------------------------|
| `and`<br>`&&` | Y      | `$x and $y`<br>`&x && $y` | _True_ si los dos son _true_                |
| `or`<br>`     |        | `                         | O                                           | `$x or $y`<br>`$x |  | $y` | _True_ si uno de los dos es _true_ |
| `xor`         | XOR    | `$x xor $y`               | _True_ si los dos son _true_, pero no ambos |
| `!`           | No     | `!$x`                     | _True_ si `$x` no es _true_                 |

### De Cadena

| Operador | Nombre                     | Ejemplo          | Resultado               |
|----------|----------------------------|------------------|-------------------------|
| `.`      | Concatenación              | `$txt1 . $txt2`  | Une las dos cadenas     |
| `.=`     | Concatenación y asignación | `$txt1 .= $txt2` | Añade `$txt2` a `$txt1` |

### De Arrays

| Operador | Nombre       | Ejemplo     | Resultado                                                                         |
|----------|--------------|-------------|-----------------------------------------------------------------------------------|
| `+`      | Unión        | `$x + $y`   | Une los contenidos de ambos arrays                                                |
| `==`     | Igualdad     | `$x == $y`  | _True_ si `$x` e `$y` tienen los mismos pares de clave-valor                      |
| `===`    | Identidad    | `$x === $y` | _True_ si `$x` e `$y` tienen los mismos pares de clave-valor y son del mismo tipo |
| `!=`     | Desigualdad  | `$x != $y`  | _True_ si `$x` no es igual a `$y`                                                 |
| `<>`     | Desigualdad  | `$x <> $y`  | _True_ si `$x` no es igual a `$y`                                                 |
| `!==`    | No-identidad | `$x != $y`  | _True_ si `$x` no es idéntico a `$y`                                              |

### Asignación condicional

| Operador | Nombre          | Ejemplo                            | Resultado                                                      |
|----------|-----------------|------------------------------------|----------------------------------------------------------------|
| `?:`     | Ternario        | `#!php $x = expr1 ? expr2 : expr3` | `$x` es _expr2_ si _expr1_ es true; si no, es _expr3_          |
| `??`     | Null coalescing | `#!php $x = expr1 ?? expr2`        | `$x` es _expr1_ si _expr1_ existe y no es nulo; si no, _expr2_ |

## Condicionales

Hay cuatro variantes de sentencias condicionales en PHP:

- `if`: ejecuta algo si una condición es _verdadera_
- `if ... else`: ejecuta una cosa si la condición es _verdadera_ y otra si no.
- `if ... elseif ... else`: ejecuta diferentes cosas según más de una condición.
- `switch`: selecciona uno o varios bloques de código para que se ejecuten según la condición.

### if

```php
<?php
$t = date("H");

// If simple
if ($t < "20") {
  echo "Have a good day!";
}

//if else
if ($t < "20") {
  echo "Have a good day!";
} else {
  echo "Have a good night!";
}

// if elseif
if ($t < "10") {
  echo "Have a good morning!";
} elseif ($t < "20") {
  echo "Have a good day!";
} else {
  echo "Have a good night!";
}
?>
```

### switch

Sintaxis: 

```
switch (n) {
  case label1:
    code to be executed if n=label1;
    break;
  case label2:
    code to be executed if n=label2;
    break;
  case label3:
    code to be executed if n=label3;
    break;
    ...
  default:
    code to be executed if n is different from all labels;
}
```

```php
<?php
$favcolor = "red";

switch ($favcolor) {
  case "red":
    echo "Your favorite color is red!";
    break;
  case "blue":
    echo "Your favorite color is blue!";
    break;
  case "green":
    echo "Your favorite color is green!";
    break;
  default:
    echo "Your favorite color is neither red, blue, nor green!";
}
?>
```

## Bucles

PHP soporta los siguientes tipos de bucles:

- `while`: ejecuta un bloque mientras una condición sea _verdadera_.
- `do...while`: ejecuta un bloque, y continúa mientras la condición siga siendo _verdadera_.
- `for`: especifica el número de veces que se recorre el bucle
- `foreach`: ejecuta un bloque de código un número de veces por cada elemento de un _array_

### While

```php
<?php
$x = 1;

while($x <= 5) {
  echo "The number is: $x <br>";
  $x++;
}
?>
```

### Dowhile

```php
<?php
$x = 1;

do {
  echo "The number is: $x <br>";
  $x++;
} while ($x <= 5);
?>
```

!!! note "Lo que importa es cuándo"
    En un bucle `do...while`, la condición de repetición se comprueba **después** de ejecutarse el código una primera vez, incluso aunque la condición fuera falsa.

### For

Sintaxis:

```
for (contador inicial; condición; incremento) {
    código;
}
```

```php
<?php
for ($x = 0; $x <= 10; $x++) {
  echo "The number is: $x <br>";
}
?>
```

### Foreach

Sintaxis:

```
foreach ($array as $valor) {
    código;
}
```

```php
<?php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $value) {
  echo "$value <br>";
}

// Este ejemplo imprime los pares clave-valor:
<?php
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

foreach($age as $x => $val) {
  echo "$x = $val<br>";
}
?>
?>
```

### Sentencias _break_ y _continue_

Se pueden usar para interrumpir un bucle "a lo bruto", y continuar con él más adelante:

```php
<?php
// sale del bucle si el valor es 4
for ($x = 0; $x < 10; $x++) {
  if ($x == 4) {
    break;
  }
  echo "The number is: $x <br>";
}

// Para el bucle en 4 (no lo imprime), pero CONTINÚA con los siguientes
for ($x = 0; $x < 10; $x++) {
  if ($x == 4) {
    continue;
  }
  echo "The number is: $x <br>";
}

// En un bucle while
while($x < 10) {
  if ($x == 4) {
    break; // o continue;
  }
  echo "The number is: $x <br>";
  $x++;
}
?>
```

## Funciones

Las funciones son el auténtico _meollo_ de PHP, y los que nos permite sacarle todo el partido. Pero eso no significa que seamos nosotros los que nos comamos la cabeza para resolver los problemas (no siempre, claro); PHP tiene un montón de [funciones incluidas](https://www.w3schools.com/php/php_ref_overview.asp) que podemos usar directamente.

### Escribir funciones

Pero cada problema es de su padre y de su madre, tarde o temprano tendremos que escribir las nuestras propias; la sintaxis es así:

```
function nombreFuncion() {
    código;
}
```

!!! note "Nombres de funciones"
    Deben empezar con una **letra** o un `_`. Los nombres de funciones **no** son sensibles a mayúsculas.

```php
<?php
function escribeMsj() {
    echo "Hey cómo andas?";
}

escribeMsj(); // invoca la función
?>
```

### Argumentos

```php
<?php
function familyName($fname) {
  echo "$fname Refsnes.<br>";
}

familyName("Jani");
familyName("Hege");
familyName("Stale");
familyName("Kai Jim");
familyName("Borge");

function familyName($fname, $year) {
  echo "$fname Refsnes. Born in $year <br>";
}

familyName("Hege", "1975");
familyName("Stale", "1978");
familyName("Kai Jim", "1983");
?>
```

!!! note "Valor por defecto"
    Si queremos especificar un valor por defecto para los argumentos (en caso de invocar la función sin parámetros), usamos:

    ```php
    <?php declare(strict_types=1); // strict requirement
    function setHeight(int $minheight = 50) {
      echo "The height is : $minheight <br>";
    }

    setHeight(350);
    setHeight(); // will use the default value of 50
    setHeight(135);
    setHeight(80);
    ?>
    ```

### Modo estricto

PHP es un lenguaje débilmente tipado, por eso no es necesario especificar el tipo de dato de los argumentos. ¿Qué pasa si necesitamos asegurarnos de que se usan los tipos apropiados?

En estos casos debemos usar la sentencia `#!php declare(strict_types=1);` justo **en la primera línea** del script.

```php
<?php
function addNumbers(int $a, int $b) {
  return $a + $b;
}
echo addNumbers(5, "5 days");
// puesto que no está habilitado el modo "estricto", "5 days" se convierte en int(5), y devuelve 10
?>

<?php declare(strict_types=1); // Modo estricto activado

// En este "modo", hay que especificar el tipo de los argumentos
function addNumbers(int $a, int $b) {
  return $a + $b;
}
echo addNumbers(5, "5 days");
// Como "5 days" no es un entero, la función da error
?>
```

### Devolviendo un valor

Para que una función devuelva un dato, usamos la palabra clave `return`:

```php
<?php declare(strict_types=1); // strict requirement
function sum(int $x, int $y) {
  $z = $x + $y;
  return $z;
}

echo "5 + 10 = " . sum(5, 10) . "<br>";
echo "7 + 13 = " . sum(7, 13) . "<br>";
echo "2 + 4 = " . sum(2, 4);
?>
```

!!! note "Devolver un tipo de dato"
    Igual que con los argumentos, se puede especificar el tipo de dato que devuelve una función, justo al declararla:

    ```php
    <?php declare(strict_types=1); // strict requirement
    function addNumbers(float $a, float $b) : float {
        return $a + $b;
    }
    echo addNumbers(1.2, 5.2);
    ?>
    
    // Se puede especificar un tipo de dato distinto al de los argumentos
    <?php declare(strict_types=1); // strict requirement
    function addNumbers(float $a, float $b) : int {
      return (int)($a + $b);
    }
    echo addNumbers(1.2, 5.2);
    ?>
    ```

### Pasar argumentos por referencia

En PHP los argumentos se suelen pasar _por valor_: la función usa una copia del valor, dejando intacta la variable original.

Cuando se pasa un argumento a una función _por referencia_, la variable también cambia. Para ello, hay que usar el operador `&`:

```php
<?php
function add_five(&$value) {
  $value += 5;
}

$num = 2;

// la siguiente función ACTUALIZA el valor de $num (normalmente no lo haría)
add_five($num);
echo $num;
?>
```

### Imprimir: echo / print

Las sentencias `echo` y `print` hacen lo mismo: **imprimir** contenido en la pantalla.

La diferencia es que:

- `echo`:
    - no devuelve nada.
    - puede recibir **múltiples parámetros**
    - es un poquito más rápido
- `print`:
    - devuelve `1`, por lo que se puede usar en expresiones.
    - sólo puede recibir **un argumento**

```php
<?php
echo "<h2>PHP is Fun!</h2>"; // EVITAR meter código HTML como parámetro (claridad)
echo "Hello world!<br>";
echo "I'm about to learn PHP!<br>";
echo "This ", "string ", "was ", "made ", "with multiple parameters.";
?>

<?php
$x = 5;
$y = 6;
print "<h2>PHP is Fun!</h2>";
print "Hello world!<br>";
print "I'm about to learn PHP!";
print $x + $y;
?>
```

### Manejo de Cadenas

**Longitud de una cadena**
:    con `strlen()`

    ```php
    <?php
    echo strlen("Hola mundo!"); // resultado 12
    ?>
    ```

**Contar palabras**
:   Con `str_word_count()`:
    
    ```php
    <?php
    echo str_word_count("Hola mundo!"); // resultado 2
    ?>
    ```

**Invertir cadena**
:   Con `strrev()`:

    ```php
    <?php
    echo strrev("Hola mundo!"); // resultado !odnum aloH
    ?>
    ```

**Buscar dentro de una cadena**
:   Con `strpos()`; devuelve la posición del carácter de la primera coincidencia, o _false_ si no lo encuentra.

    ```php
    <?php
    echo strpos("Hola mundo!", "mundo"); // resultado 6
    ?>
    ```

**Sustituir parte de una cadena**
:   Con `str_replace()`:

    ```php
    <?php
    echo str_replace("mundo", "Dolly", "Hola mundo!"); // Devuelve Hola Dolly!
    ?>
    ```

### Manejo de Números (matemáticas)

Algunas de las funciones más comunes para realizar operaciones matemáticas son:

- `pi()`: devuelve el valor de PI
- `min()` y `max()`: valor mínimo y máximo en una lista de números
- `abs()`: valor absoluto positivo de un número
- `sqrt()`: raíz cuadrada
- `round()`: redondea un decimal al entero más cercano.
- `rand(min, max)`: genera un número **aleatorio** entre los parámetros (inclusivol).

```php
<?php
echo(pi()); // 3.1415926535898

echo(min(0, 150, 30, 20, -8, -200)); // devuelve -200
echo(max(0, 150, 30, 20, -8, -200)); // devuelve 150

echo(abs(-6.7)); // devuelve 6.7

echo(sqrt(64)); // devuelve 8

echo(round(0.60)); // 1
echo(round(0.49)); // 0

echo(rand());
echo(rand(10, 100));
?>
```

### Manejo de Arrays

Esta es una lista de las funciones más útiles:

`array()`
:   Crea un array con los elementos pasados por parámetro.

`count()`
:   Cuenta el número de elementos de un array
    
    ```php
    <?php
    $cars = array("Volvo", "BMW", "Toyota");
    echo count($cars);
    ?>
    ```

**Bucle a través de los elementos de un array**
:   Usando un `for`:

    ```php
    <?php
    $cars = array("Volvo", "BMW", "Toyota");
    $arrlength = count($cars);

    for($x = 0; $x < $arrlength; $x++) {
      echo $cars[$x];
      echo "<br>";
    }
    ?>
    ```

    ```php
    // Array asociativo
    <?php
    $age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

    foreach($age as $x => $x_value) {
      echo "Key=" . $x . ", Value=" . $x_value;
      echo "<br>";
    }
    ?>
    ```

#### Ordenar arrays

**Ascendente** y **descendente** 
:   `sort()` y `rsort()`

    ```php
    <?php
    $cars = array("Volvo", "BMW", "Toyota");
    sort($cars);
    ?>

    <!-- con números -->
    <?php
    $numbers = array(4, 6, 2, 22, 11);
    rsort($numbers);
    ?>
    ```

**Según un valor**
:   En arrays _clave-valor_, con `asort()` y `arsort()` (descendente):

    ```php
    <?php
    $age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");
    asort($age);
    ?>
    ```
 
**Según la clave**
:   En arrays _clave-valor_, con `ksort()` y `krsort()` (descendente):

    ```php
    <?php
    $age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");
    ksort($age);
    ?>
    ```

## Expresiones regulares

En PHP, las expresiones regulares son _cadenas_ compuestas de:

- delimitadores
- un patrón
- modificadores opcionales

```php
<?php
$exp = "/w3schools/i";
// "/"         = delimitador
// "w3schools" = patrón
// "i"         = modificador; búsqueda ignorando may. y min.
?>
```

### Funciones para Expr. Regulares

- `preg_match()`     : devuelve `1` si se encuentra el patrón; `0` si no.
- `preg_match_all()` : devuelve el número de coincidencias del patrón (incluido 0).
- `preg_replace()`   : devuelve una nueva cadena donde los patrones encontrados han sido reemplazados.

```php
<?php
$str = "Visit W3Schools";
$pattern = "/w3schools/i";
echo preg_match($pattern, $str); // Outputs 1

// cuenta las ocurrencias de "ain" sin tener en cuenta la capitalización
$str = "The rain in SPAIN falls mainly on the plains.";
$pattern = "/ain/i";
echo preg_match_all($pattern, $str); // Outputs 4

// reemplaza "Microsoft" por "W3Schools"
$str = "Visit Microsoft!";
$pattern = "/microsoft/i";
echo preg_replace($pattern, "W3Schools", $str); // Outputs "Visit W3Schools!"
?>

```

### Modificadores

| Modificador | Descripción                    |
|-------------|--------------------------------|
| i           | Ignora mayúsculas y minúsculas |
| m           | Búsqueda multilínea            |
| u           | Soporte para UTF-8             |

### Patrones

| Expresión | Descripción                                         |
|-----------|-----------------------------------------------------|
| `[abc]`   | Encuentra uno de los caracteres incluido            |
| `[^abc]`  | Encuentra todo **excepto** los caracteres incluidos |
| `[0-9]`   | Rango de posibilidades                              |

### Metacaracteres

| Metacarácter | Descripción                                                              |
|--------------|--------------------------------------------------------------------------|
| `            | `                                                                        | Alternativa: `gato | perro | pez` |
| `.`          | Encuentra solo **una instancia** de cualquier carácter                   |
| `^`          | Busca al principio de la cadena: `^Hola`                                 |
| `$`          | Busca al final de la cadena: `mundo$`                                    |
| `\d`         | Número                                                                   |
| `\s`         | Espacio en blanco                                                        |
| `\b`         | Busca al comienzo de una palabra (`\bPALABRA`), o al final (`PALABRA\b`) |
| `\uxxxx`     | Busca signo Unicode por su código hexadecimal                            |

### Cuantificadores

| Cuantificador | Descripción                         |
|---------------|-------------------------------------|
| `n+`          | Al menos una _n_                    |
| `n*`          | Cero o más de una ocurrencia de _n_ |
| `n?`          | Cero o Una ocurrencia de _n_        |
| `n{x}`        | Secuencia de x _n_                  |
| `n{x,y}`      | Secuencia de entre x e y _n_        |
| `n{x,}`       | Secuencia de **al menos** x _n_     |

### Grupos

Se pueden usar paréntesis para crear **grupos** dentro de un patrón:

- para aplicar cuantificadores a un conjunto de elementos.
- para seleccionar partes del patrón como coincidencia.

```php
<?php
$str = "Apples and bananas.";
$pattern = "/ba(na){2}/i";
echo preg_match($pattern, $str); // Outputs 1
?>
```

