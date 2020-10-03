# Operadores básicos

## De comparación

| Operador | Nombre               | Tipos | Resultado |
|----------|----------------------|-------|-----------|
| `==`     | Igualdad             | Todos | Booleano  |
| `!=`     | Desigualdad          | Todos | Booleano  |
| `===`    | Igualdad estricta    | Todos | Booleano  |
| `!==`    | Desigualdad estricta | Todos | Booleano  |
| `>`      | Mayor que            | Todos | Booleano  |
| `>=`     | Mayor o igual que    | Todos | Booleano  |
| `<`      | Menor que            | Todos | Booleano  |
| `<=`     | Menor o igual que    | Todos | Booleano  |

## Aritméticos

| Operador | Nombre         | Tipos                | Resultado            |
|----------|----------------|----------------------|----------------------|
| `+`      | Más            | Entero, real, cadena | Entero, real, cadena |
| `-`      | Menos          | Entero, real         | Entero, real         |
| `*`      | Multiplicación | Entero, real         | Entero, real         |
| `/`      | División       | Entero, real         | Entero, real         |
| `%`      | Módulo         | Entero, real         | Entero, real         |
| `++`     | Incremento     | Entero, real         | Entero, real         |
| `--`     | Decremento     | Entero, real         | Entero, real         |
| `+valor` | Positivo       | Entero, real, cadena | Entero, real, cadena |
| `-valor` | Negativo       | Entero, real, cadena | Entero, real, cadena |

!!! note "Uso de `++` y `--`"
    El operador incremento/decremento sólo se puede aplicar a variables. Si se lo aplicamos a un número (5++) arrojaría un error

    Estos operadores se pueden colocar antes o después del operando

    ```javascript
    let counter = 1;
    let a = ++counter; // (*)

    alert(a); // 2
    //----------------------------------------------
    let counter = 1;
    let a = counter++; // (*) changed ++counter to counter++

    alert(a); // 1
    ```

    - **Prepuesto** (++contador): Devuelve el valor de la variable actualizado. Útil si queremos usar el nuevo valor inmediatamente.
    - **Pospuesto** (contador++): Actualiza el valor pero devuelve el valor antiguo.


!!! note "Incremento y decremento con otros operadores"
    Se pueden combinar con otros operadores, pero tienen más precedencia que otros

    ```javascript
    let counter = 1;
    alert( 2 * ++counter ); // 4

    let counter = 1;
    alert( 2 * counter++ ); // 2, because counter++ returns the "old" value

    /* Lo anterior no es recomendable porque combina muchas operaciones en una sola línea */
    let counter = 1;
    alert( 2 * counter );
    counter++;
    ```
### Concatenar cadenas con `+` binario

El operador de suma (`+`) también se puede usar para unir cadenas, es decir, concatenar:

```javascript
let s = "my" + "string";
alert(s); // mystring
```

si alguno de los operandos es un string, el otro también se convierte a string

```javascript
alert( '1' + 2 ); // "12"
alert( 2 + '1' ); // "21"
alert(2 + 2 + '1' ); // "41" and not "221"
```

Los demás operadores, por el contrario, convierten otros tipos de datos a número

```javascript
alert( 6 - '2' ); // 4, converts '2' to a number
alert( '6' / '2' ); // 3, converts both operands to numbers
```

### Convertir a número con `+` unario

El operador `+` colocado delante de un dato lo convierte en un dato de tipo number.

```javascript
// Sin efecto en números
let x = 1;
alert( +x ); // 1

let y = -2;
alert( +y ); // -2

// Convierte datos no numéricos
alert( +true ); // 1
alert( +"" );   // 0
```

!!! note "Un caso práctico"
    Tenemos un formulario HTML que nos devuelve datos en forma de cadena, pero queremos sumarlos ¿cómo hacerlo?

    ```javascript
    //Un simple '+' concatenaría las cadenas
    let manzanas = "2";
    let naranjas = "3";

    alert( manzanas + naranjas ); // "23"

    //Primero tenemos que convertir los datos
    let manzanas = "2";
    let naranjas = "3";

    // Ambos valores se convierten en número poniendo un + delante
    alert( +manzanas + +naranjas ); // 5

    // Versión más larga
    // alert( Number(manzanas) + Number(naranjas) ); // 5
    ```
