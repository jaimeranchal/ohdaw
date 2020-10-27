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
La herencia significa que una clase hijo _hereda_ las propiedades y métodos públicos o protegidos de la clase padre. Además de estos puede tener sus propias propiedades y métodos.

### Declaración

=== "Normal"
    ```php
    <?php
    // clase padre
    class Fruit {
      public $name;
      public $color;
      public function __construct($name, $color) {
        $this->name = $name;
        $this->color = $color;
      }
      public function intro() {
        echo "The fruit is {$this->name} and the color is {$this->color}.";
      }
    }

    // La clase Strawberry "hereda" de Fruta
    class Strawberry extends Fruit {
      public function message() {
        echo "Am I a fruit or a berry? ";
      }
    }
    $strawberry = new Strawberry("Strawberry", "red");
    $strawberry->message();
    $strawberry->intro();
    ?>
    ```

=== "Visibilidad 'protected'"
    ```php
    <?php
    class Fruit {
      public $name;
      public $color;
      public function __construct($name, $color) {
        $this->name = $name;
        $this->color = $color;
      }
      protected function intro() {
        echo "The fruit is {$this->name} and the color is {$this->color}.";
      }
    }

    class Strawberry extends Fruit {
      public function message() {
        echo "Am I a fruit or a berry? ";
      }
    }

    // Try to call all three methods from outside class
    $strawberry = new Strawberry("Strawberry", "red");  // OK. __construct() is public
    $strawberry->message(); // OK. message() is public
    $strawberry->intro(); // ERROR. intro() is protected
    ?>
    ```

=== "Visibilidad 'protected' (otro ejemplo)"
    ```php
    <?php
    class Fruit {
      public $name;
      public $color;
      public function __construct($name, $color) {
        $this->name = $name;
        $this->color = $color;
      }
      protected function intro() {
        echo "The fruit is {$this->name} and the color is {$this->color}.";
      }
    }

    class Strawberry extends Fruit {
      public function message() {
        echo "Am I a fruit or a berry? ";
        // Call protected method from within derived class - OK
        $this -> intro();
      }
    }

    $strawberry = new Strawberry("Strawberry", "red"); // OK. __construct() is public
    $strawberry->message(); // OK. message() is public and it calls intro() (which is protected) from within the derived class
    ?>
    ```
### Sobreescribir métodos heredados
La manera más simple es _redefinir_ los métodos en la clase hijo con el **mismo nombre** que tienen en la clase padre

```php
<?php
class Fruit {
  public $name;
  public $color;
  public function __construct($name, $color) {
    $this->name = $name;
    $this->color = $color;
  }
  public function intro() {
    echo "The fruit is {$this->name} and the color is {$this->color}.";
  }
}

class Strawberry extends Fruit {
  public $weight;
  public function __construct($name, $color, $weight) {
    $this->name = $name;
    $this->color = $color;
    $this->weight = $weight;
  }
  public function intro() {
    echo "The fruit is {$this->name}, the color is {$this->color}, and the weight is {$this->weight} gram.";
  }
}

$strawberry = new Strawberry("Strawberry", "red", 50);
$strawberry->intro();
?>
```

### Bloqueo de herencia con la palabra `final`
La palabra clave `final` puede usarse para **impedir**:

- La herencia
- La sobreescritura de métodos

=== "Bloqueo de herencia"
    ```php
    <?php
    final class Fruit {
      // some code
    }

    // will result in error
    class Strawberry extends Fruit {
      // some code
    }
    ?>
    ```

=== "Impedir sobreescritura"
    ```php
    <?php
    class Fruit {
      final public function intro() {
        // some code
      }
    }

    class Strawberry extends Fruit {
      // will result in error
      public function intro() {
        // some code
      }
    }
    ?>
    ```
## Constantes
Una constante no se puede cambiar una vez ha sido declarada.

- Se declaran con la palabra clave `const`
- Son sensibles a mayúscula-minúscula; se recomienda siempre en MAYÚSCULA
- Podemos **acceder** al valor de una constante:
    - _desde fuera_ de la clase con el **operador de resolución de ámbito** (`::`):
    - _desde dentro_ usando `self::CONSTANTE`

