# jQuery

JQuery es una librería JavaScript que simplifica la forma de programar: _escribe menos, haz más_. Para conseguirlo proporciona métodos muy simples para ejecutar lo que en JS serían bastantes líneas de código.

Es especialmente útil para manejar **llamadas AJAX** y **manipular el DOM**. De forma general, permite:

- Manipular el DOM/HTML
- Manipular CSS
- Métodos para eventos HTML
- Efectos y animaciones
- AJAX
- Utilidades

También se puede amplicar su funcionalidad con **plugins**.

## Añadir jQuery a tu página

=== "Descarga"
    Hay dos **versiones** disponibles desde [jQuery.com](http://jquery.com/download/):

    - _Producción_: minimizada y comprimida; la recomendada.
    - _Desarrollo_: para pruebas y desarrollo (descomprimida, para poder leer el código)

    Una vez descargada, sólo hay que añadir la referencia:

    ```html
    <head>
        <script scr="./jquery-3.5.1.min.js"></script>
    </head>
    ```
=== "Enlace"
    Desde un CDN (_Content Delivery Network_), siempre y cuando no esté caído:

    ```html
    <head>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    </head>
    ```

    Probablemente esta última opción sea más **rápida**; mucha gente ya tiene descargada la librería por haber visitado otras páginas web.

## Sintaxis

El mecanismo básico es **seleccionar** (_query_) elementos HTML para realizar **acciones** sobre ellos:

- `$` define y da acceso a jQuery
- `selector`: cadena de búsqueda de elementos
- `action()`: el método que queremos ejecutar

```javascript
$(selector).accion();

$(this).hide()    // oculta el elemento actual
$("p").hide()     // oculta todos los elementos `p`
$(".test").hide() // oculta todos los elementos con la clase `test`
$("#test").hide() // oculta todos los elementos con la id `test`
```

### El evento _Document Ready_

Para impedir que el código jQuery se ejecute _antes_ de que el documento termine de cargarse, los métodos se insertan dentro de una función:

=== "Document.ready"
    ```javascript
    $(document).ready(function(){

      // métodos jQuery 

    });
    ```
=== "Mínimo"
    ```javascript
    $(function(){

      // métodos jQuery 

    });
    ```

### Selectores

Sirven para encontrar elementos HTML. 

- Se pueden usar todos los **selectores CSS**, más alguno particular de jQuery.
- Todos los selectores empiezan con el **signo del dólar** y los **paréntesis** `$()`

=== "css"
    ```javascript
    $('*')        // todos los elementos
    $('p')        // todos los <p>
    $('p', 'a')   // todos los <p> y todos los <a>
    $('p a')      // todos los <a> hijos de <p>
    $('li.clase') // todos los <li> con la clase indicada
    $('li>p')     // todos los <p> hijos directos de <li>
    $('h1+p')     // todos los <p> inmediatamente por un <h1> hermano
    $('p:has(b)') // todos los <p> que contienen un <b>
    $('.clase')   // todos los elementos con la clase x
    $('#id')      // todos los elementos con esa id
    $('p:empty')  // todos los <p> vacíos
    $('img[alt]') // todos los <img> con atributo "alt"
    $(':button[id*=boton]') // todos los <button> cuyo "id" sea 'boton'
    $('a[href$=.pdf]') // todos los <a> cuyo "href" acabe en 'pdf'
    $('a[title^=.lr]') // todos los <a> cuyo "title" empiece por '.lr'
    ```
=== "jquery"
    ```javascript
    $('p:first')            // primer <p>
    $('p:last')             // último <p>
    $('li:first-child')     // todos los <li> primeros hijos
    $('li:only-child')      // todos los <li> hijos únicos
    $('li:nth-child(3)')    // todos los <li> terceros hijos
    $('tr:nth-child(odd)')  // todos los <tr> impares
    $('tr:nth-child(even)') // todos los <tr> pares
    $('p:odd')              // <p> impares
    $('p:eq(1)')            // segundo <p> (asumiendo que el primero es 0)
    $('p:gt(1)')            // todos los <p> a partir de la 2 posicion
    $('p:lt(1)')            // todos los <p> antes de las 2 posicion
    $('p:contains(texto)')  // todos los <p> con 'texto'
    ```
=== "formulario"
    ```javascript
    $('input') // todos los input
    // todos los input con el atributo indicado tras los dos puntos
    $(':text')
    $(':checkbox')
    $(':button')
    ...
    $(':enabled') // todos los elementos activados
    $(':disabled') // todos los elementos desactivados
    $(':checked') // ¿está seleccionado? (radiobutton + checkbox)
        $('objeto').is(':checked') //booleano
        $(':radio:checked').val() // valor del radio seleccionado
    $(':selected') // ¿está seleccionado? (option)
        $('select option:selected') // elementos selecc.
        $('select option:selected').text() // valor de la opción selecc.
        $('select option:selected').prop("value") // valor
    ```

### Funciones

=== "Filtros"
    `not()`
    :   Elementos que **no cumplen** los criterios
        ```javascript
       `$('div').not('.verde, #azul').css('border-color', 'red')`
        ```

    `filter()`
    :   filtra o busca una selección de elementos y la reduce alos que cumplen un filtro adicional
        ```javascript
        $('div').filter(".div1").css('background-color', 'blue');
        ```
    `slice(inicio, fin)`
    :   Recorta una selección de elementos según su posición
        ```javascript
        $('p').slice(2,3);
        // selecciona todos los <p> y filtra sólo el tercer párrafo
        ```

    `find()`
    :   Busca una selección de elementos y la reduce a los que cumplen la condición
        ```javascript
        $('div').find(".div1").css('background-color', 'blue');
        ```
=== "Añadir contenido"
=== "Eliminar"
=== "Atributos"
=== "Atributos"
=== "css"
=== "Bucles"

#### Bucles automáticos

Cuando se aplica una función jQuery a una **selección de elementos** no hace falta generar un bucle; se aplica automáticamente

```javascript
$('.misBotones').hide();
// oculta TODOS los botones con la clase ".misBotones"
```

#### Encadenar funciones

jQuery permite _apilar_ funciones una detrás de otra, que se ejecutarán en sucesión a la selección

```javascript
$('.misBotones').width(300).heigth(300).text('Hola').fadeIn(1000);
//  En este ejemplo se selecciona todos los elementos cuya clase es ‘misBotones’ y automáticamente se le aplican cuatro funciones.

// Otra forma:
$('.misBotones').width(300)
.heigth(300)
.text('Hola')
.fadeIn(1000);
```

### Eventos

jQuery crea sus propios **métodos** para los eventos del DOM

```javascript
$("p").click(); // asigna un evento 'click' a un párrafo

$("p").click(){
    // acción a realizar cuando hagas click en el párrafo
};
```

#### Comunes

`$(document).ready()`
:   El documento ha terminado de cargar
`on()`
:   Click simple sobre un elemento ( igual que `onclick` )
`dbclick()`
:   Doble clic
`mouseenter()`
:   El puntero _entra_ en un elemento HTML
`mouseleave()`
:   El puntero _sale_ de un elemento HTML
`mousedown()`
:   Cuando se presiona un botón del ratón, _sobre_ un elemento
`mouseup()`
:   Cuando se libera un botón presionado del ratón, _sobre_ un elemento
`hover()`
:   Ejecuta **dos funciones**: al _entrar_ en un elemento, y al _salir_
`focus()`
:   Cuando un elemento **input** obtiene el foco
`blur()`
:   Cuando un elemento **input** pierde el foco

#### Lista completa

| Method / Property                                                                                                 | Description                                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [bind()](https://www.w3schools.com/jquery/event_bind.asp)                                                         | Deprecated in version 3.0. Use the [on()](https://www.w3schools.com/jquery/event_on.asp) method instead. Attaches event handlers to elements                                                              |
| [blur()](https://www.w3schools.com/jquery/event_blur.asp)                                                         | Attaches/Triggers the blur event                                                                                                                                                                           |
| [change()](https://www.w3schools.com/jquery/event_change.asp)                                                     | Attaches/Triggers the change event                                                                                                                                                                         |
| [click()](https://www.w3schools.com/jquery/event_click.asp)                                                       | Attaches/Triggers the click event                                                                                                                                                                          |
| [dblclick()](https://www.w3schools.com/jquery/event_dblclick.asp)                                                 | Attaches/Triggers the double click event                                                                                                                                                                   |
| [delegate()](https://www.w3schools.com/jquery/event_delegate.asp)                                                 | Deprecated in version 3.0. Use the [on()](https://www.w3schools.com/jquery/event_on.asp) method instead. Attaches a handler to current, or future, specified child elements of the matching elements |
| [die()](https://www.w3schools.com/jquery/event_die.asp)                                                           | Removed in version 1.9. Removes all event handlers added with the live() method                                                                                                                            |
| [error()](https://www.w3schools.com/jquery/event_error.asp)                                                       | Removed in version 3.0. Attaches/Triggers the error event                                                                                                                                                  |
| [event.currentTarget](https://www.w3schools.com/jquery/event_currenttarget.asp)                                   | The current DOM element within the event bubbling phase                                                                                                                                                    |
| [event.data](https://www.w3schools.com/jquery/event_data.asp)                                                     | Contains the optional data passed to an event method when the current executing handler is bound                                                                                                           |
| [event.delegateTarget](https://www.w3schools.com/jquery/event_delegatetarget.asp)                                 | Returns the element where the currently-called jQuery event handler was attached                                                                                                                           |
| [event.isDefaultPrevented()](https://www.w3schools.com/jquery/event_isdefaultprevented.asp)                       | Returns whether event.preventDefault() was called for the event object                                                                                                                                     |
| [event.isImmediatePropagationStopped()](https://www.w3schools.com/jquery/event_isimmediatepropagationstopped.asp) | Returns whether event.stopImmediatePropagation() was called for the event object                                                                                                                           |
| [event.isPropagationStopped()](https://www.w3schools.com/jquery/event_ispropagationstopped.asp)                   | Returns whether event.stopPropagation() was called for the event object                                                                                                                                    |
| [event.namespace](https://www.w3schools.com/jquery/event_namespace.asp)                                           | Returns the namespace specified when the event was triggered                                                                                                                                               |
| [event.pageX](https://www.w3schools.com/jquery/event_pagex.asp)                                                   | Returns the mouse position relative to the left edge of the document                                                                                                                                       |
| [event.pageY](https://www.w3schools.com/jquery/event_pagey.asp)                                                   | Returns the mouse position relative to the top edge of the document                                                                                                                                        |
| [event.preventDefault()](https://www.w3schools.com/jquery/event_preventdefault.asp)                               | Prevents the default action of the event                                                                                                                                                                   |
| [event.relatedTarget](https://www.w3schools.com/jquery/event_relatedtarget.asp)                                   | Returns which element being entered or exited on mouse movement                                                                                                                                            |
| [event.result](https://www.w3schools.com/jquery/event_result.asp)                                                 | Contains the last/previous value returned by an event handler triggered by the specified event                                                                                                             |
| [event.stopImmediatePropagation()](https://www.w3schools.com/jquery/event_stopimmediatepropagation.asp)           | Prevents other event handlers from being called                                                                                                                                                            |
| [event.stopPropagation()](https://www.w3schools.com/jquery/event_stoppropagation.asp)                             | Prevents the event from bubbling up the DOM tree, preventing any parent handlers from being notified of the event                                                                                          |
| [event.target](https://www.w3schools.com/jquery/event_target.asp)                                                 | Returns which DOM element triggered the event                                                                                                                                                              |
| [event.timeStamp](https://www.w3schools.com/jquery/event_timestamp.asp)                                           | Returns the number of milliseconds since January 1, 1970, when the event is triggered                                                                                                                      |
| [event.type](https://www.w3schools.com/jquery/event_type.asp)                                                     | Returns which event type was triggered                                                                                                                                                                     |
| [event.which](https://www.w3schools.com/jquery/event_which.asp)                                                   | Returns which keyboard key or mouse button was pressed for the event                                                                                                                                       |
| [focus()](https://www.w3schools.com/jquery/event_focus.asp)                                                       | Attaches/Triggers the focus event                                                                                                                                                                          |
| [focusin()](https://www.w3schools.com/jquery/event_focusin.asp)                                                   | Attaches an event handler to the focusin event                                                                                                                                                             |
| [focusout()](https://www.w3schools.com/jquery/event_focusout.asp)                                                 | Attaches an event handler to the focusout event                                                                                                                                                            |
| [hover()](https://www.w3schools.com/jquery/event_hover.asp)                                                       | Attaches two event handlers to the hover event                                                                                                                                                             |
| [keydown()](https://www.w3schools.com/jquery/event_keydown.asp)                                                   | Attaches/Triggers the keydown event                                                                                                                                                                        |
| [keypress()](https://www.w3schools.com/jquery/event_keypress.asp)                                                 | Attaches/Triggers the keypress event                                                                                                                                                                       |
| [keyup()](https://www.w3schools.com/jquery/event_keyup.asp)                                                       | Attaches/Triggers the keyup event                                                                                                                                                                          |
| [live()](https://www.w3schools.com/jquery/event_live.asp)                                                         | Removed in version 1.9. Adds one or more event handlers to current, or future, selected elements                                                                                                           |
| [load()](https://www.w3schools.com/jquery/event_load.asp)                                                         | Removed in version 3.0. Attaches an event handler to the load event                                                                                                                                        |
| [mousedown()](https://www.w3schools.com/jquery/event_mousedown.asp)                                               | Attaches/Triggers the mousedown event                                                                                                                                                                      |
| [mouseenter()](https://www.w3schools.com/jquery/event_mouseenter.asp)                                             | Attaches/Triggers the mouseenter event                                                                                                                                                                     |
| [mouseleave()](https://www.w3schools.com/jquery/event_mouseleave.asp)                                             | Attaches/Triggers the mouseleave event                                                                                                                                                                     |
| [mousemove()](https://www.w3schools.com/jquery/event_mousemove.asp)                                               | Attaches/Triggers the mousemove event                                                                                                                                                                      |
| [mouseout()](https://www.w3schools.com/jquery/event_mouseout.asp)                                                 | Attaches/Triggers the mouseout event                                                                                                                                                                       |
| [mouseover()](https://www.w3schools.com/jquery/event_mouseover.asp)                                               | Attaches/Triggers the mouseover event                                                                                                                                                                      |
| [mouseup()](https://www.w3schools.com/jquery/event_mouseup.asp)                                                   | Attaches/Triggers the mouseup event                                                                                                                                                                        |
| [off()](https://www.w3schools.com/jquery/event_off.asp)                                                           | Removes event handlers attached with the on() method                                                                                                                                                       |
| [on()](https://www.w3schools.com/jquery/event_on.asp)                                                             | Attaches event handlers to elements                                                                                                                                                                        |
| [one()](https://www.w3schools.com/jquery/event_one.asp)                                                           | Adds one or more event handlers to selected elements. This handler can only be triggered once per element                                                                                                  |
| [$.proxy()](https://www.w3schools.com/jquery/event_proxy.asp)                                                     | Takes an existing function and returns a new one with a particular context                                                                                                                                 |
| [ready()](https://www.w3schools.com/jquery/event_ready.asp)                                                       | Specifies a function to execute when the DOM is fully loaded                                                                                                                                               |
| [resize()](https://www.w3schools.com/jquery/event_resize.asp)                                                     | Attaches/Triggers the resize event                                                                                                                                                                         |
| [scroll()](https://www.w3schools.com/jquery/event_scroll.asp)                                                     | Attaches/Triggers the scroll event                                                                                                                                                                         |
| [select()](https://www.w3schools.com/jquery/event_select.asp)                                                     | Attaches/Triggers the select event                                                                                                                                                                         |
| [submit()](https://www.w3schools.com/jquery/event_submit.asp)                                                     | Attaches/Triggers the submit event                                                                                                                                                                         |
| [toggle()](https://www.w3schools.com/jquery/event_toggle.asp)                                                     | Removed in version 1.9. Attaches two or more functions to toggle between for the click event                                                                                                               |
| [trigger()](https://www.w3schools.com/jquery/event_trigger.asp)                                                   | Triggers all events bound to the selected elements                                                                                                                                                         |
| [triggerHandler()](https://www.w3schools.com/jquery/event_triggerhandler.asp)                                     | Triggers all functions bound to a specified event for the selected elements                                                                                                                                |
| [unbind()](https://www.w3schools.com/jquery/event_unbind.asp)                                                     | Deprecated in version 3.0. Use the [off()](https://www.w3schools.com/jquery/event_off.asp) method instead. Removes an added event handler from selected elements                                        |
| [undelegate()](https://www.w3schools.com/jquery/event_undelegate.asp)                                             | Deprecated in version 3.0. Use the [off()](https://www.w3schools.com/jquery/event_off.asp) method instead. Removes an event handler to selected elements, now or in the future                            |
| [unload()](https://www.w3schools.com/jquery/event_unload.asp)                                                     | Removed in version 3.0. Attaches an event handler to the unload event                                                                                                                                      |


