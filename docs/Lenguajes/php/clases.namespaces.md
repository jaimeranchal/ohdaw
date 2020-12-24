# Namespaces
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

## Acceso a espacios de nombre

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

## Alias 
Para darle un _alias_ a un espacio de nombre (para que sea más cómodo):

```php
<?php
use Html as H;
$table = new H\Table();
?>
```

