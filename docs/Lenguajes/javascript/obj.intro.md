# Objetos en JavaScript
Un objeto es una _colección_ de:

- **propiedades** o valores
- **métodos** o funciones
- **eventos** o acciones

Excepto en el último punto es muy similar a cualquier otro lenguaje POO, como java.

!!! note "Acceso"
    Para acceder a una propiedad o método se usa el punto `.`:

    ```javascript
    Objeto.propiedad;
    Objeto.método([argumentos]);
    ```

!!! info "Jerarquía de Objetos en JavaScript"
    ![jerarquía](./obj.hierarchy.png)

Se puede crear un objeto vacío de **dos maneras**:

```javascript
let user = new Object(); // sintaxis de "Constructor"
let user = {};           // sintaxis de "Objeto Literal"
```
## Objetos literales

Un objeto _literal_ es una expresión que asigna a un nombre de variable un objeto, de forma parecida a lo que ocurría con las **expresiones de función**:

```javascript
let usuario = {
    nombre: "Jaime",
    edad: 34
};
```

### Acceso a propiedades
Para leer, modificar o eliminar propiedades de objetos:

```javascript
// leer e imprimir propiedades
alert( usuario.nombre ); // Jaime
alert( usuario.edad ); // 34

// añadir propiedades
usuario.esAdmin = true;

// eliminar
delete usuario.esAdmin;
```

### Propiedades calculadas
Podemos definir el nombre de una propiedad de forma general, esperando a que otra función lo _calcule_ después (por ejemplo, preguntando al usuario con un `prompt()`):


=== "Ejemplo"
    ```javascript
    let fruta = prompt("¿Qué fruta va a comprar?", "manzana");
    let bolsa = {
        [fruta]: 5,
    };

    // alternativa
    /*
    let bag = {} //objeto literal vacío
    bag[fruta] = 5; //añadimos la propiedad después
    */

    alert( bolsa.manzana ); // 5 si fruta = manzana
    ```
=== "Nombres de variable complejos"
    ```javascript
    // dentro de [] se pueden realizar operaciones
    let fruta = prompt("¿Qué fruta va a comprar?", "manzana");
    let bolsa = {
        [fruta + 'Mercadona']: 5, // bolsa.manzanaMercadona = 5
    };
    ```

### Abreviatura de propiedades
Si definimos una _función genérica_ es posible que usemos el mismo nombre para las propiedades y los parámetros. En ese caso se puede escribir de forma abreviada

=== "Versión abreviada"
    ```javascript
    function crearUsuario(nombre, edad) {
        return {
            nombre,
            edad,
            // ...otras propiedades
        };
    }
    ```

=== "Versión completa"
    ```javascript
    function crearUsuario(nombre, edad) {
        return {
            nombre : nombre,
            edad : edad,
            // ...otras propiedades
        };
    }

    let usuario = crearUsuario("Jaime", 34);
    alert(usuario.nombre); // Jaime
    ```

### Comprobar si existe una propiedad (operador "in")
JavaScript no controla si una propiedad existe cuando intentamos acceder a ella. Si no existe, simplemente devuelve _undefined_, pero ese no es un comportamiento siempre deseable.

Podemos comprobarlo comparando con el valor _undefined_, pero ¿qué pasa si la propiedad existe pero su valor _es undefined_? Para esos casos, y para mayor comodidad se puede usar el operador **in**:

```javascript
//Sintaxis
"key" in objeto
```

=== "Ejemplo"
    ```javascript
    let usuario = { nombre: "Jaime", edad: 34 };

    alert( "edad" in usuario ); // true
    alert( "blabla" in usuario ); // false
    ```
=== "Comprobación clásica"
    ```javascript
    let usuario = { nombre: "Jaime", edad: 34 };
    
    alert( usuario.altura === undefined ); // true: no existe
    ```

### Bucle "for...in"
Para recorrer todas las propiedades de un objeto realizando una operación, usamos un **tipo especial de bucle for**:

```javascript
let usuario = {
    nombre: "Jaime",
    edad: 34,
    esAdmin: true,
};

for (let key in usuario) {
    // propiedades
    alert(key);
    // valores
    alert(usuario[key]);
}
```

## Referenciar y copiar objetos
Cuando asignamos una variable a un objeto, lo que hacemos es crear una _referencia_ al mismo. Podemos crear tantas como queramos, pero todas apuntarán al **mismo objeto**; modifica una propiedad usando alguna de esas variables y quedará modificada "para todas las demás".

```javascript
let usuario = { nombre: "Juan" };
let admin = usuario;

admin.nombre = "Pablo";
alert(usuario.nombre); // "Pablo"
```

!!! note "Comparación por referencia"
    Dos objetos son iguales sólo si son el mismo objeto

    ```javascript
    let a = {};
    let b = a; // copio la referencia
    let c = {};

    alert( a == b ); // true, se refieren al mismo objeto
    alert( a === b ); // true
    alert( a == c ); // false
    ```

### Clonar y fusionar con Object.assign
Si realmente necesitamos crear un clon, una copia independiente de un objeto y una simple referencia extra, podemos hacerlo de dos maneras:

=== "Manual"
    ```javascript
    let usuario = {
        nombre: "Juan",
        edad: 30
    };

    let clon = {}; // objeto nuevo vacío

    // copiamos todas las propiedades en clon
    for (let key in usuario) {
        clon[key] = usuario[key];
    }

    // ahora clon es una copia independiente
    clon.nombre = "Pedro";
    alert( usuario.nombre ); // "Juan"
    ```
=== "Object.assign"
    ```javascript
    let usuario = { nombre: "Juan" };

    let permiso1 = { puedeVer: true };
    let permiso2 = { puedeEditar: true };

    // copia todas las propiedades de permiso1 y 2 a usuario
    Object.assign(usuario, permiso1, permiso2);

    // ahora usuario = { nombre: "Juan", puedeVer: true, puedeEditar: true }
    ```

    La sintaxis es:

    ```
    Object.assign(destino, [origen1, origen2, origen3...])
    ```

    Se puede usar como un sustituto de un bucle `for...in`:

    ```javascript
    let usuario = {
        nombre: "Juan",
        edad: 30
    };

    let clone = Object.assign({}, usuario);
    ```
    
    
