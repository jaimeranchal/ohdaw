# Sesiones de usuario

!!! info "Documentación"
    - [Manual de php.net](https://www.php.net/manual/en/book.session.php)
    - [El blog de Diego Lázaro](https://diego.com.es/sesiones-en-php)

## ¿Qué es una sesión?
Una _sesión_ es una forma de asociar ciertas actividades realizadas en una aplicación con un usuario: cuánto entró, qué hizo, y cuándo salió, por ejemplo. Las **variables de sesión** guardan la información del usuario a lo largo de varias páginas.

`$_SESSION` es un array especial utilizado para guardar información a través de los requests que un usuario hace durante su visita a un sitio web o aplicación. La información guardada en una sesión puede llamarse en cualquier momento mientras la sesión esté abierta.

A cada usuario que accede a la aplicación e inicia sesión se le asigna un session **ID único**, y es lo que le permite identificar la sesión y que esté disponible para ese usuario en concreto. La forma más segura de manejar sesiones es almacenando en el cliente sólo esta session ID, y cualquier información de la sesión guardarla en el lado del servidor.

!!! note "Duración"
    Por defecto, las variables de sesión duran hasta que se cierra el navegador.

!!! tip "Usuarios permanentes"
    Si quieres que las sesiones se guarden de forma permanente, considera usar **bases de datos**.

## Iniciar una sesión en PHP

Antes de guardar cualquier información en una sesión es necesario **iniciar el manejo se la sesión** (_session handling_). Esto se hace al principio del código PHP, y debe hacerse antes de que que cualquier texto, HTML o JavaScript se envíe al navegador. 

- Se inicia con la función `session_start()`
- las variables de sesión se guardan en la variable global `$_SESSION`

=== "Demo 1"
    ```php
    <?php
    // Start the session
    session_start();
    ?>
    <!DOCTYPE html>
    <html>
        <body>

        <?php
        // Declaramos variables de sesión
        $_SESSION["favcolor"] = "green";
        $_SESSION["favanimal"] = "cat";
        $_SESSION["tiempo"] = time();

        echo "Se han guardado estas variables de sesión.";
        // imprime toda la información guardada en sesiones
        print_r($_SESSION); 
        ?>

        </body>
    </html>
    ```

!!! important "La función `session_start()` de ser lo primero que aparezca en el documento"

## Obtener las variables de sesión

Vamos a crear una página llamada _demo_de_sesion2.php_, desde la que vamos a recuperar la información de sesiones de otra página ( _demo_de_sesion1_ )

La función `session_start()` **mantiene los datos de sesión** si ya ha sido iniciada en otra página.

- Las variables de sesión **no se pasan individualmente**, sino que se recuperan _desde_ la sesión abierta con `session_start()`.
- Todas las variables de sesión se guardan en `$_SESSION`

=== "Demo 2"
    ```php
    <?php
    session_start();
    ?>
    <!DOCTYPE html>
    <html>
        <body>

        <?php
        // Echo session variables that were set on previous page
        if (isset($_SESSION["favcolor"] && isset($_SESSION["favanimal"]))){
            echo "Favorite color is " . $_SESSION["favcolor"] . ".<br>";
            echo "Favorite animal is " . $_SESSION["favanimal"] . ".";
        } else {
            echo "No se ha iniciado sesión"
        }
        ?>

        </body>
    </html>
    ```
=== "Demo 2 (alt)"
    ```php
    <?php
    session_start();
    ?>
    <!DOCTYPE html>
    <html>
        <body>

        <?php
        print_r($_SESSION);
        ?>

        </body>
    </html>
    ```

!!! info "Cómo funciona"
    La mayoría de sesiones crean un par _clave-valor_ en el ordenador (algo como 765487cf34ert8dede5a562e4f3a7e12). Luego, cuando se abre una sesión en otra página, busca la clave en el ordenador; si la encuentra, accede a _esa_ sesión, si no, crea una nueva.

## Modificar una variable de sesión

Simplemente se sobreescribe:

```php
<?php
session_start();
?>
<!DOCTYPE html>
<html>
<body>

<?php
// to change a session variable, just overwrite it
$_SESSION["favcolor"] = "yellow";
print_r($_SESSION);
?>

</body>
</html>
```
## Eliminar sesiones

Para eliminar sesiones usamos dos funciones:

- `session_unset()`
- `session_destroy()`

!!! warning "Vaciar sesiones VS destruir sesiones"
    Según el manual de PHP.net, destruir inmediatamente una sesión puede provocar errores y sobre todo perjudica el rendimiento de la aplicación, puesto que puede darse el caso de que se generen muchas nuevas IDs en poco tiempo.

    La alternativa es simplemente **vaciar las sesiones**:

    ```php
    <?php
    session_start();
    // sobreescribe todo lo que tenga guardado con un array vacío
    $_SESSION = array();
    ?>
    ```

```php
<?php
session_start();
?>
<!DOCTYPE html>
<html>
<body>

<?php
// Elimina todas las variables de sesión
// NO ELIMINA la sesión como tal
session_unset();

// Destruye la sesión
session_destroy();
?>

</body>
</html>
```

## Consejos de seguridad

A pesar de su simplicidad, hay en determinadas ocasiones que el uso de sesiones puede salir mal. En esta sección se muestran algunas técnicas sencillas para evitar riesgos con las sesiones:

### Sessión Time-Outs

Establecer un tiempo de vida a las sesiones es algo importante para lidiar con las sesiones de usuarios en aplicaciones. Si un usuario inicia sesión en un lugar y se olvida de cerrarla, otros pueden continuar con la misma, por eso es mejor establecer un tiempo máximo de sesión:

```php
<?php
session_start();
// Establecer tiempo de vida de la sesión en segundos
$inactividad = 600;
// Comprobar si $_SESSION["timeout"] está establecida
if(isset($_SESSION["timeout"])){
    // Calcular el tiempo de vida de la sesión (TTL = Time To Live)
    $sessionTTL = time() - $_SESSION["timeout"];
    if($sessionTTL > $inactividad){
        session_destroy();
        header("Location: /logout.php");
    }
}
// El siguiente key se crea cuando se inicia sesión
$_SESSION["timeout"] = time();
?>
```

El código asegura que si no hay ninguna actividad en 10 minutos, cualquier request en adelante redigirirá a la página de logout.

### Regenerar el Session ID
La función `_session_regenerateid()` crea un nuevo ID único para representar la sesión actual del usuario. Esto se debe realizar cuando se realizan acciones importantes como logearse o modificar los datos del usuario. Darle a las sesiones un nuevo ID reduce la probabilidad de ataques session hijacking:

```php
<?php
session_start();
if($_POST["usuario"] = "admin" && $_POST["password"] == sha1($password)){
    $_SESSION["autorizado"] = true;
    session_regenerate_id();
}
?>
```

### Destruir la sesión
Como ya se ha mencionado antes, es importante utilizar `session_destroy()` cuando ya no se vaya a hacer uso de la sesión.

### Utilizar almacenamiento permanente
Utilizar una base de datos para almacenar los datos que se cree que serán persistentes. Dejarlos en la sesión por demasiado tiempo abre de nuevo puertas a ataques. Depende del desarrollador decidir que datos serán almacenados o se mantendrán en `$_SESSION`.