=== "Desde fuera"
    ```php
    <?php
    class Goodbye {
      const LEAVING_MESSAGE = "Thank you for visiting W3Schools.com!";
    }

    echo Goodbye::LEAVING_MESSAGE;
    ?>
    ```
=== "Desde dentro"
    ```php
    <?php
    class Goodbye {
      const LEAVING_MESSAGE = "Thank you for visiting W3Schools.com!";
      public function byebye() {
        echo self::LEAVING_MESSAGE;
      }
    }

    $goodbye = new Goodbye();
    $goodbye->byebye();
    ?>
    ```

## Clases Abstractas
Las clases y métodos _abstractos_ **no están implementadas**, sólo **definidas** (tienen un nombre y nada más); son las clases hijo las que se encargarán de implementarlas.

Se considera que una clase es _abstracta_ si tiene _al menos un_ método abstracto.

Clases y métodos abstractos se declaran con la palabra clave `abstract`.

```php
<?php
abstract class ParentClass {
  abstract public function someMethod1();
  abstract public function someMethod2($name, $color);
  abstract public function someMethod3() : string;
}
?>
```

Los métodos abstractos deben implementarse:

- con el **mismo nombre**
- **visibilidad** igual o menos restringida
- El **tipo** y **número** de parámetros deben ser iguales
- Las clases hijo pueden **añadir parámetros** extra.

=== "Ejemplo"
    ```php
    <?php
    // Parent class
    abstract class Car {
      public $name;
      public function __construct($name) {
        $this->name = $name;
      }
      abstract public function intro() : string;
    }

    // Child classes
    class Audi extends Car {
      public function intro() : string {
        return "Choose German quality! I'm an $this->name!";
      }
    }

    class Volvo extends Car {
      public function intro() : string {
        return "Proud to be Swedish! I'm a $this->name!";
      }
    }

    class Citroen extends Car {
      public function intro() : string {
        return "French extravagance! I'm a $this->name!";
      }
    }

    // Create objects from the child classes
    $audi = new audi("Audi");
    echo $audi->intro();
    echo "<br>";

    $volvo = new volvo("Volvo");
    echo $volvo->intro();
    echo "<br>";

    $citroen = new citroen("Citroen");
    echo $citroen->intro();
    ?>
    ```

=== "Método abstracto con argumento"
    ```php
    <?php
    abstract class ParentClass {
      // Abstract method with an argument
      abstract protected function prefixName($name);
    }

    class ChildClass extends ParentClass {
      public function prefixName($name) {
        if ($name == "John Doe") {
          $prefix = "Mr.";
        } elseif ($name == "Jane Doe") {
          $prefix = "Mrs.";
        } else {
          $prefix = "";
        }
        return "{$prefix} {$name}";
      }
    }

    $class = new ChildClass;
    echo $class->prefixName("John Doe");
    echo "<br>";
    echo $class->prefixName("Jane Doe");
    ?>
    ```
=== "Clase hija añade argumentos"
    ```php
    <?php
    abstract class ParentClass {
      // Abstract method with an argument
      abstract protected function prefixName($name);
    }

    class ChildClass extends ParentClass {
      // The child class may define optional arguments that are not in the parent's abstract method
      public function prefixName($name, $separator = ".", $greet = "Dear") {
        if ($name == "John Doe") {
          $prefix = "Mr";
        } elseif ($name == "Jane Doe") {
          $prefix = "Mrs";
        } else {
          $prefix = "";
        }
        return "{$greet} {$prefix}{$separator} {$name}";
      }
    }

    $class = new ChildClass;
    echo $class->prefixName("John Doe");
    echo "<br>";
    echo $class->prefixName("Jane Doe");
    ?>
    ```

## Interfaces
Las interfaces te permiten especificar qué métodos debe implementar una clase. Se declaran con la palabra clave `interface`:

!!! note "Polimorfismo"
    Cuando una o más clases usan la _misma interfaz_.

```php
<?php
interface InterfaceName {
  public function someMethod1();
  public function someMethod2($name, $color);
  public function someMethod3() : string;
}
?>
```

