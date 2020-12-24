# Rasgos o traits

Los traits son como clases que agrupan funcionalidades diseñadas para ser **reutilizadas en otras clases** y así evitar las limitaciones de la herencia simple.

- A diferencia de las clases, **no se pueden instanciar**, tan sólo permiten utilizar sus funciones específicas en otras clases.
- Los traits permiten relaciones _horizontales_ entre clases evitando así la verticalidad.

En el siguiente ejemplo se utiliza un trait para mostrar el modelo de coche. En la clase _Ventas_ se incluye `use Modelo`; este trait tiene una función `getModelo()` que presupone que en la clase donde se vaya a utilizar existe un método `getMarca()` de la clase _Coche_. Los métodos de la clase _Ventas_ sobreescriben los métodos del trait, y éstos a su vez sobreescriben la clase _Coche_:

```php
<?php
class Coche {
    public function getMarca() {
        echo 'Renault ';
    }
}
trait Modelo {
    public function getModelo() {
        parent::getMarca();
        echo 'Clio';
    }
}
class Ventas extends Coche {
    use Modelo;
}

$o = new Ventas();
$o->getMarca(); // Podemos obtener este método de la clase Coche
$o->getModelo(); // Podemos obtener este método del trait Modelo
?>
```

Se pueden emplear múltiples traits añadiéndolos a la clase:

```php
<?php
class Ventas extends Coche {
    use Model, Serie, Versión;
    //...
}
?>
```

O incluso emplear múltiples traits en otro trait para luego usarlo en la clase:

```php
<?php
trait Coche {
    public function getMarca() {
        echo 'Renault ';
    }
}
trait Modelo {
    public function getModelo() {
        echo 'Clio';
    }
}
trait CocheModelo {
    use Coche, Modelo;
}

class Venta {
    use CocheModelo;
}

$o = new Venta();
$o->getMarca();
$o->getModelo();
?>
```

Los traits también pueden incluir **propiedades y métodos estáticos**.
