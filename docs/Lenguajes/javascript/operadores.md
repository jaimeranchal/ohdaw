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

## Concatenera Cadenas con `+` binario

El operador de suma (`+`) también se puede usar para unir cadenas, es decir, concatenar.

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
