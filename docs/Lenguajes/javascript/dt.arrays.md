# Arrays

Un array es una _colección ordenada de datos_, donde cada **valor** está asociado a una **posición**.

!!! info "Diferencias con arrays en otros lenguajes (java)"
    - Puede almacenar valores de **cualquier tipo**, incluso mezclados
    - Su tamaño se modifica dinámicamente; si añado un elemento fuera de rango, el array amplía su tamaño como sea necesario.

## Declaración

```javascript
let arr = new Array("valor1", "valor2"); // la clásica
let arr = ["valor1", "valor2"]; //abreviada
```

## Manejo básico

=== "Acceso"
    ```javascript
    let fruits = ["Apple", "Orange", "Plum"];

    alert( fruits[0] ); // Apple
    alert( fruits[1] ); // Orange
    alert( fruits[2] ); // Plum   
    ```
=== "Añadir y reemplazar"
    ```javascript
    let fruits = ["Apple", "Orange", "Plum"];

    fruits[2] = 'Pear'; // now ["Apple", "Orange", "Pear"]
    fruits[3] = 'Lemon'; // now ["Apple", "Orange", "Pear", "Lemon"]
    ```
=== "Longitud"
    ```javascript
    let fruits = ["Apple", "Orange", "Plum"];

    alert( fruits.length ); // 3
    ```
=== "Mostrar todo"
    ```javascript
    let fruits = ["Apple", "Orange", "Plum"];
        
    alert( fruits ); // Apple,Orange,Plum
    ```


