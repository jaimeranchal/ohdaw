# Funciones _callback_

Tomemos como ejemplo una función que acepta tres parámetros:

1. **pregunta**: el texto de la pregunta
2. **si**: una _función_ que se ejecuta si la respuesta es sí
3. **no**: una _función_ que se ejecuta si la respuesta es no.

La función debe mostrar la pregunta y llamar a `si()` o `no()` según sea la respuesta:

```javascript
function preguntar(pregunta, si, no) {
    if (confirm(pregunta)) si()
    else no();
}

function showOk() {
    alert( "Has dicho que sí." );
}

function showCancel() {
    alert( "Has cancelado la ejecución." );
}

// Uso
preguntar("¿Estás de acuerdo?", showOk, showCancel);
```

**Los parámetros `showOk` y `showCancel` se llaman funciones de rellamada (_callback functions_) o simplemente _callbacks_.**

La idea es pasar una función y esperar a que le "devuelvan la llamada" en otro momento si hace falta. En el ejemplo, `showOk` es el _callback_ para la respuesta "sí", y `showCancel`para un "no".

!!! info "Abreviar código con funciones _anónimas_"
    ```javascript
    function preguntar(pregunta, si, no) {
        if (confirm(pregunta)) si()
        else no();
    }

    preguntar(
        "¿Estás de acuerdo?",
        function() {alert("Has dicho que sí.");},
        function() {alert("Has cancelado la ejecución.");}
    );
    ```

    Las funciones dentro del `preguntar(...)` no tienen nombre y por eso se llaman _anónimas_; no son accesibles fuera de esa declaración.
