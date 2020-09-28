# PHP

PHP ( _Hypertext Preprocessor_ ) es un lenguaje de código abierto muy popular especialmente adecuado para el desarrollo web y que puede ser incrustado en HTML.

Fue creado inicialmente por el programador danés-canadiense Rasmus Lerdorf en 1994. En la actualidad, la implementación de referencia de PHP es producida por The PHP Group.

- Última versión estable: 7.4.10 ( 3/9/2020 )
- Sistema de tipos: Dinámico, débil

```php
<!DOCTYPE html>
<html>
    <head>
        <title>Ejemplo</title>
    </head>
    <body>

        <?php
            echo "¡Hola, soy un script de PHP!";
        ?>

    </body>
</html>
```

- Las páginas en PHP contienen HTML con **código incrustado** que hace "algo" (en este caso, mostrar "¡Hola, soy un script de PHP!). 
- El código de PHP está encerrado entre las etiquetas especiales de comienzo y final `<?php `y `?>` que permiten entrar y salir del _modo PHP_.

Lo que distingue a PHP de algo del lado del cliente como Javascript es que el código es ejecutado en el servidor, generando HTML y enviándolo al cliente.
