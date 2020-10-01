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

## Ejemplo completo
