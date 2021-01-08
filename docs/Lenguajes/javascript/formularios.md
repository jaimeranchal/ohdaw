# Formularios en JavaScript

## El objeto _Form_

Además de modificarlos como cualquier otro elemento del DOM, JavaScript permite recuperar los valores de un formulario y **validar** las entradas del usuario directamente en el lado del cliente.

Para acceder a un formulario usamos el **objeto** _Form_, que a su vez es una propiedad del objeto _Document_.

=== "Seleccionar un formulario"
    ```javascript
    /* Método 1 */
    let formulario = document.getElementById("contacto");

    /* Método 2 */
    let formularios = document.getElementsByTagName("form"); //array de forms
    let formulario1 = formularios[0]; // primer formulario
    // variante en una línea
    let formulario = document.getElementsByTagName("form")[0];

    // Filtrar por elemento padre (menú lateral p.e.)
    let menu = document.getElementById("menulateral");
    let formularios = menu.getElementsByTagName("form");
    let formulario1 = formularios[0];

    /* Método 3 */
    // usando la propiedad 'forms' del objeto document
    let formularios = document.forms;
    let formulario = formularios[0]; // ref. por índice
    let formulario = formularios["contacto"]; // ref. por atributo 'name'
    ```

### Propiedades y métodos generales

=== "Propiedades"
    | Nombre          | Descripción                                                  |
    |-----------------|--------------------------------------------------------------|
    | `acceptCharset` | Ajusta o devuelve el atributo `accept-charset`               |
    | `action`        | Ajusta o devuelve el atributo `action`                       |
    | `enctype`       | Ajusta o devuelve el atributo `enctype`                      |
    | `length`        | Número de elementos del formulario                           |
    | `method`        | Ajusta o devuelve el atributo `method`                       |
    | `name`          | Ajusta o devuelve el atributo `name`                         |
    | `target`        | Ajusta o devuelve el atributo `target`                       |
    | `elements`      | Array con todos los objetos `input` en el orden de aparición |

    !!! hint "Borrar valores de un formulario"
        Con la propiedad `elements[]` y varios campos de tipo texto se puede hacer un bucle que los ponga en blanco:

        ```javascript
        let formulario = document.getElementById("contacto");

        if (!formulario) return false;

        for (let i = 0; i < formulario.elements.length; i++) {
            if (formulario.elements[i].type == "text") {
                formulario.elements[i].value = "";
            }
        }
        ```
=== "Métodos"
    | Método     | Descripción            |
    |------------|------------------------|
    | `reset()`  | Reinicia el formulario |
    | `submit()` | Envía los datos        |

### Referencia a objetos dentro del formulario

Se puede hacer de dos maneras:

1. con el atributo `id`: `#!javascript document.getElementById("id");`
2. con el atributo `name`: `#!javascript document.name-formulario.name-objeto;`

!!! hint "Ids y nombres"
    Se recomienda asignar un **id único** a cada elemento, y cuando exista un atributo _name_, darle el **mismo valor que el `id`**.

## Objetos _Input_

### Text

En un formulario hay **4 elementos** de tipo texto:

- _text_
- _password_
- _hidden_
- _textarea_

=== "Propiedades"
    | Nombre         | Descripción                                          |
    |----------------|------------------------------------------------------|
    | `defaultValue` | Ajusta o devuelve el valor por defecto               |
    | `form`         | Referencia al formulario que contiene este campo     |
    | `maxLength`    | Ajusta o devuelve la longitud máxima (caracteres)    |
    | `name`         | Ajusta o devuelve el atributo `name`                 |
    | `readOnly`     | Ajusta o devuelve si el campo es de sólo lectura     |
    | `size`         | Ajusta o devuelve la longitud del campo (caracteres) |
    | `type`         | Tipo del campo                                       |
    | `value`        | Ajusta o devuelve el contenido del atributo `value`  |
=== "Métodos"
    | Nombre     | Descripción                       |
    |------------|-----------------------------------|
    | `select()` | Selecciona el contenido del campo |
=== "Ejemplo"
    ```html
    <!DOCTYPE  html>
    <html>
        <head>
            <meta  http-equiv="content-type"  content="text/html;charset=utf-8">
            <title>DWEC05 - Propiedad VALUE de un objeto de tipo texto</title>
            <script  type="text/javascript">
                function convertirMayusculas(){
                    /*
                        En este ejemplo accedemos a la propiedad value de un objeto con id nombre y le asignamos 
                        su contenido actual pero convertido a mayúsculas con el método toUpperCase() del objeto String.
                    */
                    document.getElementById("nombre").value=document.getElementById("nombre").value.toUpperCase();
                }
            </script>
        </head>
    <body>
        <h1>Propiedad VALUE de un objeto INPUT de tipo TEXT</h1>
            <form id="formulario" action="pagina.php">
                <p>
                <label for="nombre">Nombre y Apellidos: </label>
                <input  type="text"  id="nombre"  name="nombre"  value="" size="30" onblur="convertirMayusculas()">
                </p>
                Introduce tu Nombre y Apellidos y haz click fuera del campo.
            </form>
        </body>
    </html>
    ```

### Checkbox

=== "Propiedades"
    | Nombre           | Descripción                                        |
    |------------------|----------------------------------------------------|
    | `checked`        | Ajusta o devuelve el estado (marcado / no marcado) |
    | `defaultChecked` | Valor por defecto del atributo `checked`           |
    | `form`           | Referencia al formulario que lo contiene           |
    | `name`           | Ajusta o devuelve el valor de `name`               |
    | `type`           | Devuelve el tipo de elemento de formulario         |
    | `value`          | Ajusta o devuelve el valor del atributo `value`    |
