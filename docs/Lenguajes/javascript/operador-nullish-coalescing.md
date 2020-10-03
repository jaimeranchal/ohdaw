# Operador de "unión nulosa" o _Nullish Coalescing_ (`??`)

Esta cosa de nombre tan raro simplemente crea un pequeño cortocirtuito donde, de dos valores dados, selecciona el que no sea nulo o indefinido.

```javascript
x = a ?? b // selecciona a si b no es 'null' o 'undefined' (y viceversa)

// equivale a
x = (a !== null && a !== undefined) ? a : b;
```

!!! warning "Reciente"
    Este operador es una adición reciente a la especificación de JavaScript, por lo que no todos los navegadores la soportan

El operador `??` tiene un valor de preferencia muy bajo, un poco por encima de `?` y `=`, pero por detrás de la multiplicación, p.e.

Está prohibido usarlo en combinación con `||` o `&&,` directamente. Con paréntesis, es posible:

```javascript
let x = 1 && 2 ?? 3; // Syntax error
let x = (1 && 2) ?? 3; // Works

alert(x); // 2
```

!!! note "Caso práctico: mostrar nombre de usuario"
    Imaginemos que tenemos tres campos: nombre, apellidos y apodo (todos opcionales); queremos mostrar alguno de ellos, si está relleno, o Anónimo si los 3 están vacíos

    ```javascript
    let firstName = null;
    let lastName = null;
    let nickName = "Supercoder";

    // Muestra el primer valor no nulo o no definido aún
    alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
    ```
