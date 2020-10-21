# Objetos de primer nivel

## Objeto _Window_
El objeto _window_ es el objeto principal, dentro del cual se ubican casi todos los demás. Podemos referirnos a él:

- Por su nombre: `#!javascript window.nombrePropiedad;`
- Con la palabra **self**: `#!javascript self.nombrePropiedad;`
- Directamente: `#!javascript nombrePropiedad;`

### Propiedades

| Propiedad        | Descripción                                                  |
|------------------|--------------------------------------------------------------|
| `closed;`        | Ventana cerrada? Boolean                                     |
| `defaultStatus;` | Ajusta o devuelve el valor por defecto de la barra de estado |
| `document;`      | Devuelve el objeto document de la ventana                    |
| `frames;`        | Devuelve un array de todos los marcos de la ventana          |
| `history;`       | Devuelve el objeto 'history' de la ventana                   |
| `length;`        | Número de frames dentro de la ventana                        |
| `location;`      | URL del fichero                                              |
| `name;`          | Ajusta o devuelve el nombre de la ventana                    |
| `navigator;`     | Objeto 'navigator' de la ventana                             |
| `opener;`        | Referencia a la ventana que abrió la v. actual               |
| `parent;`        | ventana padre                                                |
| `window; self;`  | ventana activa ;                                             |
| `status;`        | Ajusta el texto de la barra de estado                        |
| `top;`           | nombre alternativo de la ventana de nivel superior           |

### Métodos

#### Interacción con el usuario (mensajes)

```javascript
alert(mensaje);
confirm(mensaje);
prompt(mensaje, respuesta_defecto);
```

#### Abrir y cerrar

```javascript
open(url, nombre, cc); // Crea una ventana con x características (v. tabla abajo)
close();
```

!!! note "Crear ventanas"
    Un script no puede crear la **ventana principal** del navegador, sólo _subventanas_:

    ```javascript
    var subVentana = window.open("nueva.html", "nueva", "height=800, width=600");
    // cerramos esa ventana usando la variable
    subVentana.close();
    ```

#### Transformar

```javascript
focus();
blur();
moveBy(x,y);
moveTo(x,y);
scrollBy(x,y);
scrollTo(x,y);
```

#### Ejecutar acciones repetitivas

```javascript
clearInterval(id)
setInterval(expr., tiempo[milisegundos]);
clearTimeOut(nombre);
setTimeOut(expr., tiempo[milisegundos]);
```

!!! note "Características de una ventana"
    |              Método                |                Descripción                 |
    |------------------------------------|--------------------------------------------|
    |`toolbar = [yes|no|1|0]`            |¿Tiene barra de herramientas?               |
    |`location = [yes|no|1|0]`           |¿Tiene campo de localización?               |
    |`directories = [yes|no|1|0]`        |¿Tiene botones de dirección?                |
    |`status = [yes|no|1|0]`             |¿Tiene barra de estado?                     |
    |`menubar = [yes|no|1|0]`            |¿Tiene barra de menús?                      |
    |`scrollbars = [yes|no|1|0]`         |¿Tiene barra de desplazamiento?             |
    |`resizable = [yes|no|1|0]`          |¿Se puede redimensionar?                    |
    |`width = px / height = px`          |Ancho y alto de la ventana                  |
    |`outerWidth = px / outerHeight = px`|Ancho y alto total de la ventana            |
    |`top = px / left = px`              |Posición desde la esquina superior izquierda|

## Objeto _Location_

Depende del objeto _Window_ y contiene información relativa a la URL actual:

| Propiedad  | Descripción                                    |
|------------|------------------------------------------------|
| `host`     | Nombre del servidor y número de puerto         |
| `hostname` | Nombre del dominio del servidor (o dir. IP)    |
| `href`     | URL completa                                   |
| `pathname` | Ruta al recurso                                |
| `port`     | Puerto                                         |
| `protocol` | Protocolo utilizado (incluidos los dos puntos) |
| `search`   | Información pasada en una llamada a un script  |

| Método      | Descripción                                                          |
|-------------|----------------------------------------------------------------------|
| `reload()`  | Recarga la URL indicada en la propiedad `href` del objeto _Location_ |
| `replace()` | Reemplaza el historial actual mientras carga la URL indicada         |

## Objeto _Navigator_

Contiene información sobre el navegador.

| Propiedad       | Descripción                                                                              |
|-----------------|------------------------------------------------------------------------------------------|
| `appName`       | Nombre del cliente                                                                       |
| `appVersion`    | Versión del cliente                                                                      |
| `cookieEnabled` | ¿Están habilitadas las cookies?                                                          |
| `language`      | Código del idioma por defecto del navegador                                              |
| `onLine`        | ¿El navegador está _offline_?                                                            |
| `platform`      | Plataforma sobre la que se ejecuta el programa cliente                                   |
| `userAgent`     | Cabecera completa del agente en una petición HTTP. Contiene `appCodeName` y `appVersion` |

| Método          | Descripción                                         |
|-----------------|-----------------------------------------------------|
| `javaEnabled()` | Devuelve _true_ si el cliente permite ejecutar Java |

## Objeto _Document_

- Proporciona da acceso a todos los **elementos HTML**.
- Cada documento cargado en una ventana del navegador es un objeto tipo _Document_.

| Colección   | Descripción                                    |
|-------------|------------------------------------------------|
| `anchors[]` | Array con todos los hiperenlaces del documento |
| `forms[]`   | Array con todos los formularios                |
| `images[]`  | Array con todas las imágenes                   |
| `links[]`   | Array con todos los enlaces                    |

| Propiedad  | Descripción                                            |
|------------|--------------------------------------------------------|
| `cookie`   | Todos los nombres/valores de las cookies del documento |
| `domain`   | Nombre del dominio del servidor que cargó el documento |
| `referrer` | URL del documento desde el que se ha llegado al actual |
| `title`    | Devuelve o cambia el título del documento              |
| `URL`      | URL completa del documento                             |

| Métodos                 | Descripción                                                                     |
|-------------------------|---------------------------------------------------------------------------------|
| `close()`               | Cierra el flujo abierto con `document.open()`                                   |
| `getElementById()`      | Accede a un documento por su _id_                                               |
| `getElementByName()`    | Array de elementos con el valor indicado del atributo _name_                    |
| `getElementByTagName()` | Array de elementos con el valor indicado del atributo _tag_                     |
| `open()`                | Abre un flujo de escritura<br>Permite invocar `document.write()` o `writeln()`  |
| `write()`               | Escribe expresiones HTML o código JavaScript en el documento                    |
| `writeln()`             | Igual que el anterior, añadiendo un salto de línea al final de cada instrucción |
