# Interfaces

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

