# Formularios en PHP

- Fuente: https://www.w3schools.com/php/php_forms.asp

Tomemos un ejemplo simple de formulario en HTML, que guarde los datos en un archivo llamado `bienvenida.php`:

```html
<html>
    <body>
        <form action="bienvenida.php" method="post">
        Nombre: <input type="text" name="nombre"><br>
        Email: <input type="text" name="email"><br>
        <input type="submit">
        </form>
    </body>
</html>
```

Para mostrar los datos podemos imprimir simplemente las variables. El archivo `bienvenida.php` sería así:

```php
<html>
<body>

Bienvenido <?php echo $_POST["nombre"]; ?><br>
Tu dirección de email es: <?php echo $_POST["email"]; ?><br>

</body>
</html>
```

Todo está bien...expecto porque **no hemos validado los datos**.

!!! note "Seguridad"
    Es _vital_ que se controle la seguridad de transmisión de los datos en un formulario, especialmente si contiene **información personal**

## GET y POST

Ambos métodos crean _array_ de tipo _clave-valor_:

- La **clave** es el atributo `name` del elemento `<input>`.
- El **valor** son los datos introducidos por el usuario.

### Método GET

- La información enviada por el método GET es **visible para todo el mundo** (y aparece en la URL)
- Está limitado a envíos de unos 2000 caracteres.
- Se usa para información **no sensible**

### Método POST

- La información enviada mediante POST es **invisible a los demás** (se insertan en el cuerpo de la petición HTML)
- **No tiene límite** a la cantidad de información que puede enviar/recibir.
- Soporta funciones avanzadas como entrada de binarios de varias partes ( _multi-part_ ) al subir archivos al servidor.

!!! note "Siempre usa POST"
    Si puedes, usa siempre el método POST

## Ejemplo

Vamos a crear un formulario con las siguientes reglas:

| Campo      | Reglas de validación                                                 |
|------------|----------------------------------------------------------------------|
| Nombre     | Obligatorio. Sólo puede contener **letras** y **espacios en blanco** |
| email      | Obligatorio. Debe ser una dirección válida (con `@`)                 |
| web        | Opcional. Si existe, debe ser una dirección válida                   |
| Comentario | Opcional. Campo multilínea (`textarea`)                              |
| Género     | Obligatorio. Debe **seleccionarse** uno                              |

Veamos cada tipo de elemento por partes:

### Campos de texto

```html
Nombre: <input type="text" name="name">
E-mail: <input type="text" name="email">
Website: <input type="text" name="website">
Comentario: <textarea name="comment" rows="5" cols="40"></textarea>
```

### Botones radio

```html
Género:
<input type="radio" name="gender" value="female">Female
<input type="radio" name="gender" value="male">Male
<input type="radio" name="gender" value="other">Other
```

### Elemento `<form>`

```html
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
```

