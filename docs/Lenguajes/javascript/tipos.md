# Tipos de datos

Hay 8 tipos de datos básicos:

- _number_: números de cualquier clase, dentro del rango ±253
- _bigint_: enteros de longitud aleatoria (y superando los límites de number)
- _string_: cadenas y caracteres.
- _boolean_
- _null_: valores desconocidos; tipo único.
- _undefined_: valor inicial de cualquier variable sin asignar (sólo declarada).
- _object_: objetos y estructuras de datos complejas
- _symbol_: identificadores únicos

JS utiliza un sistema de **asignación dinámica** de tipos de datos, de forma que la misma variable puede contener diferentes tipos de datos a lo largo de una ejecución. Por esa misma razón no hace falta declarar el tipo al crear las variables.

!!! note "Función `typeof()`"
    Para ver el tipo de dato de una variable podemos usar el operador typeof(x):

    - Devuelve una cadena
    - por errores heredados, devuelve object cuando el tipo es null.

## Tipo _number_

El tipo de dato number incluye:

- **enteros**
- **coma flotante**: con punto (12.345 en lugar de 12,345)
- _infinito_ (`Infinity`)
- `NaN` (_Not a Number_): representa un error computacional, resultado de una operación incorrecta o no definida. Se propaga al resto de operaciones derivadas de aquella que lo produzca.

!!! info "Operaciones matemáticas seguras"

    JS no se va a quedar colgado si realizamos una operación 'imposible', como dividir por cero o usar cadenas en operaciones matemáticas; simplemente, obtendremos bien _Infinity_ o _NaN_ como resultado, pero la aplicación no se interrumpirá.

## Tipo _BigInt_
Se creó para casos en los que fuera necesario trabajar con números más allá de los límites para el tipo number. Más información [aquí](https://javascript.info/bigint)

!!! warning "Soporte"
    Ahora mismo solo Firefox, Chrome y Edge soportan BigInt

## Tipo _String_

Hay tres formas de asignar un valor de tipo string:

```javascript
let str = "Hola";
let str2 = 'También valen comillas simples';
let frase = `permite insertar otra variable como ${str}`;
```

El operador `${...}` permite insertar una variable, una expresión aritmética o cosas más complejas

```javascript
let name = "Jaime";
alert( `Hola, ${name}`); //Hola, Jaime

alert( `El resultado es ${1+2}`) //El resultado es 3
//si no usamos `backticks`, la cadena se interpreta de modo literal
alert( "El resultado es ${1+2}") //El resultado es ${1+2}
```

## Conversiones

En JS los operadores y las funciones se encargan de convertir los valores que se les pasan al tipo correcto; `alert()`, p.e. convierte cualquier valor a cadena y lo muestra. Los operadores matemáticos hacen otro tanto.

Las conversiones más habituales son: a **string**, a **número** y a **booleano**.

Hay varias formas de convertir entre tipos de datos:

### Por unión de tipos diferentes

- Entero + Float = Float
- Número + String = String

### Mediante funciones

- `String(valor)`
- `Number(Valor)`:
    - _undefined_ se convierte en _NaN_.
    - _null_ se convierte en `0`.
    - _true_ y _false_ se convierten en `1` y `0`.
    - Una cadena:
        - vacía, a `0`.
        - sin números, a _NaN_.
- `Boolean(valor)`:
    - `0`, _null_, _undefined_, _NaN_, y una cadena vacía (`""`) se convierten en _false_.
    - Todo lo demás, a _true_

```javascript
// String()--------------------
let value = true;
alert(typeof value); // boolean

value = String(value); // now value is a string "true"
alert(typeof value); // string

// Number()--------------------
alert( "6" / "2" ); // 3, strings are converted to numbers

let str = "123";
alert(typeof str); // string
let num = Number(str); // becomes a number 123
alert(typeof num); // number

let age = Number("an arbitrary string instead of a number");
alert(age); // NaN, conversion failed

alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN (error reading a number at "z")
alert( Number(true) );        // 1
alert( Number(false) );       // 0

// Boolean()--------------------
alert( Boolean(1) ); // true
alert( Boolean(0) ); // false

alert( Boolean("hello") ); // true
alert( Boolean("") ); // false

alert( Boolean("0") ); // true
alert( Boolean(" ") ); // spaces, also true (any non-empty string is true)
```

!!! Tip "De cadena a número"
    El método `ParseInt(string, base)` es más potente y da mejores resultados que `Number()`. La función parseInt comprueba el primer argumento, una cadena, e intenta devolver un entero de la base especificada. Por ejemplo, una base de 10 indica una conversión a número decimal, 8 octal, 16 hexadecimal, y así sucesivamente. 

    Si parseInt encuentra un carácter que no es un numeral de la base especificada, lo ignora a él y a todos los caracteres correctos siguientes, devolviendo el valor entero obtenido hasta ese punto. parseInt trunca los números en valores enteros. Se permiten espacios anteriores y posteriores.

    Si no se especifica la base o se especifica como 0, JavaScript asume lo siguiente:

    - Si el parámetro cadena comienza por "0x", la base es 16 (hexadecimal).
    - Si el parámetro cadena comienza por "0", la base es de 8 (octal). Esta característica está desaconsejada.
    - Si el parámetro cadena comienza por cualquier otro valor, la base es 10 (decimal).
    - Si el primer carácter no se puede convertir en número, parseInt devuelve NaN

    ```javascript
    // Todos estos ejemplos dan 15
    parseInt("F", 16);
    parseInt("17", 8);
    parseInt("15", 10);
    parseInt(15.99, 10);
    parseInt("FXX123", 16);
    parseInt("1111", 2);
    parseInt("15*3", 10);
    parseInt("12", 13);
    ```
    