=== "Ejemplo"
    ```html
    <html>
        <head>
            <meta charset="UTF-8">
            <title>Objeto FORM DOM</title>
            <script type="text/javascript">
                function marcar() {
                    document.getElementById("verano").checked = true;
                }

                function desmarcar() {
                    document.getElementById("verano").checked = false;
                }
            </script>
        </head>
    <body>
        <form action="" method="get">
            <input type="checkbox" id="verano" name="verano" value="Si" />¿Te gusta el verano?
            <input type="submit" />
        </form>
        <button onclick="marcar()">Marcar Checkbox</button>
        <button onclick="desmarcar()">Desmarcar Checkbox</button>
    </body>
    </html>
    ```

### Radio
Los objetos tipo _radio_ son muy similares a los _checkbox_ salvo porque se organizan en **grupos** con el mismo atributo `name`.

Comparte la mayoría de propiedades con `checkbox`y le añade una más:

| Nombre  | Descripción                                                                |
|---------|----------------------------------------------------------------------------|
| `index` | array con todos los botones _radio_ dentro de un grupo con el mismo nombre |

=== "Ejemplo"
    ```html
    <!DOCTYPE HTML>
    <html>
        <head>
            <meta charset="utf-8">
            <title>DWEC05 - Trabajando con objetos input de tipo radio</title>
            <script type="text/javascript">
                function mostrarDatos()
                 {
                    for (var i = 0; i < document.formulario.actores.length; i++)
                    {
                        if (document.formulario.actores[i].checked)
                            alert(document.formulario.actores[i].value);
                    }
                }
            </script>
        </head>
        <body>
            <h1>Trabajando con objetos input de tipo radio</h1>
            <form name="formulario" action="stooges.php">
                <fieldset>
                    <legend>Selecciona tu actor favorito:</legend>
                    <input type="radio" name="actores" id="actor-1" value="Walter Bruce Willis - 19 de Marzo de 1955" checked>
                    <label for="actor-1">Willis</label>
                    <input type="radio" name="actores" id="actor-2" value="James Eugene Jim Carrey - 17 de Enero de 1962">
                    <label for="actor-2">Carrey</label>
                    <input type="radio" name="actores" id="actor-3" value="Luis Tosar - 13 de Octubre de 1971">
                    <label for="actor-3">Tosar</label>
                    <input type="button" id="consultar" name="consultar" value="Consultar Más Datos" onclick="mostrarDatos()">
                </fieldset>
            </form>
        </body>
    </html>
    ```

### Select

Un elemento _select_ es un **array** de elementos _option_. Habitualmente se muestra como:

- Un menú desplegable
- Una lista con selección múltiple

Más información sobre los atributos del elemento _select_ [aquí](https://www.htmlquick.com/es/reference/tags/select.html).

=== "Colección"
    | Nombre          | Descripción                                                    |
    |-----------------|----------------------------------------------------------------|
    |`options`|Devuelve la colección de objetos _option_ correspondiente a una lista |
=== "Propiedades"
    | Nombre             | Descripción                                                    |
    |--------------------|----------------------------------------------------------------|
    | `autofocus`        | Devuelve o ajusta si la lista desplegable obtiene el foco      |
    | `disabled`         | Devuelve o ajusta si la lista desplegable está habilitada o no |
    | `length`           | Devuelve el número de elementos _option_ de la lista           |
    | `multiple`         | Devuelve o ajusta si está habilitada la selección múltiple     |
    | `selectedIndex`    | Devuelve o ajusta el índice de la opción seleccionada          |
    | `options[0].text`  | Devuelve o ajusta el texto de la opción                        |
    | `options[0].value` | Devuelve o ajusta el valor de la opción                        |
=== "Métodos"
    | Nombre            | Descripción                                             |
    |-------------------|---------------------------------------------------------|
    | `add()`           | Añade una opción a la lista desplegable                 |
    | `checkValidity()` |                                                         |
    | `remove()`        | Elimina una opción                                      |
    | `item(index)`     | Devuelve el elemento _option_ en la posición del índice |
    | `namedItem(id)`   | Devuelve el elemento _option_ con el _id_ indicado      |
=== "Ejemplo"
    ```html
    <!DOCTYPE  html>
    <html>
        <head>
            <meta http-equiv="content-type" content="text/html;charset=utf-8">
            <title>DWEC05 - Trabajando con un objeto Select</title>
            <script type="text/javascript">
                function consultar()
                 {
                    var objProvincias = document.getElementById("provincias");
                    var texto = objProvincias.options[objProvincias.selectedIndex].text;
                    var valor = objProvincias.options[objProvincias.selectedIndex].value;
                    alert("Datos de la opción seleccionada:

        Texto: " + texto + "
        Valor: " + valor);
                }
            </script>
        </head>
        <body>
            <h1>Trabajando con un objeto Select</h1>
            <form id="formulario" action="pagina.php">
                <p>
                    <label for="provincias">Seleccione provincia:</label>
                    <select name="provincias" id="provincias">
                        <option value="C">La Coruña</option>
                        <option value="LU">Lugo</option>
                        <option value="OU">Ourense</option>
                        <option value="PO">Pontevedra</option>
                    </select>
                </p>
                Selecciona una opción y pulsa el botón.
                <input type="button" name="boton" value="Consultar información de la opción" onclick="consultar()" />
            </form>
        </body>
    </html>
    ```
    