!!! note "`$_SERVER["PHP_SELF"]`"
    Es una [variable superglobal](./tutorial.md#superglobales) que devuelve el nombre de archivo del script que se está ejecutando actualmente.
    
    De esta forma el usuario recibirá los mensajes de error en la misma página que el formulario

!!! note "La función `htmlspecialchars()`"
    Convierte caracteres especiales a entidades HTML, por ejemplo `<` a `&lt;` etc. Esto impide que los atacantes inyecten código HTML o JavaScript en los formularios.

### Nota general sobre seguridad

Los atacantes pueden usar la variable `$_SERVER["PHP_SELF"]`; si tu página usa esta variable un usuario puede introducir una barra inclinada (`/`) seguida de algún comando XSS ( _Cross Site Scripting_ ) y se ejecutarán.

!!! note "XSS"
    _Cross Site Scripting_ es un tipo de vulnerabilidad de seguridad típica de las aplicaciones web, que permite al atacante inyectar código en las páginas que ven otros usuarios desde el lado del cliente.

Imaginemos que tenemos el siguiente formulario en una página llamada "test_form.php":

```php
<form method="post" action="<?php echo $_SERVER["PHP_SELF"];?>">
```

Ahora, alguien escribe en la barra de direcciones del navegador:

```
http://www.example.com/test_form.php/%22%3E%3Cscript%3Ealert('hacked')%3C/script%3E
```

Ese código se convierte en:

```html
<form method="post" action="test_form.php/"><script>alert('hacked')</script>
```

- Añade una etiqueta `<script>` y el comando JavaScript `alert()`.
- Cuando la página cargue, se ejecutará el código y el usuario verá un mensaje "Hackeado!".

!!! warning "Cuidado"
    Se puede añadir _cualquier código_ JavaScript dentro de la etiqueta `<script>`. Un usuario con mala intención puede redirigir a otros hacia un servidor diferente, o alterar las variables globales o enviar el formulario a una dirección distinta (y robarnos los datos).

Evita este tipo de ataques usando la **función `htmlspecialchars()`**. Con ella, el código insertado en el ejemplo anterior se convierte en:

```html
<form method="post" action="test_form.php/&quot;&gt;&lt;script&gt;alert('hacked')&lt;/script&gt;">
<!-- Y todo sigue funcionando como debería -->
```

## Crear una función de validación

1. Pasar las variables a través de la función `htmlspecialchars()`
2. Eliminar caracteres innecesarios (espacios de sobra, tabulaciones, saltos de línea) con la función `trim()`
3. Eliminar las barras inclinadas a la izquierda (`\`) con la función `stripslashes()`
4. Escribir una **función** que haga _todo lo anterior_

```php
<?php
// define variables y les asigna un valor vacío
$name = $email = $gender = $comment = $website = "";

// Comprobamos que se ha enviado el formulario
if ($_SERVER["REQUEST_METHOD"] == "POST") { //si el método es POST, se ha enviado
// los siguientes campos son OPCIONALES
  $name = test_input($_POST["name"]);
  $email = test_input($_POST["email"]);
  $website = test_input($_POST["website"]);
  $comment = test_input($_POST["comment"]);
  $gender = test_input($_POST["gender"]);
}

function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
?>
```

### Campos obligatorios

En el ejemplo anterior, los campos eran _opcionales_; ahora vamos a hacer que sean **obligatorios**

- Primero creamos una serie de variables que contengan los **mensajes de error**.
- Luego comprobamos si el `input` está vacío.
    - Si lo está, actualiza el valor de la variable correspondiente
    - Si no, pasa los datos a la función `test_input()`

```php
<?php
// define variables y las deja vacías
$nameErr = $emailErr = $genderErr = $websiteErr = "";
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  if (empty($_POST["name"])) {
    $nameErr = "Name is required";
  } else {
    $name = test_input($_POST["name"]);
  }

  if (empty($_POST["email"])) {
    $emailErr = "Email is required";
  } else {
    $email = test_input($_POST["email"]);
  }

  if (empty($_POST["website"])) {
    $website = "";
  } else {
    $website = test_input($_POST["website"]);
  }

  if (empty($_POST["comment"])) {
    $comment = "";
  } else {
    $comment = test_input($_POST["comment"]);
  }

  if (empty($_POST["gender"])) {
    $genderErr = "Gender is required";
  } else {
    $gender = test_input($_POST["gender"]);
  }
}
?>
```

Luego, en el formulario HTTP añadimos una orden para imprimir esas variables en cada campo:

```php
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">

    Name: <input type="text" name="name">
    <span class="error">* <?php echo $nameErr;?></span>
    <br><br>
    E-mail:
    <input type="text" name="email">
    <span class="error">* <?php echo $emailErr;?></span>
    <br><br>
    Website:
    <input type="text" name="website">
    <span class="error"><?php echo $websiteErr;?></span>
    <br><br>
    Comment: <textarea name="comment" rows="5" cols="40"></textarea>
    <br><br>
    Gender:
    <input type="radio" name="gender" value="female">Female
    <input type="radio" name="gender" value="male">Male
    <input type="radio" name="gender" value="other">Other
    <span class="error">* <?php echo $genderErr;?></span>
    <br><br>
    <input type="submit" name="submit" value="Submit">

</form>
```

### Validar nombres, emails y URLs

#### Nombres

Debemos comprobar que el campo _solo_ contiene **letras**, **guiones**, **apóstrofes** y **espacios**. Si no cumple con esas condiciones, se genera un mensaje de error.

```php
<?php
$name = test_input($_POST["name"]);
if (!preg_match("/^[a-zA-Z-' ]*$/",$name)) {
  $nameErr = "Only letters and white space allowed";
}
?>
```

#### Email

La manera más fácil y segura es usar una función ya incluida en PHP, `filter_var()`:

```php
<?php
$email = test_input($_POST["email"]);
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
$emailErr = "Invalid email format";
}
?>
```

!!! info "Funciones de validación"
    La función `filter_var()` permite validar más cosas además del email. Admite tres parámetros:

    1. El valor a filtrar
    2. El filtro que se quiere aplicar
    3. Opciones (si el filtro las admite)

    Más info en [php.net](https://www.php.net/manual/es/function.filter-var.php).

#### URL

Para validar la dirección web usamos **expresiones regulares**:

```php
<?php
$website = test_input($_POST["website"]);
if (!preg_match("/\b(?:(?:https?|ftp):\/\/|www\.)[-a-z0-9+&@#\/%?=~_|!:,.;]*[-a-z0-9+&@#\/%=~_|]/i",$website)) {
  $websiteErr = "Invalid URL";
}
?>
```

## Ejemplo completo

El código PHP quedaría tal que así:

```php
<?php
// define variables and set to empty values
$nameErr = $emailErr = $genderErr = $websiteErr = "";
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  if (empty($_POST["name"])) {
    $nameErr = "Name is required";
  } else {
    $name = test_input($_POST["name"]);
    // check if name only contains letters and whitespace
    if (!preg_match("/^[a-zA-Z-' ]*$/",$name)) {
      $nameErr = "Only letters and white space allowed";
    }
  }

  if (empty($_POST["email"])) {
    $emailErr = "Email is required";
  } else {
    $email = test_input($_POST["email"]);
    // check if e-mail address is well-formed
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
      $emailErr = "Invalid email format";
    }
  }

  if (empty($_POST["website"])) {
    $website = "";
  } else {
    $website = test_input($_POST["website"]);
    // check if URL address syntax is valid (this regular expression also allows dashes in the URL)
    if (!preg_match("/\b(?:(?:https?|ftp):\/\/|www\.)[-a-z0-9+&@#\/%?=~_|!:,.;]*[-a-z0-9+&@#\/%=~_|]/i",$website)) {
      $websiteErr = "Invalid URL";
    }
  }

  if (empty($_POST["comment"])) {
    $comment = "";
  } else {
    $comment = test_input($_POST["comment"]);
  }

  if (empty($_POST["gender"])) {
    $genderErr = "Gender is required";
  } else {
    $gender = test_input($_POST["gender"]);
  }
}
?>
```

Para mostrar los valores introducidos ( _después_ de darle al botón de enviar) en el mismo formulario, dejamos el código así:

```php
Name: <input type="text" name="name" value="<?php echo $name;?>">

E-mail: <input type="text" name="email" value="<?php echo $email;?>">

Website: <input type="text" name="website" value="<?php echo $website;?>">

Comment: <textarea name="comment" rows="5" cols="40"><?php echo $comment;?></textarea>

Gender:
<input type="radio" name="gender"
<?php if (isset($gender) && $gender=="female") echo "checked";?>
value="female">Female
<input type="radio" name="gender"
<?php if (isset($gender) && $gender=="male") echo "checked";?>
value="male">Male
<input type="radio" name="gender"
<?php if (isset($gender) && $gender=="other") echo "checked";?>
value="other">Other
```

