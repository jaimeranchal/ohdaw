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
