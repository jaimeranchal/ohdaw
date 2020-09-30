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

## Validación

Vamos a crear un formulario con las siguientes reglas:

| Campo      | Reglas de validación                                                 |
|------------|----------------------------------------------------------------------|
| Nombre     | Obligatorio. Sólo puede contener **letras** y **espacios en blanco** |
| email      | Obligatorio. Debe ser una dirección válida (con `@`)                 |
| web        | Opcional. Si existe, debe ser una dirección válida                   |
| Comentario | Opcional. Campo multilínea (`textarea`)                              |
| Género     | Obligatorio. Debe **seleccionarse** uno                              |

### Código de ejemplo

Veamos cada tipo de elemento por partes:

#### Campos de texto

```html
Nombre: <input type="text" name="name">
E-mail: <input type="text" name="email">
Website: <input type="text" name="website">
Comentario: <textarea name="comment" rows="5" cols="40"></textarea>
```

#### Botones radio

```html
Género:
<input type="radio" name="gender" value="female">Female
<input type="radio" name="gender" value="male">Male
<input type="radio" name="gender" value="other">Other
```

#### Elemento `<form>`

```html
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
```


### Nombres, URL y e-mail

## Campos obligatorios

## Ejemplo completo