## De asignación

| Operador             | Nombre                        | Ejemplo       | Significado |
|----------------------|-------------------------------|---------------|-------------|
| `=`                  | Asignación                    | `x=y`         | `x=y`       |
| `+=, -=, *=, /=, %=` | Operación y Asignación        | `x+=y`        | `x=x+y`     |
| `<<=`                | Desplazar bits a la izquierda | `x<<=y`       | `x=x<<y`    |
| `>=, >>=, >>>=`      | Desplazar bits a la derecha   | `x>=y`        | `x=x>y`     |
| `&=`                 | AND bit a bit                 | `x&=y`        | `x=x&y`     |
| `                    | =`                            | OR bit a bit  | `x          | =y` | `x=x | y` |
| `^=`                 | XOR bit a bit                 | `x^=y`        | `x=x^y`     |
| `[]=`                | Desestructurar asignaciones   | `[a,b]=[c,d]` | `a=c, b=d`  |

!!! tip "No compliques el código"
    El hecho de que el signo = sea también un operador, implica que devuelve un valor (el resultado de la operación evaluada a la derecha o la asignación etc). En ocasiones se puede ver código como este en librerías, aunque lo suyo es **evitarlo**:

    ```javascript
    let a = 1;
    let b = 2;

    let c = 3 - (a = b + 1);

    alert( a ); // 3
    alert( c ); // 0
    ```

    Otra posibilidad que debemos no usar, si es posible es **encadenar asignaciones**

    ```javascript
    let a, b, c;

    a = b = c = 2 + 2;

    alert( a ); // 4
    alert( b ); // 4
    alert( c ); // 4
    ```

## Booleanos

| Operador | Nombre |
|----------|--------|
| `&&`     | AND    |
| `||`     | OR     |
| `!`      | NOT    |

!!! info "Orden de ejecución"

    - ...
    - 17  : suma _unaria_ `+`
    - 17  : negación _unaria_  `-`
    - 16  : potencia `**`
    - 15  : multiplicación `*`
    - 15  : división `/`
    - 13  : suma `+`
    - ...
    - 3   : asignación `=`
    - ... 

## Operadores de Objeto

| Operador     | nombre    | Ejemplo                                                                                                      | Uso                                                              |
|--------------|-----------|--------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| `.`          | Punto     | `Objeto.propiedad                                                                                            | método`                                                          | Acceso a propiedades o métodos |
| `[]`         | Corchetes | `#!javascript let provincias =[ “Cuenca”, “Toledo”, “Ciudad Real”]`                                          | Crear un array                                                   |
| `[]`         | Corchetes | `#!javascript provincias[1]=“Guadalajara”`                                                                   | Enumerar un elemento de un array                                 |
| `in:`        |           | `#!javascript “write” in document`                                                                           | Devuelve true si el objeto tiene la propiedad o método           |
| `instanceof` |           | `#!javascript aNumeros= new Array(1, 2, 3);`<br>`#!javascript aNumeros instanceof Array;   // Devuelve true` | Devuelve true si es una instancia de un objeto nativo Javascript |

## La coma

El operador coma (`,`) permite evaluar varias expresiones, aunque sólo devuelve la última.

Se usa para encadenar expresiones que se evalúan de izquierda a derecha; el ejemplo más claro lo tenemos en un bucle `for`: 

```javascript
for (let i=0, j=0; i<125; i++, j+10)
let a = (1 + 2, 3 + 4);
var nombre, dirección, apellidos
alert( a ); // 7 (El resultado de 3 + 4)
```

!!! note "Nota"
    La coma tiene una precedencia muy baja. La coma se evalúa después de la asignación, de modo que los paréntesis son importantes

A veces la gente los usa, y especialmente en los frameworks JavaScript, para crear construcciones más complejas que ejecutan varias acciones a la vez:

```javascript
// tres operaciones en una líneas
for (a = 1, b = 3, c = a * b; a < 10; a++) {...}
```

