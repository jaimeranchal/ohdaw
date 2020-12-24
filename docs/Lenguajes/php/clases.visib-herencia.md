# Interacción entre clases

Supongamos ahora que queremos comparar y mostrar qué coche es más rápido dependiendo de su potencia. Añadimos un nuevo método a la clase Coche:

```php
<?php
class Coche {
    //...

    public function elCocheElegidoEsMasRapido($cocheElegido)
    {
        return $cocheElegido->potencia > $this->potencia;
    }
}
//...
?>
```

Tenemos dos objetos `$miCoche` y `$otroCoche`. La condición que mostrará que coche es más rápido:

```php
<?php
//...
if ($miCoche->elCocheElegidoEsMasRapido($otroCoche)) {
    echo 'El ' . $otroCoche->marca . ' es más rápido';
} else {
    echo 'El ' . $miCoche->marca . ' es más rápido';
}

$miCoche->elCocheElegidoEsMasRapido($otroCoche);
// En este caso el Audi ($miCoche) es más rápido que el BMW ($otroCoche)
?>
```

## Comparar objetos

Podemos comparar la igualdad entre dos objetos, con los **operadores de comparación** (`==`) y de **identidad** (`===`):

- _Dos objetos serán iguales_ si tienen los **mismos atributos y valores**, siendo instancias de la misma clase. En ejemplo anterior, los atributos de los dos objetos deberían tener los mismos valores, por ejemplo: verde, 100, audi.
- _Dos objetos serán idénticos_ si además hacen **referencia a la misma instancia** de la misma clase.

## Visibilidad

### Public

Anteriormente hemos definido todas las propiedades y métodos como **public**. Esto significa que pueden ser accedidos y alterados desde cualquier parte fuera de la clase. En el ejemplo anterior hemos podido definir `$miCoche->potencia` sin ningún problema, y podríamos aplicarle cualquier valor (un número, un string...).

### Private

Cuando se define una propiedad como **private**, se indica que ésta no puede verse o modificarse a no ser que sea _desde la propia clase_. Si utilizamos `$miCoche->potencia` para verla o definirla, nos dará error. 

#### Cambiar propiedades privadas

Si queremos definir una propiedad privada se utiliza un `setter`, un método public para definir una propiedad private:

=== "Definición del setter"
    ```php
    <?php
    //...
    class Coche {

        //...

        public function setPotencia($potencia)
        {
            $this->potencia = $potencia;
        }
        //...
        // Viene bien tener un getter para poder acceder al valor de la propiedad:
        public function getPotencia()
        {
            return $this->potencia;
        }
        //...
    }
    ```
=== "Ejemplo de uso"
    Así podemos definir desde fuera de la clase el valor de la propiedad potencia:

    ```php
    <?php
    //...
    $miCoche->setPotencia(120);
    ?>
    ```

Con esto conseguimos **limitar el rango de valores posibles**.

En este caso, la propiedad `$potencia` queremos que pueda ser sólo un número. Para ello, modificamos el setter anterior:

```php
<?php
//...
public function setPotencia($potencia)
{
    if(!is_numeric($potencia)){
        throw new \Exception('Potencia no válida: '. $potencia);
    }
    // Si $potencia no es un número, devolverá un error
    $this->potencia = $potencia;
}
//...
?>
```

## Herencia y visibilidad "protected"

Para entender las propiedades y métodos bajo la palabra reservada _protected_, vamos a explicar primero las relaciones entre clases mediante la herencia.

- Una clase puede heredar métodos y propiedades de otra clase, y éstos se pueden **sobreescribir** empleando el _mismo nombre_ que en la clase madre.
- Las clases sólo pueden **heredar de una única clase** (no es posible herencia múltiple), pero sí que pueden **heredarse unas a otras** (sí es posible herencia multinivel).
- La herencia se realiza mediante la **palabra `extends`**.

Supongamos ahora la clase Coche con 3 tipos de propiedades y _CocheDeLujo_ como heredera:

```php
<?php
class Coche {
    private $color = 'red';
    protected $potencia = 120;
    public $marca = 'audi';
}
class CocheDeLujo extends Coche {

    // La función displayColor devolverá un error porque es private
    public function displayColor()
    {
        return $this->color;
    }
    // La función displayPotencia devolverá 120, ya que hereda el valor
    public function displayPotencia()
    {
        return $this->potencia;
    }
    // La función displayMarca devolverá audi, ya que también hereda el valor
    public function displayMarca()
    {
        return $this->marca;
    }
}
?>
```

La clase _CocheDeLujo_ podrá emplear y modificar las propiedades y métodos protected y public de la madre, pero no las private. Además, podrá añadir cualquier propiedad o métodos complementarios.

Creamos varios métodos en la clase Coche, incluído `printCaracteristicas()`:

```php
<?php
class Coche {
    protected $color;
    public function setColor($color)
    {
        $this->color = $color;
    }
    public function getColor()
    {
        return $this->color;
    }
    public function printCaracteristicas()
    {
        echo 'Color: '. $this->getColor;
    }
}
?>
```

Añadimos otros métodos en _CocheDeLujo_, y modificamos la herencia de `printCaracteristicas()`:

```php
<?php
class CocheDeLujo extends Coche {
    protected $extras;
    public function setExtras($extras)
    {
        $this->extras = $extras;
    }
    public function getExtras()
    {
        return $this->extras;
    }
    public function printCaracteristicas()
    {
        echo 'Color: '. $this->color;
        echo '<hr/>';
        echo 'Extras: ' . $this->extras;
    }
}

$miCoche = new CocheDeLujo();
$miCoche->setColor('negro');
$miCoche->setExtras('TV');

$miCoche->printCaracteristicas(); // Devuelve Color : negro Extras: TV
?>
```

El objeto `$miCoche`, que es una instancia de la clase _CocheDeLujo_, puede determinar la nueva propiedad `$extras` además de las anteriores. El método que se ha heredado se ha modificado para añadir la nueva característica.

### Bloqueo de herencia con `final`

Es posible impedir que un método pueda sobreescribirse mediante la palabra final:

```php
<?php
class Coche {
    final public function getColor()
    {
        echo "Rojo";
    }
}
class CocheDeLujo extends Coche {
    public function getColor()
    {
        echo "Azul";
    }
} // Dará un error fatal, getColor() no puede sobreescribirse
?>
```

También se puede impedir que la clase pueda heredarse mediante la misma palabra:

```php
<?php
final class Coche {
    public function getColor()
    {
        echo "Rojo";
    }
}
class CocheDeLujo extends Coche {
    // Error fatal, clase no heredable
}
?>
```

## Resumen

Accesos que permiten las palabras reservadas public, protected y private en métodos y propiedades:

- Private:
    - Desde la misma clase que declara
- Protected:
    - Desde la misma clase que declara
    - Desde las clases que heredan esta clase
- Public:
    - Desde la misma clase que declara
    - Desde las clases que heredan esta clase
    - Desde cualquier elemento fuera de la clase
