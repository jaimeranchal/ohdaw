# Funciones Flecha
Es una **sintaxis abreviada** para escribir funciones, sustituyendo la palabra clave `function` por `=>` después de los parámetros:

```javascript
let suma = () => { // igual que: let function suma() {...
    return 5 + 7;
}

// Con argumentos
let suma2 = (a,b) => {
    return a + b;
}

// más breve aún
let suma3 = (a,b) => a + b;
```

