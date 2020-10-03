# Comparaciones
Para comparar dos valores, se utilizan los signos mayor que (`>`), menor que (`<`), igual (`==`) y sus combinaciones (`<=`, `>=`). La desigualdad o no coincidencia se indica mediante `!=`.

El resultado de una comparación siempre es un booleano (true o false).

```javascript
alert( 2 > 1 );  // true (correct)
alert( 2 == 1 ); // false (wrong)
alert( 2 != 1 ); // true (correct)

// Se puede asignar (el resultado de) una comparación a una variable
let result = 5 > 4;
alert(result); // true
```

## Comparar cadenas

Cuando hablamos de comparar cadenas lo que en realidad solemos querer hacer son dos cosas:

- Ver si son iguales
- Ordenar una serie de elementos.

En el segundo caso, los programas suelen comparar el valor numérico de cada letra en la [tabla de caracteres Unicode](https://unicode-table.com/es/blocks/). Si quisiéramos ordenar una lista de nombres de la A a la Z, numéricamente se interpretaría como una lista de menor a mayor: la A equivale a un número más pequeño que la Z.

Estos son los pasos que sigue el algoritmo:

1. compara el primer carácter de ambas cadenas
1. si el primer carácter de la primera cadena es mayor( o menor), entonces la primera cadena es mayor que la segunda. Listos.
1. si el primer carácter de ambas cadenas es el mismo, repite el mismo proceso con el segundo carácter.
1. el proceso se repite hasta el final de alguna de las cadenas.
1. si ambas cadenas tienen la misma longitud, son iguales. En caso contrario, la más larga es mayor (va después).

```javascript
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

!!! Tip "Cuidado con el orden lexicográfico"
    La tabla de caracteres Unicode que se usa como referencia para traducir las letras a números tiene algunas importantes diferencias con el orden normal; las mayúsculas están antes que las minúsculas por ejemplo, y las vocales acentuadas (may. o min.) se situarían al final del todo de la lista ([aquí](https://javascript.info/string) se trata con un poco más de detalle)

## Compararando diferentes tipos

Para comparar datos de tipos distintos, JavaScript los convierte a números:

```javascript
alert( '2' > 1 ); // true, string '2' se convierte en 2
alert( '01' == 1 ); // true, string '01' se convierte en 1

//Con booleanos
alert( true == 1 ); // true
alert( false == 0 ); // true
```

!!! note "Una consecuencia divertida"
    Es posible que dos valores sean iguales, pero uno de ellos sea true como booleano, y el otro false.

    ```javascript
    let a = 0;
    alert( Boolean(a) ); // false

    let b = "0";
    alert( Boolean(b) ); // true

    alert(a == b); // true!
    ```

## Igualdad estricta (`===`)

El problema del operador de igualdad (`==`) es que no diferencia un 0 de un _false_, como se ve antes. La razón es la conversión a dato numérico que se realiza **antes** de la comparación.

```javascript
alert( 0 == false ); //true
alert( '' == false); //true
```

¿Y si queremos diferenciar entre tipos de datos? Usamos el operador de igualdad estrica (`===`).

```javascript
alert( 0 === false ); // false, because the types are different
```

## Valores _null_ y _undefined_

El comportamiento de estos dos valores (o tipos) es un poco **contraintuitivo**

```javascript
// ¿Igualdad, o no?
alert( null === undefined ); // false
alert( null == undefined ); // true
```

!!! note "Cosas extrañas"
    Con el resto de comparaciones, _null_ y _undefined_ se convierten a número, lo que da lugar a resultados un poco raros, matemáticamente hablando:

    ```javascript
    // Tipo 'null'
    alert( null > 0 );  // (1) false
    alert( null == 0 ); // (2) false
    alert( null >= 0 ); // (3) true (null solo es igual a undefined)

    // Tipo 'undefined'
    alert( undefined > 0 ); // false (1)
    alert( undefined < 0 ); // false (2)
    alert( undefined == 0 ); // false (3)
    ```

!!! Tip "Evita problemas"
    Evita siempre que puedas comparaciones con valores nulos o indefinidos
