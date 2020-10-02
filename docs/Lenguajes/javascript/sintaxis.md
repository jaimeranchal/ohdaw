# Sintaxis básica

## Sentencias y comentarios

Las convenciones de escritura de código en JavaScript son muy similares a las de otros lenguajes de programación.

Un script se compone de varias partes, igual que cualquier otro programa:

- sentencias (statements)
- Comentarios

```javascript
"use strict";
// Comentario de una línea

/* Comentario de varias líneas:
    Muestra unos datos guardados en variable por consola
*/

// Variables
var nombre="María";
let edad=23;

// Una función
console.log(nombre);

// Estructuras de control
if (edad == 23) {
   var localidad = "Córdoba";
   console.log("Localidad IF=" + localidad); 
}
```

!!! advice "Final de sentencia implícito"
    Como en java, varias sentencias vienen separadas por un punto y coma (;); sin embargo, puede omitirse cuando hay un salto de línea, porque JS lo considera _implícito_.
    
    En consecuencia, un salto de línea no implica siempre un final de sentencia, por lo que es recomendable **usar siempre el punto y coma** para terminar cada sentencia

Veamos un ejemplo de posibles fallos:

Este código imprime un mensaje por cada posición de un array:

```javascript
[1,2].forEach(alert)
```

Si ponemos un mensaje de alerta antes del código y no lo terminamos...

```javascript
alert("Va a ocurrir un error")
[1,2].forEach(alert)
```

Efectivamente, después del mensaje de alerta, encontramos un error ¿por qué?

!!! warning "Cuidado"
    JavaScript no inserta automáticamente punto y coma después de un corchete cuadrado

```javascript
/* Este código sí que funciona */
alert("Ahora sí que sí");
[1,2].forEach(alert)
```

## Modo estricto

En 2009 apareció la versión ECMAScript5 (ES5) que introdujo modificaciones sustanciales en el lenguaje, haciéndolo incompatible con el código anterior a esa versión. Para preservar la retrocompatibilidad, la mayoría de estos cambios están desactivados por defecto. Para usar las nuevas características hay que habilitarlos usando la directiva `"use strict"`.

La directiva `"use strict"` (comillas simples o dobles) debe aparecer **encabezando el archivo**; en cualquier otra posición no tendrá ningún efecto.

```javascript
/* Sólo pueden aparecer comentarios por encima */
"use strict"
//Este código usa las funciones "modernas"
alert("some code");
"use strict" //esta sentencia se ignora

//el modo estricto NO está activado
```

!!! note "No se puede cancelar use strict"
    Una vez activado el motor no será compatible con versiones más antiguas

!!! info "Consola del desarrollador"
    Por defecto, la consola de desarrollador no usa el modo estricto; debemos activarlo:

    ```javascript
    'use strict'; /*<Shift+Intro para una nueva línea>*/
    //  ...your code
    /* <Intro para ejecutar> */

    /* Si el navegador es muy ANTIGUO */
    (function() {
      'use strict';
      // ...Tu código aquí...
    })()
    ```

### ¿Debo usar siempre el modo estricto?

**Respuesta corta**:
:   Sí, es una buena costumbre.

**Respuesta larga**:
:   No siempre. JS soporta clases y módulos que pueden habilitarlo de forma automática.
