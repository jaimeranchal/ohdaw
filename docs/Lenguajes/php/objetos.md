# Manejo de Objetos en PHP

## Fundamentos de POO
POO son las siglas de _Programación Orientada a Objetos_; mientras la programación procedural se centra en escribir procedimientos (o funciones) para manejar datos, en POO el objetivo es crear _Objetos_ que contengan tanto las funciones como los datos.

Sus ventajas son:

- Más **rápida** y **fácil de ejecutar**.
- La **estructura** es más clara
- Ayuda a mantener el principio DRY ( _Dont Repeat Yourself_ ), es decir, a **reutilizar código**.

## Clases y Objetos

Una clase es una _plantilla_ para objetos, mientras que un objeto es una _instancia_ de una clase.

### Definir una clase

```php
<?php
class Fruta {
    // código
    // Propiedades o atributos
    public $nombre;
    public $color;

    // Métodos
    // Setter
    function set_name($nombre) {
        $this -> nombre = $nombre;
    }
    // Getter
    function get_name() {
        return $this -> nombre;
    }
}
?>
```

### Instanciar objetos

```php
<?php
$manzana = new Fruta();
$banana = new Fruta();
$manzana -> set_name('Manzana');
$banana -> set_name('Banana');

// Imprimimos las propiedades de los objetos
echo $manzana->get_name();
echo "<br>";
echo $banana->get_name();
?>
```

#### La palabra clave `$this`
La palabra `$this` se refiere al **objeto actual** y sólo está disponible _dentro de métodos_:

```php
<?php
class Fruta {
    public $nombre;
    function set_name($nombre) {
        // Aquí $this sustituye a la instancia del objeto Fruta
        $this -> nombre = $nombre;
    }
}
$manzana = new Fruta();
// Fuera de un método no podemos usar $this
$manzana->set_name('Manzana dorada');
?>
```

#### `instanceof`
Esta palabra clave sirve para comprobar si un objeto es una instancia de una determinada clase:

```php
<?php
$manzana = new Fruta();
var_dump($manzana instanceof Fruta);
?>
```

## Constructores
Un método constructor nos permite inicializar las propiedades de un objeto en el momento de su creación. Si declaras una función `__construct()` PHP la usará para crear un objeto de esa clase.

Si usas este método no es necesario crear un método `set_name()` (a menos que necesites un _setter_ por separado):

```php
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  function get_name() {
    return $this->name;
  }
}

$apple = new Fruit("Apple");
echo $apple->get_name();
?>
```

!!! important "Sobrecarga de métodos"
    PHP **no admite sobrecarga** de métodos, como sí hacen otros lenguajes POO.

## Destrucción de Objetos
Es un método análogo al constructor, que permite eliminar un objeto, porque sea necesario, o porque termine de ejecutarse el script. Para definir nuestro propio método destructor usamos `__destruct()`:

```php
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  // Invocada automáticamente al terminar el script
  function __destruct() {
    echo "The fruit is {$this->name}.";
  }
}

$apple = new Fruit("Apple");
?>
```

## Modificadores de acceso
PHP admite tres modificadores de visibilidad:

- **public**: accesible en cualquier parte. Opción _por defecto_.
- **protected**: accesible solo _dentro de la clase_ (y subclases).
- **private**: sólo accesible _dentro de su clase_.

=== "Propiedades"
    ```php
    <?php
    class Fruit {
      public $name;
      protected $color;
      private $weight;
    }

    $mango = new Fruit();
    $mango->name = 'Mango'; // OK
    $mango->color = 'Yellow'; // ERROR
    $mango->weight = '300'; // ERROR
    ?>
    ```

=== "Métodos"
    ```php
    <?php
    class Fruit {
      public $name;
      public $color;
      public $weight;

      function set_name($n) {  // a public function (default)
        $this->name = $n;
      }
      protected function set_color($n) { // a protected function
        $this->color = $n;
      }
      private function set_weight($n) { // a private function
        $this->weight = $n;
      }
    }

    $mango = new Fruit();
    $mango->set_name('Mango'); // OK
    $mango->set_color('Yellow'); // ERROR
    $mango->set_weight('300'); // ERROR
    ?>
    ```
    
## Herencia
## Constantes
## Clases Abstractas
## Interfaces
## Traits
## Métodos estáticos
## Atributos estáticos
## Namespaces
## Iterables
