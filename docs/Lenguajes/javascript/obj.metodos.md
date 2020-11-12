# Métodos para Objetos
Además de propiedades, los objetos definen acciones posibles:

=== "Declaración separada"
    ```javascript
    let usuario = {
        nombre: "Jaime",
        edad: 30
    };

    usuario.diHola = function() {
        alert("Hola!");
    };

    usuario.diHola(); // Hola!
    ```
=== "Dentro del objeto"
    ```javascript
    // Dos opciones, la larga:
    let usuario = {
        diHola: function() {
            alert("Hola!");
        }
    };

    // la versión corta
    let usuario = {
        diHola() {
            alert("Hola!");
        }
    };

    ```
    
## Operador this
Si queremos acceder a otras propiedades o métodos del mismo objeto en el que estamos, podemos usar el **operador `this`**:

```javascript
let usuario = {
    nombre: "Jaime",
    edad: 30

    diHola() {
        alert(this.nombre);
    }
};

usuario.diHola(); // "Jaime"
```

El valor concreto del operador `this` se evalúa cada vez que se invoca, de modo que nos permite programar de manera genérica un objeto para que funcione independientemente del nombre con el que lo llamemos.

!!! important "No hay operador `this` en funciones flecha"
    Si lo usamos dentro de una función flecha, tomará el valor del contexto externo:

    ```javascript
    let usuario = {
        nombre: "Jaime",
        edad: 30

        diHola() {
            let flecha = () => alert(this.nombre);
            flecha();
        }
    };

    usuario.diHola(); // "Jaime"
    ```

## Constructor con operador new
Los objetos literales (`{...}`) son cómodos para crear _un único objeto_, pero no cuando necesitamos crear varios parecidos, como varios usuarios u objetos de menú.

Para eso usamos el **operador `new`**. Dos reglas:

1. Su nombre empieza por **mayúscula**
2. Sólo se ejecutan con el operador `new`.

```javascript
function Usuario(nombre) {
    this.nombre = nombre;
    this.esAdmin = false;
}

let usuario = new Usuario("Jack");

alert(usuario.nombre); // Jack
alert(usuario.esAdmin); // false
```


