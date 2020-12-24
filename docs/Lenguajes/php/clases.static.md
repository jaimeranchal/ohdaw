# Propiedades y métodos estáticos

Las propiedades y métodos estáticos permiten que sean **accesibles sin necesidad de instanciar** la clase. 

## Acceso
Para acceder a constantes y métodos estáticos usamos el _paamayim nekudotayim_, **operador de resolución de ámbito** o **doble dos puntos** "`::`" (_double colon_). Este operador también permite **sobreescribir propiedades o métodos** de una clase.

!!! note "Acceso"
    No se puede acceder a una propiedad static con un objeto instanciado, aunque sí a un método static.

Puesto que los métodos estáticos pueden ejecutarse sin tener una instancia del objeto, no es posible usar la pseudo-variable `$this`, ya que `$this` hace referencia al objeto creado.

!!! important "Operador self"
    Para referirnos a propiedades estáticas de una clase, en lugar de `$this`, usamos el **operador** `self` seguido del **operador de dos puntos doble**:

=== "Acceso a propiedad estática"
    ```php
    <?php
    class Coche {
        public static $color = 'rojo';

        public function mostrarColor()
        {
            return self::$color;
        }
    }

    print Coche::$color . "\n"; // Muestra "rojo"

    // Creando el objeto miCoche:
    $miCoche = new Coche();
    print $miCoche->mostrarColor() . "\n"; // Muestra "rojo"
    print $miCoche->color . "\n"; // Error, propiedad color no definida
    print $miCoche::$color . "\n"; // Muestra "rojo"
    ```
=== "Acceso a método estático"
    ```php
    <?php
    class Coche {
        public static function mostrarColor()
        {
            echo 'Rojo';
        }
    }
    Coche::mostrarColor(); // Muestra Rojo
    $miCoche = new Coche;
    $miCoche->mostrarColor(); // Muestra Rojo
    ```