!!! info "Interfaces VS clases Abstractas"
    Aunque son similares, hay algunas **diferencias**:

    - Las interfaces no pueden tener propiedades; las clases abstractas si
    - Todos los métodos deben ser 
        - _públicos_; en las clases abstractas pueden ser públicos o protegidos.
        - _Abstractos_; no es necesario usar la palabra clave `abstract`
    - Una clase puede implementar una interfaz y heredar de otra clase _al mismo tiempo_.

### Usando interfaces
Una clase que _implementa_ una interfaz usa la palabra clave `implements`:

=== "Interfaz base"
    ```php
    <?php
    interface Animal {
      public function makeSound();
    }

    class Cat implements Animal {
      public function makeSound() {
        echo "Meow";
      }
    }

    $animal = new Cat();
    $animal->makeSound();
    ?>
    ```
=== "Clases implementadas"
    ```php
    <?php
    // Interface definition
    interface Animal {
      public function makeSound();
    }

    // Class definitions
    class Cat implements Animal {
      public function makeSound() {
        echo " Meow ";
      }
    }

    class Dog implements Animal {
      public function makeSound() {
        echo " Bark ";
      }
    }

    class Mouse implements Animal {
      public function makeSound() {
        echo " Squeak ";
      }
    }

    // Create a list of animals
    $cat = new Cat();
    $dog = new Dog();
    $mouse = new Mouse();
    $animals = array($cat, $dog, $mouse);

    // Tell the animals to make a sound
    foreach($animals as $animal) {
      $animal->makeSound();
    }
    ?>
    ```

    En este ejemplo, el método declarado en la interfaz se implementa de forma concreta para cada animal, pero todos "hacen ruido". Esto es lo que nos permite hacer un bucle (`foreach`) sobre todos ellos invocando su "ruido" sin saber de qué clase son.

## Traits
PHP sólo soporta la _herencia única_. ¿Qué pasa si queremos heredar de _varias clases_? Para eso están los "rasgos" (traits).

Los rasgos se usan para declarar métodos que vayan a usarse en varias clases:

- pueden tener métodos normales y abstractos
- pueden incluir métodos con cualquier visibilidad.
- Se declaran con la palabra clave `trait`

=== "Declaración"
    ```php
    <?php
    trait TraitName {
      // some code...
    }
    ?>
    ```
=== "Uso"
    ```php
    <?php
    class MyClass {
      use TraitName;
    }
    ?>
    ```
=== "Ejemplo"
    ```php
    <?php
    trait message1 {
    public function msg1() {
        echo "OOP is fun! ";
      }
    }

    class Welcome {
      use message1;
    }

    $obj = new Welcome();
    $obj->msg1();
    ?>
    ```
=== "Ejemplos 2"
    ```php
    <?php
    trait message1 {
      public function msg1() {
        echo "OOP is fun! ";
      }
    }

    trait message2 {
      public function msg2() {
        echo "OOP reduces code duplication!";
      }
    }

    class Welcome {
      use message1;
    }

    class Welcome2 {
      use message1, message2;
    }

    $obj = new Welcome();
    $obj->msg1();
    echo "<br>";

    $obj2 = new Welcome2();
    $obj2->msg1();
    $obj2->msg2();
    ?>
    ```

## Métodos estáticos
Los métodos estáticos se pueden **llamar directamente** sin crear una instancia de la clase.

Se declaran con la palabra clave `static`:

=== "Declaración"
    ```php
    <?php
    class ClassName {
      public static function staticMethod() {
        echo "Hello World!";
      }
    }
    ?>
    ```
=== "Acceso"
    ```php
    <?php
    // Sintaxis
    // ClassName::staticMethod();
    class greeting {
      public static function welcome() {
        echo "Hello World!";
      }
    }

    // Call static method
    greeting::welcome();
    ?>
    ```
=== "Ejemplo"
    ```php
    <?php
    class greeting {
      public static function welcome() {
        echo "Hello World!";
      }

      public function __construct() {
        self::welcome();
      }
    }

    new greeting();
    ?>
    ```
