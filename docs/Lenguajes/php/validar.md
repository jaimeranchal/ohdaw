# Validación de datos

!!! info "Fuente"
    Lo siguiente es una traducción de una estupenda entrada de [supunkavinda.blog](https://supunkavinda.blog/php/input-validation-with-php)

Validar los datos consiste en comprobar que cumplen unos determinados requisitos de _forma_ o _tipo de dato_. Es una cuestión de **seguridad** y una **obligación** para evitar errores e incluso ataques.

## Validar el método de entrada

Uno de los contextos más habituales en el que necesitamos validar los datos (si no es el que más) son los formularios. HTML proporciona dos _métodos de petición_ de datos: `GET` y `POST`.

!!! info "Resumen de GET y POST"

    **El método GET**

    - Se usa para **obtener** datos de un recurso
    - Enviar datos mediante la URL como un par clave-valor (visible)
        - Cambia la dirección URL
        - Los datos enviados pueden guardarse en la caché del navegador
        - Permanecen en el historial
    - Es menos seguro que POST pero _más rápido_.
    - **Nunca debe usarse** para transferir datos sensibles
    - Tiene limitaciones:
        - longitud
        - solo datos ASCII

    **El método POST**
    
    - Se usa para **enviar** datos al servidor
    - Envía los datos en el cuerpo del mensaje HTTP
        - No se guarda en el historial
    - **Debe usarse** para tratar datos sensibles
    - Más lento que GET pero _más seguro_.
    - No tiene limitaciones de longitud ni tipo de datos.

Vamos un ejemplo de formulario sencillo:

=== "HTML"

    ```html
    <form method="POST" action="act.php">
        <!-- elementos del formulario -->
    </form>
    ```

=== "PHP"

    ```php
    <?php
    if ($_SERVER['REQUEST_METHOD'] === 'POST'){
        // el método de petición está bien
    } else {
        exit('Invalid request');
    }
    ?>
    ```

    Puedes cambiar `POST` por el método que uses en tu formulario:
    
## Comprobar datos introducidos por el usuario

Imaginemos que tenemos un campo para el correo electrónico en un formulario que usa `POST`. El código php asigna la variable POST a una variable local así:

```php
<?php
$email = $_POST['email'];
?>
```

Si no se introduce un email, PHP lanzará un error estilo _Undefined Index: email_. Por lo tanto, lo primero que hay que comprobar es si **las variables de entrada ( _input_ ) han sido fijadas**; es decir, si se les ha dado algún valor concreto.

Para ellos podemos usar las funciones `empty()` y `isset()`:

=== "Inputs no booleanos"
    - Usamos la función `empty()` que comprueba si el argumento está vacío o no.
    - En los siguientes casos devolverá _true_:
        - `""`      : una cadena vacía
        - `0`       : como entero
        - `0.0`     : 0 como un _float_
        - `"0"`     : 0 como una _cadena_
        - `NULL`
        - `FALSE`
        - `array()` : un array vacío

    ```php
    <?php
    if (!empty($_POST['email'])){
        $email = $_POST['email'];
    }
    ?>
    ```

    En este caso sólo se asigna el valor de _email_ si no está vacío. Si el campo es obligatorio (como suele serlo), habrá que lanzar un mensaje de error si lo está.

=== "Inputs booleanos"
    - La función `isset()` es más apropiada para datos de tipo booleano porque la función `empty()` devuelve _true_ para un valor _false_ (porque efectivamente, _false_ no es un valor vacío).
    - La función `isset()` comprueba que:
        - la variable **está fijada** o **declarada**
        - el valor de la variable **no es nulo**.

    ```php
    <?php
    if (isset($_POST['boolean'])){
        $boolean = $_POST['boolean'];
    }
    ?>
    ```

## Validar entradas

### Prevenir ataques XSS
El primer ataque que puede producirse mediante un formulario no validado es una **inyección de código** ( _Cross-Site Scripting_ ).

Imaginemos que un formulario para introducir el _nombre de usuario_:


- Si alguien introdujera como nombre de usuario `<script>location.href='https://www.atacker.com'</script>`, que es un código javascript, ese se guardaría en la base de datos tal cual.
- Si después usáramos un código para mostrar los nombres de usuario, al llegar al valor introducido antes, el usuario desafortunado sería _redirigido_ automáticamente a una web atacante.

Para evitar esto:

- usamos una función que **elimina los caracteres especiales** que identifican al código como tal, `htmlspecialchars()`. Así, el código anterior se convertiría en `&lt;script&gt;location.href='https://www.attacker.com'&lt;/script&gt;`.
- Además de transformar ciertos símbolos, también hay que **eliminar los espacios en blanco** para no ocupar espacio extra en la base de datos. En esta caso usamos la función `trim()`.

=== "HTML"
    ```html
    <form method="POST" action="">
        <input type="text" name="username"/>
        <input type="submit" name="submit"/>
    </form>
    ```
=== "PHP (htmlspecialchars)"
    ```php
    <?php
    if (!empty($_POST['username']) && !empty($_POST['email'])) {
        $username = htmlspecialchars($_POST['username']);
        $email = htmlspecialchars($_POST['email']);
    }
    ?>
    ```
=== "PHP (htmlspecialchars + trim)"
    ```php
    <?php
    if (!empty($_POST['username']) && !empty($_POST['email'])) {
        $username = trim(htmlspecialchars($_POST['username']));
        $email = trim(htmlspecialchars($_POST['email']));
    }
    ?>
    ```

### Validar tipos de datos concretos

PHP incluye una función para **validar variables**, `filter_var()` con la siguiente sintaxis:

```
filter_var($variable [, $filter = FILTER_TIPO...[, $opciones]])
```

!!! note "Tipos de filtro"
    La función `filter_var()` admite [muchos filtros](https://www.php.net/manual/es/filter.filters.php). Si no se escoge ninguno o se usa `FILTER_DEFAULT` los datos se pasarán tal cual, sin filtrar. Los más comunes son:

    - **FILTER_VALIDATE_BOOLEAN**
    - **FILTER_VALIDATE_EMAIL**
    - **FILTER_VALIDATE_INT**
    - **FILTER_VALIDATE_FLOAT**
    - **FILTER_VALIDATE_REGEXP** + una expresión regular. Útil para campos de _cadena_
    - **FILTER_VALIDATE_URL**

=== "Email"
    ```php
    <?php
    if (!empty($_POST['email'])) {

        $email = trim(htmlspecialchars($_POST['email']));
        $email = filter_var($email, FILTER_VALIDATE_EMAIL);

        if ($email === false) {
            exit('Invalid Email');
        }

    }
    ```

=== "URL"
    ```php
    <?php
    if (!empty($_POST['url'])) {

        $url = trim(htmlspecialchars($_POST['url']));
        $url = filter_var($url, FILTER_VALIDATE_URL);

        if ($url === false) {
            exit('Invalid URL');
        }

    }
    ```
=== "Enteros"
    ```php
    <?php
    if (!empty($_POST['number'])) {

        $number = $_POST['number'];
        $number = filter_var($number, FILTER_VALIDATE_INT);

        if ($number === false) {
            exit('Invalid number');
        }

    }
    ```

    !!! note "Nota"
        La opción `FILTER_VALIDATE_INT` **convierte** automáticamente números en formato cadena a numérico. Con el resto de tipos:

        - 25.11 devolverá _false_ porque no es un entero.
        - _true_ (booleano) devuelve `1` (entero)
        - _false_ (booleano) devuelve _false_ (booleano)
        - Arrays, Objetos y cadenas sin caracteres numéricos devuelven _false_ (booleano).

=== "Booleanos"
    ```php
    <?php
    if (!empty($_POST['check'])){
        
        $check = $_POST['check'];
        $check = filter_var($check, FILTER_VALIDATE_BOOLEAN);

    }
    ?>
    ```

    !!! important "Control de checkbox"
        Muchos navegadores envían una cadena `"on"` cuando el usuario activa una opción. La opción `FILTER_VALIDATE_BOOLEAN` **convierte** automáticamente cadenas del tipo `"on"`, `"yes"`, `"true"` a _true_ (booleano). Cualquier otro valor de entrada devuelve _false_.

## Clase de Validación

PHP permite Programación Orientada a Objetos, lo que nos permite guardar todas las funciones de validación que necesitemos en un clase, dentro de un archivo separado.

Vamos a crear un ejemplo con los siguientes métodos:

- `check()` : comprueba que los inputs no están vacíos
- `int()`   : valida enteros
- `str()`   : escapa caracteres htmo y elimina espacios en blanco
- `bool()`  : convierte cualquier variable en booleano
- `email()` : valida emails
- `url()`   : valida URLs.

```php
<?php

class  Input {
    /* Variable estática.
     * True = muestra mensajes de error
     * False = desactiva los mensajes de error.
     */
	static $errors = true;

	static function check($arr, $on = false) {
		if ($on === false) {
			$on = $_REQUEST;
		}
		foreach ($arr as $value) {	
			if (empty($on[$value])) {
				self::throwError('Data is missing', 900);
			}
		}
	}

	static function int($val) {
		$val = filter_var($val, FILTER_VALIDATE_INT);
		if ($val === false) {
			self::throwError('Invalid Integer', 901);
		}
		return $val;
	}

	static function str($val) {
		if (!is_string($val)) {
			self::throwError('Invalid String', 902);
		}
		$val = trim(htmlspecialchars($val));
		return $val;
	}

	static function bool($val) {
		$val = filter_var($val, FILTER_VALIDATE_BOOLEAN);
		return $val;
	}

	static function email($val) {
		$val = filter_var($val, FILTER_VALIDATE_EMAIL);
		if ($val === false) {
			self::throwError('Invalid Email', 903);
		}
		return $val;
	}

	static function url($val) {
		$val = filter_var($val, FILTER_VALIDATE_URL);
		if ($val === false) {
			self::throwError('Invalid URL', 904);
		}
		return $val;
	}

    /* Lanza un mensaje de error si la variable $error es TRUE.
     * Si no necesitas mostrar los errores, ajusta el valor de
     * $error a FALSE
     */
	static function throwError($error = 'Error In Processing', $errorCode = 0) {
		if (self::$errors === true) {
			throw new Exception($error, $errorCode);
		}
	}
}
?>

```

Cuando queramos usar estos métodos, sólo tenemos que **importar** la clase al principio del documento, o crear una [función de autocarga](https://supunkavinda.blog/php/autoload-classes-namespaces):

```php
<?php
include('validacion.php');

// Usamos el Scope Resolution Operator para acceder a métodos ESTÁTICOS
// Clase::miMetodoEstático();
Input::check(['email', 'password'], $_POST);

// Valida un entero
$number = Input::int($_POST['number']);

// Valida una cadena
$name = Input::str($_POST['name']);

// Convierte en booleano
$bool = Input::bool($_POST['boolean']);

// Valida un email
$email = Input::email($_POST['email']);

// Valida una URL
$email = Input::email($_POST['email']);
$url = Input::url($_POST['url']);
?>
```

