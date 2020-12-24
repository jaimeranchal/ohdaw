# Métodos mágicos

Los métodos mágicos permiten reaccionar a **eventos** que le puedan ocurrir a un objeto instanciado.

Comienzan con una **doble barra baja**(`__`) y son: 

1. `__construct()`,
2. `__destruct()`,
3. `call()`,
4. `callStatic()`,
5. `__get()`,
6. `__set()`,
7. `__isset()`,
8. `__unset()`,
9. `sleep()`,
10. `wakeup()`,
11. `__toString()`,
12. `invoke()`,
13. `set_state()`,
14. `clone()` 
15. `debugInfo()`

Vamos a ver algunos de ellos:

## Contructor

El más utilizado es el constructor `__construct()`, que se ejecuta cuando el objeto se crea, pudiendo inyectar parámetros y dependecias requeridos para crear el objeto.

```php
<?php
class Coche {
    public $color;

    public function __construct($color)
    {
        $this->color = $color;
    }
}

$miCoche = new Coche('Rojo');
echo $miCoche->color; // Devuelve Rojo
?>
```

!!! note "Contructores heredados"
    Los constructores también se heredan. Si una clase hija define un constructor propio, para heredar el de la madre es necesario indicarlo con `parent::__construct()`:

    ```php
    <?php
    class Coche {
        public $color;

        public function __construct($color)
        {
            $this->color = $color;
        }
    }
    class CocheDeLujo extends Coche {
        public $extras;

        public function __construct($color, $extras)
        {
            parent::__construct($color);
            $this->extras = $extras;
        }
    }

    $miCoche = new CocheDeLujo('Rojo', 'TV');
    echo $miCoche->color; // Devuelve Rojo
    echo $miCoche->extras; // Devuelve TV
    ?>
    ```

## Destructor

El método `__destruct()` hace lo contrario, se ejecuta cuando un objeto se destruye, aunque **apenas se utiliza**.

## Getter y Setter

Los métodos mágicos `__get()` y `__set()` actúan como _getters_ y _setters_ para **propiedades que no tienen visibilidad _public_**.

## Isset y unset

Este método mágico permite **comprobar la existencia de una propiedad no pública**.

El método `__isset()` actúa como la función `isset()` normal, como cuando se trabaja con arrays o variables. 

El método `__unset()` también actúa como la función `unset()` normal pero para **propiedades no públicas**.

## toString

El método mágico `__toString()` permite devolver el objeto en forma de string:

```php
<?php
class Post {
    public $title;

    public function setTitle($title)
    {
        $this->title = $title;
    }
    public function __toString()
    {
        return $this->title;
    }
}

$post = new Post;
$post->setTitle('Este es mi primer artículo');
echo $post; // Devuelve Este es mi primer artículo
?>
```