=== "Llamada 1"
    ```php
    <?php
    class greeting {
      public static function welcome() {
        echo "Hello World!";
      }
    }

    class SomeOtherClass {
      public function message() {
        greeting::welcome();
      }
    }
    ?>
    ```

    Para que un método estático pueda ser invocado desde métodos en otras clases debe ser público.

=== "Llamada 2"
    ```php
    <?php
    class domain {
      protected static function getWebsiteName() {
        return "W3Schools.com";
      }
    }

    class domainW3 extends domain {
      public $websiteName;
      public function __construct() {
        $this->websiteName = parent::getWebsiteName();
      }
    }

    $domainW3 = new domainW3;
    echo $domainW3 -> websiteName;
    ?>
    ```

    Para llamar a un método estático desde una clase hijo, usa la palabra clave `parent` dentro de la clase hijo. Aquí, el método estático puede ser público o protegido.

## Atributos estáticos
Como los métodos estáticos, los atributos estáticos pueden ser llamados directamente sin crear una instancia de clase.

=== "Declaración"
    ```php
    <?php
    class ClassName {
      public static $staticProp = "W3Schools";
    }
    ?>
    ```

=== "Acceso"
    ```php
    <?php
    class pi {
      public static $value = 3.14159;
    }

    // Get static property
    echo pi::$value;
    ?>
    ```

=== "Ejemplo"
    ```php
    <?php
    class pi {
      public static $value=3.14159;
      public function staticValue() {
        return self::$value;
      }
    }

    $pi = new pi();
    echo $pi->staticValue();
    ?>
    ```

    Acceso desde la misma clase con `self`.

=== "Ejemplo 2"
    ```php
    <?php
    class pi {
      public static $value=3.14159;
    }

    class x extends pi {
      public function xStatic() {
        return parent::$value;
      }
    }

    // Get value of static property directly via child class
    echo x::$value;

    // or get value of static property via xStatic() method
    $x = new x();
    echo $x->xStatic();
    ?>
    ```

    Acceso a una propiedad estática desde una clase hija usando `parent`.

## Namespaces
Los espacios de nombre resuelven dos problemas:

1. Permiten organizar mejor el código agrupando clases que trabajan juntas para resolver una tarea.
2. Permiten que varias clases usen el mismo nombre.

Por ejemplo puedes tener un conjunto de clases que describe una tabla HTML (Table, Row, Cell p.e.) pero también tienes otro conjunto que describe muebles con (tiene sentido en inglés) clases llamadas Table, Chair y Bed. Los espacios de nombre permiten agruparlas e impide que haya confusión al llamar a las dos clases llamadas "Table".

Los espacios de nombre se declaran con la palabra clave `namespace`:

!!! important "Primera línea"
    La declaración de un `namespace` debe ser lo primero que aparezca en un archivo PHP. Lo siguiente sería inválido:

    ```php
    <?php
    echo "Hello World!";
    namespace Html;
    ...
    ?>
    ```

```php
<?php
namespace Html;
class Table {
  public $title = "";
  public $numRows = 0;
  public function message() {
    echo "<p>Table '{$this->title}' has {$this->numRows} rows.</p>";
  }
}
$table = new Table();
$table->title = "My table";
$table->numRows = 5;
?>

<!DOCTYPE html>
<html>
<body>

<?php
$table->message();
?>

</body>
</html>
```

!!! note "Espacios de nombre anidados"
    Se pueden _anidar_ `namespace`s uno dentro de otro: `namespace Code\Html`

### Usar Namespaces
Todo el código que sigue a una declaración de `namespace` funciona _dentro_ de ese espacio  de nombre, de modo que no hace falta indicarlo al instanciar clases que pertenezcan a él. Para acceder a clases _externas_ a ese espacio de nombre, hay que añadirlo:

```php
<?php
// Declaración
$table = new Html\Table()
$row = new Html\Row();
?>

<?php
namespace Html;
$table = new Table();
$row = new Row();
?>
```

### Alias de espacio de nombre
Para darle un _alias_ a un espacio de nombre (para que sea más cómodo):

```php
<?php
use Html as H;
$table = new H\Table();
?>
```

## Iterables
