# Introducción a POO en PHP

POO son las siglas de _Programación Orientada a Objetos_; mientras la programación procedural se centra en escribir procedimientos (o funciones) para manejar datos, en POO el objetivo es crear _Objetos_ que contengan tanto las funciones como los datos.

Sus ventajas son:

- Más **rápida** y **fácil de ejecutar**.
- La **estructura** es más clara
- Ayuda a mantener el principio DRY ( _Dont Repeat Yourself_ ), es decir, a **reutilizar código**.

## Clases y objetos

Una clase define 

1. los **datos** 
2. la **lógica** de un objeto. La lógica se divide en:
    1. _funciones_ (métodos) y 
    2. _variables_ (propiedades).

!!! note "Definiciones"
    La clase es como una **plantilla** que define características y funciones. 
    El objeto agrupa los datos de la clase y permite utilizarlos desde una **unidad**.

Para crear un objeto hay que instanciar una clase:

=== "Instanciar un objeto"
    ```php
    <?php
    class Coche {
     // Contenido de la clase
    }

    $miCoche = new Coche(); // Objeto
    ?>
    ```
=== "Definir una propiedad"
    ```php
    <?php
    class Coche {
        public $color;
    }
    ?>
    ```

=== "Definir un método"
    ```php
    <?php
    class Coche {
        public function getColor()
        {
            // Contenido de la función
        }
    }
    ?>
    ```

### Acceso a propiedades

Podemos definir las características de un coche de la siguiente forma:

```php
<?php
class Coche {
    public $color;
    public $potencia;
    public $marca;
}

$miCoche = new Coche();
$miCoche->color = 'rojo';
$miCoche->potencia = 120;
$miCoche->marca = 'audi';
?>
```

Si queremos mostrar las propiedades que hemos definido usamos el **operador flecha** `->`:

```php
<?php
//...
echo 'Color del coche: ' . $miCoche->color; // Muestra Color del coche: rojo
?>
```

### Métodos _getter_

Ahora vamos a definir un método que devuelve una propiedad:

```php
<?php
class Coche {
    //...

    // Devuelve el color del objeto instanciado
    public function getColor()
    {
        return $this->color;
    }
}
//...
?>
```

!!! note "Variable `$this`"
    La variable `$this` se puede utilizar en cualquier método, y hace referencia al objeto que hemos instanciado, en este caso `$miCoche`. 

Podemos obtener el mismo resultado que antes para mostrar el color del coche pero ahora utilizando un método para mostrar la propiedad color:

```php
<?php
//...
echo 'Color del coche: ' . $miCoche->getColor();
Las propiedades pueden tener valores por defecto:

class Coche {
    public $color = 'rojo';
//...
}
?>
```

De esta forma siempre que se instancie un objeto de la clase Coche, éste será de color rojo a no ser que se modifique después. Ahora creamos también el objeto `$otroCoche` y le ponemos otras características:

```php
<?php
class Coche {

    public $color = 'rojo';
    public $potencia;
    public $marca;

    public function getColor()
    {
        return $this->color;
    }
}

$miCoche = new Coche();
$miCoche->color = 'verde';
$miCoche->potencia = 120;
$miCoche->marca = 'audi';

$otroCoche = new Coche();
$otroCoche->color = 'azul';
$otroCoche->potencia = 100;
$otroCoche->marca = 'bmw';
?>
```

Creamos ahora dos "getters" para la potencia y la marca:

```php
<?php
class Coche {

    public $color = 'rojo';
    public $potencia;
    public $marca;

    public function getColor()
    {
        return $this->color;
    }
    public function getPotencia()
    {
        return $this->potencia;
    }
    public function getMarca()
    {
        return $this->marca;
    }
}
?>
```

Y creamos una función (fuera de la clase) que devuelve las caracteristicas totales de un objeto coche:

```php
<?php
function printCaracteristicas($cocheConcreto)
    {
        echo 'Color: '. $cocheConcreto->getColor();
        echo "\n";
        echo 'Potencia: '. $cocheConcreto->getPotencia();
        echo "\n";
        echo 'Marca: '. $cocheConcreto->getMarca();
    }

$miCoche = new Coche();
$miCoche->color = 'verde';
$miCoche->potencia = 120;
$miCoche->marca = 'audi';

$otroCoche = new Coche();
$otroCoche->color = 'azul';
$otroCoche->potencia = 100;
$otroCoche->marca = 'bmw';

printCaracteristicas($miCoche);
echo "\n";
printCaracteristicas($otroCoche);
?>
```

La función `printCaracteristicas()` requiere un argumento, `$cocheConcreto`, que es una instancia de la clase Coche que mostrará las características del propio objeto.

