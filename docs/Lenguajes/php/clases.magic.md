# Métodos mágicos

!!! info "Fuentes"
    - [Web de Diego](https://diego.com.es/metodos-magicos-en-php)

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

Suponemos que tenemos una aplicación en la que queremos **extraer tweets** de la API de Twitter. Extraemos en JSON los tweets del usuario y queremos **transformar** cada tweet en un objeto de PHP con el que trabajar.

## Contructor y destructor

El método mágico más utilizado en PHP es `__construct()`, un método que es llamado automáticamente cuando se crea el objeto. Permite inyectar **parámetros** y **dependencias** para construir el objeto.

```php
<?php
class Tweet {
    protected $id;
    protected $text;
    public function __construct ($id, $text){
        $this->id = $id;
        $this->text = $text;
    }
}
$tweet = new Tweet(54, "Hola que tal");
```

Cuando creamos una nueva instancia del objeto Tweet, podemos pasarle parámetros que se inyectarán en el método `__construct()`.

!!! note "Contructores heredados"
    Los constructores también se heredan. Si una clase hija define un constructor propio, para heredar el del padre es necesario indicarlo con `parent::__construct()`:

    ```php
    <?php
    class Entidad {
        protected $meta;
        public function __construct(array $meta){
            $this->meta = $meta;
        }
    }
    class Tweet extends Entidad {
        protected $id;
        protected $text;
        public function __construct ($id, $text, array $meta){
            $this->id = $id;
            $this->text = $text;
            parent::__construct($meta);
        }
    }
    ```

## Destructor

El método `__destruct()` hace lo contrario, se ejecuta cuando un objeto se destruye, aunque **apenas se utiliza**. Si por ejemplo tenemos una conexión a la base de datos en el objeto y queremos asegurarnos de que se cierra al destruir el objeto, podemos hacer lo siguiente:

```php
<?php
public function __destruct(){
    $this->connection->destroy();
}
```

## Get() y Set()

Los métodos mágicos `__get()` y `__set()` actúan como _getters_ y _setters_ para **propiedades que no tienen visibilidad _public_**.

```php
<?php
public function __get($property){
    if(property_exists($this, $property)) {
        return $this->$property;
    }
}
```

El método acepta el nombre de la propiedad que estabas buscando como argumento.

No es necesario llamar al método `__get()` ya que PHP lo llamará automáticamente cuando trates de acceder a una propiedad que no es _public_. También lo llamará cuando no existe una propiedad, por ejemplo:

```php
<?php
class Datos {
    public function __get($propiedad){
        echo "Esta propiedad no existe!";
    }
}
$datos = new Datos;
$datos->noexisto; // Devuelve: Esta propiedad no existe!
```

También podemos establecer una propiedad que no existe con `__set()`:

```php
<?php
class Datos {
    public function __set($propiedad, $valor){
        $this->propiedad = $valor;
    }
}
$datos = new Datos;
$datos->noexisto = "Ahora si que existo";
var_dump($datos);
/*
object(Datos)[1]
  public 'propiedad' => string 'Ahora si que existo' (length=19)
*/
```

Otra forma de uso es con una propiedad de tipo array:

```php
<?php
class Datos {
    protected $datos = array();
    public function __set($nombre, $valor){
        $this->datos[$nombre] = $valor;
    }
    public function __get($nombre){
        return $this->datos[$nombre];
    }
}
$datos = new Datos;
$datos->primero = "Este es el primer dato";
$datos->segundo = "Este es el segundo dato";
$datos->tercero = "Este es el tercer dato";
echo $datos->primero . "<br>";
echo $datos->segundo . "<br>";
echo $datos->tercero;
```

Si eliminamos el método `set()` y añadimos el _ampersand_ de referencia `&` en `get()`, podemos establecer y obtener las propiedades:

```php
<?php
class Datos {
    protected $datos = array();
    public function &__get($nombre){
        return $this->datos[$nombre];
    }
}
$datos = new Datos;
$datos->primero = "Este es el primer dato";
$datos->segundo = "Este es el segundo dato";
$datos->tercero = "Este es el tercer dato";
echo $datos->primero . "<br>";
echo $datos->segundo . "<br>";
echo $datos->tercero;
```

!!! note "Cuándo usarlos"
    **No se recomienda** usar estos métodos por varias razones:

    - son más lentos
    - no facilitan el autocompletado de código en los IDE
    - complican el mantenimiento y la refactorización
    - no es posible tener un método magic _protected_... 
    
    En su lugar se pueden emplear _getters_ y _setters_.

## isset() y unset()

=== "isset"
    `__isset()` permite **comprobar la existencia de una propiedad no pública**.

    ```php
    <?php
    public function __isset($property){
        return isset($this->$property);
    }
    var_dump(isset($tweet->id));
    ```

    - La alternativa sería usar la función `isset()` normal **dentro de la clase**.
=== "unset"
    Si `unset()` vacía arrays o elimina propiedades _públicas_, `__unset()` permite eliminar propiedades **no publicas**:

    ```php
    <?php
    public function __unset($propiedad){
        unset($this->$propiedad);
    }
    ```

## toString()

El método mágico `__toString()` permite devolver el objeto en forma de string **directamente con** `echo()`:

=== "Ejemplo 1"
    ```php
    <?php
    public function __toString(){
        return $this->text;
    }

    $tweet = new Tweet(12, 'Hola que tal');
    echo $tweet; // devuelve "Hola qué tal"
    ```
=== "Ejemplo 2"
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
