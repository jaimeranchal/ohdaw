# Clases en JavaScript

!!! info "¿Qué es una clase?"
    En POO, una clase es una **plantilla** de código extensible para crear objetos, que proporciona valores iniciales para sus estados (variables) e implementaciones de su comportamiento (funciones o métodos).

La sintaxis básica para crear clases es la siguiente:

```javascript
class MiClase {
    propiedad = valor; // campos de clase (no todos los navegadores la soportan)

    constructor(...) {
        // ...
    }

    metodo(...) {}

    // Getters y Setters
    get algo(...) {}
    set algo(...) {}

    //metodo con nombre calculado (simbolo)
    [simbolo.iterator]() {}

    // ...
}
```

