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

## Métodos

### Transformar

- `toString()` :   Convierte un array en cadena separada por comas
- `join(" separador ")`:   Une todos los elementos de una array en una cadena, especificando el separador

### Añadir y quitar

- `pop()`: elimina el último elemento de un array y lo devuelve
- `push()`: añade un nuevo elemento _al final_; devuelve la nueva longitud del array.
- `shift()`: elimina el primer elemento del array y mueve (_shifts_) los demás; devuelve el elemento eliminado
- `unshift()`: añade un elemento _al comienzo_ y mueve los demás en consecuencia; devuelve la nueva longitud del array.
- `splice(int posicion, int delenda, valor [, valor 2...])`: añade los valores en la posición indicada, borrando (si se quiere) el número de elementos que se le diga (delenda). Devuelve un array con los elementos borrados.

### Cambiar el valor de elementos
Se hace directamente accediendo al elemento a través de su posición y sobreescribiendo el valor (no necesita método).

### Borrar directamente
Aunque **no se recomienda** eliminar _tal cual_ un elemento de un array, se puede hacer con `delete elemento[indice]`. Lo mejor en estos casos es usar alguna de las funciones descritas antes para quitar elementos.

### Unir Arrays
Con la función `concat`. Por ejemplo, `array1.concat(array2)` crea un nuevo array fusionando dos, _array1_ con _array2_. Esta función devuelve un nuevo array.

### Extraer parte de un array
Con la función `slice(from, to)`: extrae parte un array en otro nuevo; no incluye la última posición (to).

### Recorrer

Podemos user **tres bucles** además del clásico `for`:

- Bucle _for in_ 
- Bucle _for of_
- Bucle _for each_

Los tres utilizan una sintaxis abreviada para ejecutar acciones sobre cada _elemento_ de un objeto complejo; no sólo se usan con arrays, sino también con **Objetos** (para recorrer sus propiedades, p.e.):

=== "For In"
    ```javascript
    let mostrarForIn = function() {
        console.log("Recorrido for in");
        for (let key in array) {
            console.log(array[key]);
            // para imprimir la posición y el valor
            console.log(`${key} - ${array[key]}`);
        }
    }
    ```
=== "For Of"
    ```javascript
    let mostrarForOf = function() {
        console.log("Recorrido for of");
        // elemento apunta al contenido de la posición del array
        for (let elemento of array) {
        //for (let [key,elemento] of array.entries()) {
            console.log(elemento);
            //console.log(`${key} - ${elemento}`);
        }
    }
    ```
=== "For Each"
    ```javascript
    let mostrarForOf = function() {
        console.log("Recorrido for each");
        // elemento apunta al contenido de la posición del array
        array.forEach(element => {
            console.log(element)
        });
    }
    ```

### Ordenar arrays

- `sort()`: ordena _alfabéticamente_ un array.
- `reverse()`: invierte el orden de un array.
- **Ordenar números (función Comparar)**
  `arr.sort(function(a,b){return a - b});`: ascendente
  `arr.sort(function(a,b){return b - a});`: descendente
  `arr.sort(function(a,b){return 0.5 - Math.random()});`: aleatorio

!!! info "Aleatorio exacto: método Ficher Yates"
    ```javascript
    var points = [40, 100, 1, 5, 25, 10];

    for (i = points.length -1; i > 0; i--) {
      j = Math.floor(Math.random() * i)
      k = points[i]
      points[i] = points[j]
      points[j] = k
    }
    ```

### Valor más alto o más bajo

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b});
// now points[0] contains the lowest value
// and points[points.length-1] contains the highest value
```

## Arrays bidimensionales

Como tales, **no existen** en JavaScript, pero se puede conseguir con un pequeño truquillo:

```javascript
array = [];
array[0] = []; //creamos un array vacío DENTRO de la posición 0, que nos sirve como "columnas"
```

