# Constantes

Las constantes son **valores invariables**. Se definen mediante la palabra `const` y se obtienen utilizando el **operador doble dos puntos**: `$objeto::CONSTANTE` o `NombreClase::CONSTANTE`. 

- **No van precedidas del símbolo** `$`
- se escriben en **mayúscula**:

```php
<?php
class Coche {
    const RUEDAS = 4;
}

// Obtener el valor mediante el nombre de la clase:
echo Coche::RUEDAS . "\n";
// Obtener el valor mediante el objeto:
$miCoche = new Coche();
echo $miCoche::RUEDAS . "\n";
```

