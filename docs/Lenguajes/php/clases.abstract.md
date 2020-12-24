# Clases Abstractas
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

