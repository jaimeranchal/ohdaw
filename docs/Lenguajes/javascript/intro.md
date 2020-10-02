# JavaScript
Dándole funcionalidad a la web

!!! summary "Contenidos"
    1. [Fundamentos](./fundamentos.md)
    2. Calidad de código
    2. Objetos
    2. Uso avanzado de funciones
    2. Prototipos y Herencia
    2. Clases
    2. Manejo de errores
    2. Promises, async/await
    2. Generadores e iteración avanzada
    2. Módulos

## Qué es JavaScript
Javascript se creó con la intención de darle vida a las páginas web. Los programas escritos en este lenguaje se denomina scripts y se pueden insertar en el código html de una página para que se ejecuten automáticamente cuando cargue.

Los scripts se escriben y ejecutan como archivos de texto plano; no necesitan preparación especial o ser compilados.

!!! info "¿Por qué se le llama JavaScript?"
    Su nombre original era _LiveScript_, pero debido a la popularidad de java en aquel momento se consideró mejor posicionarlo como un hermano menor.

    JavaScript es un lenguaje independiente con su propia especificación, [ECMAScript](http://es6-features.org/) y no tiene ninguna relación con Java

    La versión del estándar más importante es ECMAScript 6, lanzada en junio de 2015.

JavaScript se puede ejecutar en el **navegador** y también en un **servidor**, o cualquier programa que tenga un _motor JavaScript_. El navegador incorpora uno de fábrica, a veces llamado JavaScript virtual machine.

Diferentes motores tienen diferentes "nombres en clave", p.ej:

- V8 (Chrome y Opera)
- SpiderMonkey (Firefox)
- Otros: "Trident" y "Chakra" para dif. versiones de IE, o "ChakraCore" para Microsoft Edge; "Nitro" y "SquirrelFish" para Safari, etc.

!!! tip "Consejo"
    Es bueno recordar estos nombres como referencia cuando busquemos artículos o información para desarrolladores. Si leemos que 'V8 soporta la característica x' seguramente funcione en Chrome y Opera

## Para qué sirve

JavaScript es un lenguaje del lado del _cliente_. No permite acceder a la memoria del ordenador ni a la CPU. Su actividad se limita al navegador donde se ejecuta; aquí puede hacer todo lo que tenga que ver con **manipulación de la web**, **interacción con el usuario** y, gracias a herramientas como [node.js](https://wikipedia.org/wiki/Node.js) con el **servidor**:

- Añadir nuevo código HTML a la página, cambiar el contenido, modificar los estilos.
- Reaccionar a las acciones del usuario, ejecutarse al hacer click, mover el puntero o presionar una tecla.
- Enviar peticiones a servidores remotos, descargar y subir archivos (tecnologías [AJAX](https://en.wikipedia.org/wiki/Ajax_(programming)) y [COMET](https://en.wikipedia.org/wiki/Comet_(programming))).
- Obtener y fijar cookies, preguntar al visitante, mostrar mensajes
- Recordar los datos en lado del cliente (almacenamiento local)

### ¿Qué NO PUEDE hacer JavaScript en un navegador?
Para impedir accesos no deseados a información privada, existen varias restricciones:

- En una web, JS no puede leer/escribir archivos en el disco duro, copiarlos o ejecutar programas. No tiene acceso directo a funciones del SO.
- Los navegadores recientes permiten que JS trabaje con archivos, con acceso limitado y solo si el usuario ejecuta alguna acción, como "soltar" un archivo en el navegador o seleccionarlo mediante un <input>.
- También puede interactuar con la cámara y el micrófono, si el usuario da su consentimiento.
- Normalmente las pestañas / ventanas son **independientes** (no son visibles entre sí).

!!! note "Nota"
    A veces no, p.ej. cuando una ventana usa JS para abrir otra; incluso en ese caso, JS no puede leer datos de un sitio web diferente (dominio, protocolo o puerto) [Same Origin Policy]


- JavaScript puede comunicarse a través de la red con el servidor donde está alojada la página actual, pero requiere **permiso explícito** (en las cabeceras HTTP) del lado remoto.

!!! info
    Estas limitaciones no existen cuando se utiliza JS fuera del navegador, como en un servidor p.e.

## ¿Cómo funciona?

Sin entrar en detalles:

1. El motor (embebido si se trata de un navegador) analiza (parse) el script.
1. Lueco lo convierte (compila) al lenguaje de la máquina.
1. Finalmente la máquina ejecuta el ćodigo.

El motor va optimizando el proceso en cada paso. También observa el código mientras se ejecuta, analizando el flujo de datos de entrada, optimizando el código de la máquina aprovechando esa información.

## La "familia" JavaScript

Como la sintaxis de JS no puede responder a las necesidades de todos, desde hace unos años han aparecido algunos lenguajes que son "traducidos" o _transpilados_ a JavaScript antes de que se ejecute en el navegador, y que añaden diferentes características.

La mayoría de aplicaciones actuales ejecutan este proceso de traducción sin que el usuario lo vea.

Ejemplos:

- [CoffeeScript](https://coffeescript.org): "endulza" un poco JS, con una sintaxis más breve; suele gustarle a los devs. de Ruby.
- [TypeScript](https://www.typescriptlang.org): se centra en añadir **declaración estricta de tipos de datos** para simplificar el desarrollo y dar soporte a sistemas complejos. Desarrollado por Microsoft.
- [Flow](https://flow.org): hace lo mismo que TypeScript, pero desarrollado por Facebook.
- [Dart](https://www.dartlang.org): desarrollado por Google, es un lenguaje independiente con su propio motor que corre en entornos diferentes al navegador (aplicaciones móviles, p.ej.); también se puede transpilar a JS.


