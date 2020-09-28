# BootStrap

- Fuente: https://www.w3schools.com/bootstrap4/bootstrap_get_started.asp
- Versión: 4

## ¿Qué es _BootStrap_?

BootStrap es un _framework_ gratuito para desarrollar páginas web rápida y fácilmente. Incluye **plantillas** [html](/docs/Lenguajes/html/intro.md) y [css](/docs/Lenguajes/css/intro.md) para tipografías, botones, tablas, navegación, modales, galerías de imágenes y otras cosas, así como algunos plugins [JavaScript](/docs/Lenguajes/javascript/intro.md)

La versión 3 de Bootstrap es la **más estable** y puede usarse si necesitas **compatibilidad** con **IE8-9**, pero carece de muchas funcionalidades y no está en desarrollo activo.

Una alternativa un poco más ligera puede ser [W3.css](https://www.w3schools.com/w3css/default.asp)

## ¿Cómo puedo usar BS4 en mi web?

De dos maneras:

1. Añadiendo una referencia en la cabecera a una plantilla ofrecida por un CDN ( _Content Delivery Network_ )
2. Descargándolo manualmente desde su [página oficial](https://getboostrap.com)

Normalmente usaremos la primera opción, añadiendo alguna de estas líneas:

```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

<!-- jQuery library -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>

<!-- Popper JS -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>

<!-- Latest compiled JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
```

!!! note "Ventajas"
    Cuando un usuario visita una página que use Bootstrap, las plantillas se descargan en la caché, y sirven para todas las demás páginas que lo utilicen. Como muchas páginas usan BS4 hoy, lo más probable que mejore la **velocidad de carga** de tu web.

!!! info "¿jQuery y Popper?"
    BS4 necesita ambos para algunos elementos javascript (modales, _tooltips_, _popovers_, etc) 

### Una página sencilla
#### Añadir el _doctype_ HTML5
#### Diseño responsivo
#### Contenedores
#### Ejemplos

Diseño responsivo con un contenedor de ancho fijo:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap 4 Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</head>
<body>

<div class="container">
  <h1>My First Bootstrap Page</h1>
  <p>This is some text.</p>
</div>

</body>
</html>
```

Página básica con un contenedor _fluido_ que ocupe todo el ancho ( _full width_ )

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap 4 Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</head>
<body>

<div class="container-fluid">
  <h1>My First Bootstrap Page</h1>
  <p>This is some text.</p>
</div>

</body>
</html>
```

!!! note "Clases"
    La única diferencia entre ambas es la **clase** del elemento `div`: _container_ o _fluid_

## Tutorial

Un ejemplo de página con BS4:

```html
--8<-- "Herramientas/Frameworks/bs4.ejemplo01.html"
```

Otro ejemplo un poco más elaborado:

```html
--8<-- "Herramientas/Frameworks/bs4.ejemplo02.html"
```
### Contenedores

Los contenedores sirven para _enmarcar_ el contenido dentro de ellos; hay **dos tipos**:

1. La clase `.container`: **ancho fijo**.
2. La clase `.container-fluid`: **ancho completo** (el que permita el viewport).

#### Contenedor Fijo

|             | Extra pequeño | Pequeño   | Mediano   | Grande    | Extra Grande |
|-------------|---------------|-----------|-----------|-----------|--------------|
| _viewport_  | &lt;576px     | &gt;576px | &gt;768px | &gt;992px | &gt;1200px   |
| `max-width` | 100%          | 540px     | 720px     | 960px     | 1140px       |

#### Padding
Por defecto, los contenedores tienen un padding de 15px (derecha e izquierda); para modificarlos o añadirlo, podemos usar las **[Utilidades](#utilidades) de espaciado**: `#!html <div class="container pt-3"></div>` (añade un padding de 16px arriba).

#### Contenedores responsivos

Especificando una subclase podemos cambiar la anchura máxima según el tamaño o el _viewport_:


| Clase          | Extra pequeño | Pequeño      | Mediano   | Grande    | Extra Grande |
| -------------  | -----------   | ------------ | -------   | --------- | ----------   |
| _viewport_     | &lt;576px     | &gt;576px    | &gt;768px | &gt;992px | &gt;1200px   |
| `container-sm` | 100%          | 540px        | 720px     | 960px     | 1140px       |
| `container-md` | 100%          | 100%         | 720px     | 960px     | 1140px       |
| `container-lg` | 100%          | 100%         | 100%      | 960px     | 1140px       |
| `container-xl` | 100%          | 100%         | 100%      | 100%      | 1140px       |

### Crear un grid básico

Bootstrap implementa un sistema de disposición de bloques con **flexbox** que permite hasta 12 columnas (evidentamente no hace falta usarlas todas).

Para conseguirlo usamos **5 clases**:

1. `.col-` (dispositivos extra pequeños, con menos de 576px de ancho)
2. `.col-sm-` (576px o más)
3. `.col-md-` (768px o más)
4. `.col-lg-` (992px o más)
5. `.col-xl-` (1200px o más)

#### Estructura básica

```html
<!-- Control the column width, and how they should appear on different devices -->
<div class="row">
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
</div>
<div class="row">
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
</div>

<!-- Or let Bootstrap automatically handle the layout -->
<div class="row">
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>
```

!!! Tip "Consejo"
    Para hacer el diseño realmente responsivo podemos usar algo como `#!html <div class="col-sm|md|lg|xl">`.

### Tipografías

La configuración por defecto de BS4 es:

```css
* {
    font-size: 16px;
    line-height: 1.5;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
p {
    margin-top: 0;
    margin-bottom: 1rem; /* 16px */
}
h1 {font-size: 2.5rem; /* 40px */}
h2 {font-size: 2rem; /* 32px */}
h3 {font-size: 1.75rem; /* 28px */}
h4 {font-size: 1.5rem; /* 24px */}
h5 {font-size: 1.25rem; /* 20px */}
h6 {font-size: 1rem; /* 16px */}
```

#### Clases y elementos para texto

`display`
:   Se usan para destacar más que los títulos normales (más grandes pero un tipo de letra más ligero); hay **cuatro clases**: `.display-1|2|3|4`.

`<small>`
:   Subtítulos para los títulos y cabeceras.

`<mark>`
:   Añade un fondo amarillo al texto, como _marcado con rotulador_.

`<abbr>`
:   Añade un subrayado con línea de puntos.

`<blockquote>`
:   Bloques de citas (hay que añadirle también la **clase** `.blockquote`)

```html
<div class="container">
  <h1>Blockquotes</h1>
  <p>The blockquote element is used to present content from another source:</p>
  <blockquote class="blockquote">
    <p>For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.</p>
    <footer class="blockquote-footer">From WWF's website</footer>
  </blockquote>
</div>

```

`<dl>`
:   Lista de definiciones.

`<code>`
:   Texto con tipografía de ancho fijo.

`<kbd>`
:   Combinación de teclas.

`<pre>`
:   Bloque de código.

#### Más clases para texto

- `.font-weight-bold`     : Bold text
- `.font-weight-bolder`   : Bolder text
- `.font-italic`          : Italic text
- `.font-weight-light`    : Light weight text
- `.font-weight-lighter`  : Lighter weight text
- `.font-weight-normal`   : Normal text
- `.lead`                 : Makes a paragraph stand out
- `.small`                : Indicates smaller text (set to 80% of the size of the parent)
- `.text-left`            : Indicates left-aligned text
- `.text-*-left`          : Indicates left-aligned text on small, medium, large or xlarge screens
- `.text-break`           : Prevents long text from breaking layout
- `.text-center`          : Indicates center-aligned text
- `.text-*-center`        : Indicates center-aligned text on small, medium, large or xlarge screens
- `.text-decoration-none` : Removes the underline from a link
- `.text-right`           : Indicates right-aligned text
- `.text-*-right`         : Indicates right-aligned text on small, medium, large or xlarge screens
- `.text-justify`         : Indicates justified text
- `.text-monospace`       : Monospaced text
- `.text-nowrap`          : Indicates no wrap text
- `.text-lowercase`       : Indicates lowercased text
- `.text-reset`           : Resets the color of a text or a link (inherits the color from its parent)
- `.text-uppercase`       : Indicates uppercased text
- `.text-capitalize`      : Indicates capitalized text
- `.initialism`           : Displays the text inside an <abbr> element in a slightly smaller font size
- `.list-unstyled`        : Removes the default list-style and left margin on list items (works on both <ul> and <ol>). This class only applies to immediate children list items (to remove the default list-style from any nested lists, apply this class to any nested lists as well)
- `.list-inline`          : Places all list items on a single line (used together with .list-inline-item on each <li> elements)
- `.pre-scrollable`       : Makes a <pre> element scrollable

#### Colores de texto

BS4 usa los colores para indicar _mensajes_ usando las siguientes clases:

- `.text-muted` (gris)
- `.text-primary` (azul claro, importante)
- `.text-success` (verde claro; éxito)
- `.text-info` (aguamarina; información)
- `.text-warning` (amarillo; aviso)
- `.text-danger` (rojo; peligro)
- `.text-secondary` (gris)
- `.text-white`
- `.text-dark` (gris oscuro)
- `.text-body` (color por defecto)
- `.text-light`

También se puede añadir un 50% de **opacidad** con las clases `.text-black-50` y `.text-white-50`.

#### Color de fondo

- `.bg-primary` (azul claro, importante)
- `.bg-success` (verde claro; éxito)
- `.bg-info` (aguamarina; información)
- `.bg-warning` (amarillo; aviso)
- `.bg-danger` (rojo; peligro)
- `.bg-secondary` (gris)
- `.bg-dark` (gris oscuro)
- `.bg-light`

### Tablas

Para las tablas BS4 usa **separadores horizontales** y un padding ligerito.

Clases:

- `.table` (básico)
- `.table-striped` (colores alternos, como de cebra)
- `.table-bordered` (con bordes)
- `.table-hover` (ligero oscurecimiento de fila al pasar el ratón)
- `.table-dark` (pues eso, para señores Sith)
- `.table-borderless` (sin bordes)

Clases _contextuales_, para colorear filas de modo independiente:

- `.table-primary` (azul claro, importante)
- `.table-success` (verde claro; éxito)
- `.table-info` (aguamarina; información)
- `.table-warning` (amarillo; aviso)
- `.table-danger` (rojo; peligro)
- `.table-active` (gris; color del _hover_)
- `.table-secondary` (gris)
- `.table-dark` (gris oscuro)
- `.table-light`

Colores de cabecera de tabla (`th`)

- `.thead-dark`
- `.thead-light`

Tablas _pequeñas_ (con un padding reducido; mis favoritas)

- `.table-sm`

Tablas responsivas:

- `.table-responsive` (añade una barra de desplazamiento **horizontal**). Se puede elegir cuándo aparece la barra en función del _viewport_:
    - `.table-responsive-sm` (menos de 576px)
    - `.table-responsive-md` (menos de 768px)
    - `.table-responsive-lg` (menos de 992px)
    - `.table-responsive-xl` (menos de 1200px)

### Imágenes

#### Formas

Usa las siguientes clases:

- **Esquinas redondeadas**: `.rounded`
- **Círculo / óvalo** (vintage): `.rounded-circle`
- **Enmarcadas** (rollo _polaroid_): `.img-thumbnail`

#### Alineación

- A la izquierda: `.float-left`
- A la derecha: `.float-right`

Centrar es un poco más delicado; usa las clases `.mx-auto` y `.d-block`:

```html
<img src="paris.jpg" class="mx-auto d-block">

```

#### Responsivas

Usa la clase `.img-fluid` que asigna los atributos `max-width: 100%` y `height: auto` a la imagen:

```html
<img class="img-fluid" src="paris.jpg" alt="Paris">
```
### Jumbotron

Un Jumbotron es una enorme caja gris usada para llamar la atención sobre algún tipo de contenido especial

!!! tip "Consejo"
    Dentro de un _Jumbotron_ puedes incluir cualquier tipo de código HTML

```html 
<div class="jumbotron">
  <h1>Bootstrap Tutorial</h1>
  <p>Bootstrap is the most popular HTML, CSS...</p>
</div>

<!-- El siguiente jumbotron tiene ancho fijo -->
<div class="jumbotron jumbotron-fluid">
  <div class="container"> <!-- el contenedor es obligatorio para que funcione -->
    <h1>Bootstrap Tutorial</h1>
    <p>Bootstrap is the most popular HTML, CSS...</p>
  </div>
</div>
```
### Alertas

Las clases usan la misma nomenclatura que los tipos y colores de texto:

- `.alert-primary` (azul claro, importante)
- `.alert-secondary` (gris)
- `.alert-success` (verde claro; éxito)
- `.alert-info` (aguamarina; información)
- `.alert-warning` (amarillo; aviso)
- `.alert-danger` (rojo; peligro)
- `.alert-dark` (gris oscuro)
- `.alert-light`

#### Enlaces de alerta

Usa la clase `.alert-link` para que los enlaces queden resaltados en el mismo color que la alerta:

```html
<div class="alert alert-success">
  <strong>Success!</strong> Indicates a successful or positive action.
</div>
```

#### Alertas que se pueden cerrar

Se puede añadir un botón para cerrar el mensaje de alerta: 

1. añadiendo la clase `.alert-dismissible` al contenedor.
2. añadiendo `class="close"` y `data-dismiss="alert"` a un **enlace** o **botón**.

```html
<div class="alert alert-success alert-dismissible">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>Success!</strong> Indicates a successful or positive action.
</div>
```

!!! info 
    La **entidad** `&times;` (&times;) es el símbolo de la "x", y se recomienda usarla en lugar de la letra.

#### Alertas animadas

Es una _versión_ de las alertas que se pueden cerrar, con efecto de **desvanecimiento**. Necesita las clases `.fade` y `.show`:

```html
<div class="alert alert-danger alert-dismissible fade show">...</div>
```

### Botones

BS4 proporciona los siguientes estilos para enlaces (`<a>`), botones (`<button>`) y cajas de entrada (`<input>`):

```html
<button type="button" class="btn">Basic</button>
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-dark">Dark</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-link">Link</button>
```

```html
<a href="#" class="btn btn-info" role="button">Link Button</a>
<button type="button" class="btn btn-info">Button</button>
<input type="button" class="btn btn-info" value="Input Button">
<input type="submit" class="btn btn-info" value="Submit Button">
```

#### Con borde

Las mismas clases, con el sufijo `-outline`: `#!html <button class="btn btn-outline-primary">`, etc.

#### Tamaños

```html
<button type="button" class="btn btn-primary btn-lg">Large</button>
<button type="button" class="btn btn-primary">Default</button>
<button type="button" class="btn btn-primary btn-sm">Small</button>
```

#### Botones de "nivel"

Con la clase `.btn-block`. Ocupan todo el ancho del elemento padre:

```html
<button type="button" class="btn btn-primary btn-block">Full-Width Button</button>
```

#### Botones activos o desactivados

Un botón _activo_ aparece **presionado**, mientras que un botón _desactivado_ no puede ser pulsado.

- Para los **botones** se usa la clase `.active` y el atributo `.disabled`.
- Para los **enlaces** (`a`), se debe usar la clase `disabled` (y no el atributo)

```html
<button type="button" class="btn btn-primary active">Active Primary</button>
<button type="button" class="btn btn-primary" disabled>Disabled Primary</button>
<a href="#" class="btn btn-primary disabled">Disabled Link</a>
```

#### Botones con "Spinner"

```html
<button class="btn btn-primary">
  <span class="spinner-border spinner-border-sm"></span>
</button>

<button class="btn btn-primary">
  <span class="spinner-border spinner-border-sm"></span>
  Loading..
</button>

<button class="btn btn-primary" disabled>
  <span class="spinner-border spinner-border-sm"></span>
  Loading..
</button>

<button class="btn btn-primary" disabled>
  <span class="spinner-grow spinner-grow-sm"></span>
  Loading..
</button>
```

### Grupos de botones

Un grupo de botones es algo tan simple como varios botones unidos. Para conseguirlo se usa un elemento `<div>` con la clase `.btn-group`:

```html
<div class="btn-group">
  <button type="button" class="btn btn-primary">Apple</button>
  <button type="button" class="btn btn-primary">Samsung</button>
  <button type="button" class="btn btn-primary">Sony</button>
</div>

```

- Para cambiar el tamaño del grupo se usa la clase `.btn-group-lg` o `.btn-group-sm` en el **contenedor** `<div>`.
- Para conseguir un grupo de botones vertical se usa la clase `.btn-group-vertical` también en el **contenedor**.

#### Anidar botones

Se pueden crear **submenús** desplegables de botones:

```html
<div class="btn-group">
  <button type="button" class="btn btn-primary">Apple</button>
  <button type="button" class="btn btn-primary">Samsung</button>
  <div class="btn-group">
    <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
       Sony
    </button>
    <div class="dropdown-menu">
      <a class="dropdown-item" href="#">Tablet</a>
      <a class="dropdown-item" href="#">Smartphone</a>
    </div>
  </div>
</div>
```

El botón para desplegar el menú se puede separar del botón principal:

```html
<div class="btn-group">
  <button type="button" class="btn btn-primary">Sony</button>
  <button type="button" class="btn btn-primary dropdown-toggle dropdown-toggle-split" data-toggle="dropdown">
    <span class="caret"></span>
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" href="#">Tablet</a>
    <a class="dropdown-item" href="#">Smartphone</a>
  </div>
</div>
```

Un ejemplo de menú de botones desplegable vertical:

```html
<div class="btn-group-vertical">
  <button type="button" class="btn btn-primary">Apple</button>
  <button type="button" class="btn btn-primary">Samsung</button>
  <div class="btn-group">
    <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
       Sony
    </button>
    <div class="dropdown-menu">
      <a class="dropdown-item" href="#">Tablet</a>
      <a class="dropdown-item" href="#">Smartphone</a>
    </div>
  </div>
</div>

```

!!! Tip "Grupos de botones lado a lado"
    Por defecto los grupos de botones son elementos _inline_, de modo que aparecerán uno al lado del otro cuando haya más de uno:

    ```html
    <div class="btn-group">
      <button type="button" class="btn btn-primary">Apple</button>
      <button type="button" class="btn btn-primary">Samsung</button>
      <button type="button" class="btn btn-primary">Sony</button>
    </div>

    <div class="btn-group">
      <button type="button" class="btn btn-primary">BMW</button>
      <button type="button" class="btn btn-primary">Mercedes</button>
      <button type="button" class="btn btn-primary">Volvo</button>
    </div>

    ```

### Badges

Los _badges_ (traducido como "insignia" o "símbolo") se utilizan para añadir información extra a algún elemento.

Se utilizan dentro de un **elemento `<span>`** con las clases `.badge` y `.badge-*`:

```html
<h1>Example heading <span class="badge badge-secondary">New</span></h1>
```

#### Contextuales

```html
<span class="badge badge-primary">Primary</span>
<span class="badge badge-secondary">Secondary</span>
<span class="badge badge-success">Success</span>
<span class="badge badge-danger">Danger</span>
<span class="badge badge-warning">Warning</span>
<span class="badge badge-info">Info</span>
<span class="badge badge-light">Light</span>
<span class="badge badge-dark">Dark</span>
```

#### Con forma de píldora

```html
<span class="badge badge-pill badge-primary">Primary</span>
<span class="badge badge-pill badge-secondary">Secondary</span>
<span class="badge badge-pill badge-success">Success</span>
<span class="badge badge-pill badge-danger">Danger</span>
<span class="badge badge-pill badge-warning">Warning</span>
<span class="badge badge-pill badge-info">Info</span>
<span class="badge badge-pill badge-light">Light</span>
<span class="badge badge-pill badge-dark">Dark</span>
```

#### Dentro de un elemento

Por ejemplo, para mostrar el número de mensajes pendientes en un botón:

```html
<button type="button" class="btn btn-primary">
    Mensajes <span class="badge badge-light">4</span>
</button>
```

### Barras de progreso

Para añadir una barra de progreso, usa la clase `.progress` en el elemento contenedor y la clase `.progress-bar` en el elemento hijo. Con la propiedad CSS `width` y `height` se puede ajustar la **anchura** y la **altura** respectivamente:

```html
<div class="progress">
  <div class="progress-bar" style="width:70%"></div>
</div>
```

#### Etiquetas

Si quieres añadir un texto a la barra de progreso escribe dentro del elemento:

```html
<div class="progress">
    <div class="progress-bar" style="width:70%">70%</div>
</div>
```

#### Colores

Por defecto, la barra tiene el color _primario_, pero se pueden usar el resto de **clases contextuales** para darle color: `bg-success`, `bg-info`, `bg-warning`, etc.

#### Con barras

Para darle el efecto de líneas diagonales, usa la clase `.progress-bar-striped`.

#### Animada

Usa la clase `.progress-bar-animated`.

#### Barras múltiples

Las barras se pueden apilar, una al lado de la otra:

```html
<div class="progress">
  <div class="progress-bar bg-success" style="width:40%">
    Free Space
  </div>
  <div class="progress-bar bg-warning" style="width:10%">
    Warning
  </div>
  <div class="progress-bar bg-danger" style="width:20%">
    Danger
  </div>
</div>
```

### Spinners

Para añadir animaciones de carga ( _spinners_ ), se usan las siguientes clases:

- `.spinner-border`
- con colores:
    -  `.spinner-border text-muted`
    -  `.spinner-border text-primary`
    -  `.spinner-border text-success`
    -  `.spinner-border text-info`
    -  `.spinner-border text-warning`
    -  `.spinner-border text-danger`
    -  `.spinner-border text-secondary`
    -  `.spinner-border text-dark`
    -  `.spinner-border text-light`
- `.spinner-grow` (con efecto de "crecer")

#### Tamaño

Usa las clases `.spinner-border-sm` o `.spinner-grow-sm` para crear uno **más pequeño**.

#### Dentro de botones

Ejemplo para crear un botón de carga con animación:

```html
<button class="btn btn-primary">
  <span class="spinner-border spinner-border-sm"></span>
  Loading..
</button>

<button class="btn btn-primary" disabled>
  <span class="spinner-border spinner-border-sm"></span>
  Loading..
</button>

<button class="btn btn-primary" disabled>
  <span class="spinner-grow spinner-grow-sm"></span>
  Loading..
</button>
```

### Paginación

Por _paginación_ me refiero a crear una serie de botones para moverse entre "páginas" de contenido (lo típico: `Anterior | 1 | 2 | 3 | Siguiente`).

Usa la clase `.pagination` en un elemento `<ul>`, y añade la clase `.page-item` a cada elemento `<li>` junto con una clase `.page-link` a los enlaces  (elementos `<a>`).

```html
<ul class="pagination">
  <li class="page-item"><a class="page-link" href="#">Previous</a></li>
  <li class="page-item"><a class="page-link" href="#">1</a></li>
  <li class="page-item"><a class="page-link" href="#">2</a></li>
  <li class="page-item"><a class="page-link" href="#">3</a></li>
  <li class="page-item"><a class="page-link" href="#">Next</a></li>
</ul>
```

#### Página activa

Con la clase `.active`:

```html
<ul class="pagination">
  <li class="page-item"><a class="page-link" href="#">Previous</a></li>
  <li class="page-item"><a class="page-link" href="#">1</a></li>
  <li class="page-item active"><a class="page-link" href="#">2</a></li>
  <li class="page-item"><a class="page-link" href="#">3</a></li>
  <li class="page-item"><a class="page-link" href="#">Next</a></li>
</ul>
```

#### Botón desactivado

```html
  <li class="page-item disabled"><a class="page-link" href="#">Previous</a></li>
```

#### Tamaño de los botones

Usando las clases `.pagination-lg` (grande), y `.pagination-sm` (pequeño).

#### Alineación

```html
<!-- Por defecto ( a la izquierda ) -->
<ul class="pagination" style="margin:20px 0">
  <li class="page-item">...</li>
</ul>

<!-- Centrado -->
<ul class="pagination justify-content-center" style="margin:20px 0">
  <li class="page-item">...</li>
</ul>

<!-- A la derecha -->
<ul class="pagination justify-content-end" style="margin:20px 0">
  <li class="page-item">...</li>
</ul>
```

#### "Breadcrumbs"

Otra forma de navegación:  `Fotos / Verano 2017 / Italia / Roma`. Usa las clases `.breadcrumb` y `.breadcrumb-item`:

```html
<ul class="breadcrumb">
  <li class="breadcrumb-item"><a href="#">Photos</a></li>
  <li class="breadcrumb-item"><a href="#">Summer 2017</a></li>
  <li class="breadcrumb-item"><a href="#">Italy</a></li>
  <li class="breadcrumb-item active">Rome</li>
</ul>
```

### Grupos de listas

Es bastante fácil, con la clase `.list-group`.

Ésta se puede combinar con el resto de clases y elementos vistos.

- Sin bordes: `.list-group-flush`
- Horizontal: `.list-group` + `.list-group-horizontal` ( `#!html <ul class="list-group list-group-horizontal">`)

!!! Tip "Listas con número de elementos"
    Se puede conseguir añadiendo un `<span>` a cada elemento `<li>`:
    
    ```html
    <ul class="list-group">
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Inbox
        <span class="badge badge-primary badge-pill">12</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Ads
        <span class="badge badge-primary badge-pill">50</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Junk
        <span class="badge badge-primary badge-pill">99</span>
      </li>
    </ul>
    ```
    
### Tarjetas 

Las tarjetas son cajas que pueden contener otros elementos dentro, en un estilo similar a una _tarjeta de visita_. Es mejor verlo con imágenes así que mientras no consiga añadirlas aquí remito directamente a la fuente en [w3schools](https://www.w3schools.com/bootstrap4/bootstrap_cards.asp)

```html
<!-- Básica -->
<div class="card">
    <div class="card-body">Tarjeta básica</div>
</div>

<!-- Con cabecera y pie -->
<div class="card">
  <div class="card-header">Header</div>
  <div class="card-body">Content</div>
  <div class="card-footer">Footer</div>
</div>

<!-- Títulos, texto y enlaces -->
<div class="card">
  <div class="card-body">
    <h4 class="card-title">Card title</h4>
    <p class="card-text">Some example text. Some example text.</p>
    <a href="#" class="card-link">Card link</a>
    <a href="#" class="card-link">Another link</a>
  </div>
</div>
```

#### Tarjetas con imágenes

```html
<div class="card" style="width:400px">
   <!-- imagen arriba del todo; para imagen abajo, usar "card-img-bottom" -->
   <!-- se coloca fuera del contenedor "card-body" para que ocupe todo el ancho -->
  <img class="card-img-top" src="img_avatar1.png" alt="Card image">
  <div class="card-body">
    <h4 class="card-title">John Doe</h4>
    <p class="card-text">Some example text.</p>
    <a href="#" class="btn btn-primary">See Profile</a>
  </div>
</div>
```

#### Tarjetas cliqueables

Para que toda la tarjeta se convierta en un enlace, se usa la clase `.stretched-link` en un **enlace** dentro de la tarjeta:

```html
<a href="#" class="btn btn-primary stretched-link">See Profile</a>
```

#### Conjuntos de tarjetas

Con **columnas responsivas** (estilo [Pinterest](https://www.pinterest.es)):

```html
<div class="card-columns">
  <div class="card bg-primary">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the first card</p>
    </div>
  </div>
  <div class="card bg-warning">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the second card</p>
    </div>
  </div>
  <div class="card bg-success">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the third card</p>
    </div>
  </div>
  <div class="card bg-danger">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the fourth card</p>
    </div>
  </div>
  <div class="card bg-light">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the fifth card</p>
    </div>
  </div>
  <div class="card bg-info">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the sixth card</p>
    </div>
  </div>
</div>
```

Con **columnas de ancho fijo**:

```html
<div class="card-deck">
  <div class="card bg-primary">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the first card</p>
    </div>
  </div>
  <div class="card bg-warning">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the second card</p>
    </div>
  </div>
  <div class="card bg-success">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the third card</p>
    </div>
  </div>
  <div class="card bg-danger">
    <div class="card-body text-center">
      <p class="card-text">Some text inside the fourth card</p>
    </div>
  </div>
</div>
```

Si queremos que las tarjetas aparezcan pegadas una a la otra, se usa la clase `card-group`:

```html
<div class="card-group">
```

### Desplegables

Para crear un menú desplegable usamos la clase `.dropdown` en un contenedor, y la clase `.dropdown-item` a cada elemento del menú:

```html
<div class="dropdown">
  <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
    Dropdown button
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" href="#">Link 1</a>
    <!-- añade una línea divisoria -->
    <div class="dropdown-divider"></div>
    <a class="dropdown-item" href="#">Link 2</a>
    <!-- añade una cabecera a un grupo de enlaces -->
    <div class="dropdown-header">Dropdown header 1</div>
    <a class="dropdown-item" href="#">Link 3</a>
  </div>
</div>
```

#### Posición del desplegable

- A la derecha: `.dropright`
- A la izquierda: `.dropleft`
- Arriba: `.dropup`

### Contenido desplegable (Collapse)

BootStrap permite crear botones para mostrar y ocultar contenido:

```html
<button data-toggle="collapse" data-target="#demo">Collapsible</button>

<div id="demo" class="collapse">
Lorem ipsum dolor text....
</div>
```

!!! info "Oculto por defecto"
    Si queremos que el contenido sea visible desde el principio, hay que añadir la clase `.show`:

    ```html
    <div id="demo" class="collapse show">
    Este texto será visible sin necesidad de hacer nada
    </div>
    ```
#### Desplegables en acordeón

Una forma bastante atractiva de mostrar contenido:

!!! note "Expandir sólo el contenido activo"
    Para que el resto de contenido se contraiga y oculte al abrir otro, hay que añadir el atributo `data-parent` al item que queramos

```html
<div id="accordion">

  <div class="card">
    <div class="card-header">
      <a class="card-link" data-toggle="collapse" href="#collapseOne">
        Collapsible Group Item #1
      </a>
    </div>
    <div id="collapseOne" class="collapse show" data-parent="#accordion">
      <div class="card-body">
        Lorem ipsum..
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <a class="collapsed card-link" data-toggle="collapse" href="#collapseTwo">
        Collapsible Group Item #2
      </a>
    </div>
    <div id="collapseTwo" class="collapse" data-parent="#accordion">
      <div class="card-body">
        Lorem ipsum..
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <a class="collapsed card-link" data-toggle="collapse" href="#collapseThree">
        Collapsible Group Item #3
      </a>
    </div>
    <div id="collapseThree" class="collapse" data-parent="#accordion">
      <div class="card-body">
        Lorem ipsum..
      </div>
    </div>
  </div>

</div>
```

### Navs

Para crear un menú de navegación horizontal, hay que añadir la clase `.nav` a un elemento `<ul>`, y la clase `.nav-item` a los elementos `<li>` que lo compongan:

```html
<ul class="nav">
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#">Disabled</a>
  </li>
</ul>
```

#### Alineación

- Centrado: `.justify-content-center`
- A la derecha: `.justify-content-end`

#### Vertical

```html
<ul class="nav flex-column">
```

#### Pestañas

Para convertir el menú de nevagación en pestañas usamos la clase `.nav-tabs` y la clase `.active` en el enlace activo:

```html
<ul class="nav nav-tabs">
  <li class="nav-item">
    <a class="nav-link active" href="#">Active</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#">Disabled</a>
  </li>
</ul>
```

!!! info "Pestañas dinámicas"
    Si quieres que al pinchar en un elemento se muestre el contenido (en lugar de cargar otra página), hay que hacer un par de cosas:

    1. Añadir el atributo `data-toggle="tab"` a **cada enlace**.
    2. Añadir la clase `.tab-pane` con un **id único** para cada pestaña.
    3. Envolver todos los enlaces en un elemento `<div>` con la clase `.tab-content`

    ```html
    <!-- Nav tabs -->
    <ul class="nav nav-tabs">
      <li class="nav-item">
        <a class="nav-link active" data-toggle="tab" href="#home">Home</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" data-toggle="tab" href="#menu1">Menu 1</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" data-toggle="tab" href="#menu2">Menu 2</a>
      </li>
    </ul>

    <!-- Tab panes -->
    <div class="tab-content">
      <div class="tab-pane container active" id="home">...</div>
      <div class="tab-pane container fade" id="menu1">...</div>
      <div class="tab-pane container fade" id="menu2">...</div>
    </div>
    ```

#### Enlaces estilo "píldora"

- En lugar de la clase `.nav-tabs`, usamos `.nav-pills`
- Si queremos que las píldoras ocupen todo el ancho hay que añadir la clase `.nav-justified`.

### Barra de navegación

Uno de las partes más importantes de una web es la **barra de navegación**, que aparece como cabecera de la página.

Con BootStrap, la barra puede _expandirse_ o _contraerse_, dependiendo del tamaño de la pantalla.

```html
<!-- A grey horizontal navbar that becomes vertical on small screens -->
<nav class="navbar navbar-expand-sm bg-light">
<!-- Si quitamos la clase "navbar-expand-sm" (y variantes) el menú será vertical -->

  <!-- Links -->
  <ul class="navbar-nav">
    <li class="nav-item">
      <a class="nav-link" href="#">Link 1</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link 2</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link 3</a>
    </li>
  </ul>

</nav>
```

- Para **centrar** la barra añadimos la clase `.justify-content-center`.
- Para añadir **color**, las clases auxiliares `.bg-color`.

```html
<!-- Grey with black text -->
<nav class="navbar navbar-expand-sm bg-light navbar-light">
  <ul class="navbar-nav">
    <li class="nav-item active">
      <a class="nav-link" href="#">Active</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link</a>
    </li>
    <li class="nav-item">
      <a class="nav-link disabled" href="#">Disabled</a>
    </li>
  </ul>
</nav>

<!-- Black with white text -->
<nav class="navbar navbar-expand-sm bg-dark navbar-dark">...</nav>

<!-- Blue with white text -->
<nav class="navbar navbar-expand-sm bg-primary navbar-dark">...</nav>
```

#### Logo / Marca

Para añadir un logo o marca se usa la clase `.navbar-brand`:

```html
<nav class="navbar navbar-expand-sm bg-dark navbar-dark">
  <a class="navbar-brand" href="#">Logo</a>
  ...
</nav>
```

!!! info "Iconos"
    Cuando se usa la clase `.navbar-brand` en una imagen, BS4 ajustará el tamaño y estilo automáticamente a la altura de la barra.

    ```html
    <nav class="navbar navbar-expand-sm bg-dark navbar-dark">
        <a class="navbar-brand" href="#">
            <img src="bird.jpg" alt="Logo" style="width:40px;">
        </a>
          ...
    </nav>
    ```

#### Menú de hamburguesa (esconder la barra de navegación)

Para crear una barra de navegación desplegable hay que:

1. Usar un botón con `#!html class="navbar-toggler", data-toggle="collapse", data-target="#destino"`.
2. Envolver el contenido de la barra en un elemento `<div>` con `#!html class="collapse navbar-collapse id="destino"`

!!! warning "Data-target"
    El identificador del atributo `data-target` del **botón** tiene que coincidir con el id del **contenedor de los enlaces**

```html
<!-- Elimina la clase navbar-expand-md para que SIEMPRE se escondan los enlaces -->
<nav class="navbar navbar-expand-md bg-dark navbar-dark">
  <!-- Brand -->
  <a class="navbar-brand" href="#">Navbar</a>

  <!-- Toggler/collapsibe Button -->
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#collapsibleNavbar">
    <span class="navbar-toggler-icon"></span>
  </button>

  <!-- Navbar links -->
  <div class="collapse navbar-collapse" id="collapsibleNavbar">
    <ul class="navbar-nav">
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
    </ul>
  </div>
</nav>
```

#### Formularios y botones en la barra

Para agrupar lado a lado los formularios (`<input>`) y botones _dentro_ de la barra, envuélvelos en un elemento `#!html <form class="form-inline">`.

```html
<nav class="navbar navbar-expand-sm bg-dark navbar-dark">
  <form class="form-inline" action="/action_page.php">
    <input class="form-control mr-sm-2" type="text" placeholder="Search">
    <button class="btn btn-success" type="submit">Search</button>
    <!-- botón de login (agrupando dos botones) -->
    <div class="input-group">
      <div class="input-group-prepend">
        <span class="input-group-text">@</span>
      </div>
      <input type="text" class="form-control" placeholder="Username">
    </div>
  </form>
</nav>
```

#### Texto en la barra

```html
<nav class="navbar navbar-expand-sm bg-dark navbar-dark">

<!-- Links -->
  <ul class="navbar-nav">
    <li class="nav-item">
      <a class="nav-link" href="#">Link 1</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link 2</a>
    </li>
  </ul>

  <!-- Navbar text-->
  <span class="navbar-text">
    Navbar text
  </span>

</nav>
```

#### Posición fija

Para fijar la barra arriba, añade la clase `.fixed-top`; de forma parecida, para fijarla abajo, usa `.fixed-bottom`.

```html
<nav class="navbar navbar-expand-sm bg-dark navbar-dark fixed-top">
  ...
</nav>
```

!!! note
    Algo parecido se puede conseguir con la clase `.sticky-top`.

    La diferencia es que una barra _pegajosa_ solo se queda fija una vez haces scroll sobre ella; el efecto se aprecia si la barra no está arriba del todo de la página (porque haya un banner, por ejemplo).

### Formularios

BootStrap proporciona dos tipos de formularios:

- Apilados: verticales y ocupando todo el ancho.
- En línea

Los siguientes ejemplos crean dos cajas de texto para loguearse (email, usuario, "recuérdame" y "submit").

#### Apilados

```html
<form action="/action_page.php">
  <div class="form-group">
    <label for="email">Email address:</label>
    <input type="email" class="form-control" placeholder="Enter email" id="email">
  </div>
  <div class="form-group">
    <label for="pwd">Password:</label>
    <input type="password" class="form-control" placeholder="Enter password" id="pwd">
  </div>
  <div class="form-group form-check">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

#### En línea

A este ejemplo se han añadido clases de las **utilidades** de BootStrap para mejorar el espaciado entre cajas:

```html
<form class="form-inline" action="/action_page.php">
  <label for="email" class="mr-sm-2">Email address:</label>
  <input type="email" class="form-control mb-2 mr-sm-2" placeholder="Enter email" id="email">
  <label for="pwd" class="mr-sm-2">Password:</label>
  <input type="password" class="form-control mb-2 mr-sm-2" placeholder="Enter password" id="pwd">
  <div class="form-check mb-2 mr-sm-2">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-primary mb-2">Submit</button>
</form>
```

#### Modo columna / grid

```html
<!-- básico -->
<form>
  <div class="row">
    <div class="col">
      <input type="text" class="form-control" id="email" placeholder="Enter email" name="email">
    </div>
    <div class="col">
      <input type="password" class="form-control" placeholder="Enter password" name="pswd">
    </div>
  </div>
</form>

<!-- más compacto -->
<form>
  <div class="form-row">
    <div class="col">
      <input type="text" class="form-control" id="email" placeholder="Enter email" name="email">
    </div>
    <div class="col">
      <input type="password" class="form-control" placeholder="Enter password" name="pswd">
    </div>
  </div>
</form>
```

#### Validar Formularios

Se pueden añadir mensajes de validación de los datos introducidos:

- **Antes de introducirlos**: `#!html <form class="was-validated">`
- **Después de introducirlos**: `#!html <form class="needs-validation">`

También se puede añadir un mensaje con:

- Confirmación de que está correcto: `#!html <div class="valid-feedback">Válido</div>`
- Lo que debe corregir (después de introducir los datos): `#!html <div class="invalid-feedback">Este campo no puede estar vacío</div>`

```html
<!-- Este muestra los errores ANTES de rellenarlo -->
<form action="/action_page.php" class="was-validated">
  <div class="form-group">
    <label for="uname">Username:</label>
    <input type="text" class="form-control" id="uname" placeholder="Enter username" name="uname" required>
    <div class="valid-feedback">Valid.</div>
    <div class="invalid-feedback">Please fill out this field.</div>
  </div>
  <div class="form-group">
    <label for="pwd">Password:</label>
    <input type="password" class="form-control" id="pwd" placeholder="Enter password" name="pswd" required>
    <div class="valid-feedback">Valid.</div>
    <div class="invalid-feedback">Please fill out this field.</div>
  </div>
  <div class="form-group form-check">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox" name="remember" required> I agree on blabla.
      <div class="valid-feedback">Valid.</div>
      <div class="invalid-feedback">Check this checkbox to continue.</div>
    </label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>

<!-- El siguiente ejemplo muestra la validación DESPUÉS de introducir los datos -->
<!-- Necesita código jQuery -->
<form action="/action_page.php" class="needs-validation" novalidate>
  <div class="form-group">
    <label for="uname">Username:</label>
    <input type="text" class="form-control" id="uname" placeholder="Enter username" name="uname" required>
    <div class="valid-feedback">Valid.</div>
    <div class="invalid-feedback">Please fill out this field.</div>
  </div>
  <div class="form-group">
    <label for="pwd">Password:</label>
    <input type="password" class="form-control" id="pwd" placeholder="Enter password" name="pswd" required>
    <div class="valid-feedback">Valid.</div>
    <div class="invalid-feedback">Please fill out this field.</div>
  </div>
  <div class="form-group form-check">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox" name="remember" required> I agree on blabla.
      <div class="valid-feedback">Valid.</div>
      <div class="invalid-feedback">Check this checkbox to continue.</div>
    </label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>

<script>
// Desactiva los envíos de formulario si hay campos inválidos
(function() {
  'use strict';
  window.addEventListener('load', function() {
    // Recupera los formularios a los que vamos añadir estilo de validación
    var forms = document.getElementsByClassName('needs-validation');
    // Bucle que impide el envío de formulario sobre los anteriores
    var validation = Array.prototype.filter.call(forms, function(form) {
      form.addEventListener('submit', function(event) {
        if (form.checkValidity() === false) {
          event.preventDefault();
          event.stopPropagation();
        }
        form.classList.add('was-validated');
      }, false);
    });
  }, false);
})();
</script>
```

### Input

BootStrap soporta los siguientes controles de formulario:

- input
- textarea
- checkbox
- radio
- select

#### BS Input

Se pueden usar todos los tipos de _input_ de HTML5:

- **text**
- **password**
- **datetime**
- datetime-local
- date
- month
- time
- week
- number
- **email**
- url
- **search**
- tel
- color

!!! note "Tipo"
    El estilo BS4 solo se aplicará si se declara apropiadamente el **tipo** de input.

```html
<!-- Ejemplo de dos inputs, uno tipo "text" y otro "password"-->
<!-- la clase .form-control nos ayuda a aplicar ancho completo y el padding apropiado -->
<div class="form-group">
  <label for="usr">Name:</label>
  <input type="text" class="form-control" id="usr">
</div>
<div class="form-group">
  <label for="pwd">Password:</label>
  <input type="password" class="form-control" id="pwd">
</div>
```

```html
<!-- Área de texto -->
<div class="form-group">
  <label for="comment">Comment:</label>
  <textarea class="form-control" rows="5" id="comment"></textarea>
</div>
```

```html
<!-- chekbox -->
<div class="form-check">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 1
  </label>
</div>
<div class="form-check">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 2
  </label>
</div>
<div class="form-check">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="" disabled>Option 3
  </label>
</div>

<!-- inline checkbox -->
<div class="form-check-inline">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 1
  </label>
</div>
<div class="form-check-inline">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 2
  </label>
</div>
<div class="form-check-inline">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="" disabled>Option 3
  </label>
</div>
```

!!! note "Botones redondos ( _radio buttons_ )"
    Se hacen igual que los _checkbox_, cambiando el atributo _type_ a `radio`, y sustituyendo _value_ por `#!html name="optradio"`.

    ```html
    <div class="form-check">
      <label class="form-check-label">
        <input type="radio" class="form-check-input" name="optradio">Option 1
      </label>
    </div>
    ```

#### Listas de opciones

Las _select list_ se crean así:

```html
<div class="form-group">
  <label for="sel1">Select list:</label>
  <select class="form-control" id="sel1">
    <option>1</option>
    <option>2</option>
    <option>3</option>
    <option>4</option>
  </select>
</div>
```

#### Ajustar el tamaño

Usando las clases `.form-control-sm` o `.form-control-lg`.

#### Caja de entrada con texto plano

```html
<input type="text" class="form-control-plaintext">
```

#### Selección de archivo y barra de ajuste ("range")

```html
<input type="range" class="form-control-range">
<input type="file" class="form-control-file border">
```

### Grupos de input

Si queremos añadir un botón, texto o icono a una caja de entrada, debemos envolver ambos en un contenedor con la clase `.input-group`.

- **Antes de la caja**: `.input-group-prepend`
- **Después de la caja**: `.input-group-append`

El texto de ayuda debe llevar la clase `.input-group-text`

```html
<!-- Entrada de usuario (@ a la izquierda) -->
<form>
  <div class="input-group mb-3">
  <!-- clase ".mb-3" controla el margen inferior -->
    <div class="input-group-prepend">
      <span class="input-group-text">@</span>
    </div>
    <input type="text" class="form-control" placeholder="Username">
  </div>

<!-- Entrada de email (@example.com a la derecha) -->
  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Your Email">
    <div class="input-group-append">
      <span class="input-group-text">@example.com</span>
    </div>
  </div>
</form>
```

#### Tamaño del grupo

Usando las clases `.input-group-sm` y `.input-group-lg`.

#### Grupos de entrada múltiples y ayudas

Si queremos crear algo como:

```html
<!--
| Person | "Nombre"       | "Apellido" |

| Uno | Dos | Tres |                   |
-->

<!-- Multiple inputs -->
<form>
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <span class="input-group-text">Person</span>
    </div>
    <input type="text" class="form-control" placeholder="First Name">
    <input type="text" class="form-control" placeholder="Last Name">
  </div>
</form>

<!-- Multiple addons / help text -->
<form>
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <span class="input-group-text">One</span>
      <span class="input-group-text">Two</span>
      <span class="input-group-text">Three</span>
    </div>
    <input type="text" class="form-control">
  </div>
</form>
```

#### Botones en grupos de entrada

```html
<!-- Botón básico a la izquierda -->
<div class="input-group mb-3">
  <div class="input-group-prepend">
    <button class="btn btn-outline-primary" type="button">Basic Button</button>
  </div>
  <input type="text" class="form-control" placeholder="Some text">
</div>

<!-- botón de "ir" a los resultados de la búsqueda -->
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="Search">
  <div class="input-group-append">
    <button class="btn btn-success" type="submit">Go</button>
  </div>
</div>

<!-- Dos botones: ok y cancelar -->
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="Something clever..">
  <div class="input-group-append">
    <button class="btn btn-primary" type="button">OK</button>
    <button class="btn btn-danger" type="button">Cancel</button>
  </div>
</div>
```

#### Etiquetas del grupo

El siguiente ejemplo añade una "etiqueta" al grupo y lleva el foco al primer cuadro de entrada al hacer click sobre ella:

```html
<label for="demo">Write your email here:</label>
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="Email" id="demo" name="email">
  <div class="input-group-append">
    <span class="input-group-text">@example.com</span>
  </div>
</div>
```

### Formularios personalizados

Más que personalizados deberían considerarse _alternativas_ predefinidas ya por BootStrap: un _checkbox_ con esquinas redondeadas, menú desplegable diferente... [Aquí se pueden ver](https://www.w3schools.com/bootstrap4/bootstrap_forms_custom.asp)

```html
<!-- Checkbox -->
<form>
  <div class="custom-control custom-checkbox">
    <input type="checkbox" class="custom-control-input" id="customCheck" name="example1">
    <label class="custom-control-label" for="customCheck">Check this custom checkbox</label>
  </div>
</form>

<!-- Switch -->
<form>
  <div class="custom-control custom-switch">
    <input type="checkbox" class="custom-control-input" id="switch1">
    <label class="custom-control-label" for="switch1">Toggle me</label>
  </div>
</form>

<!-- Radio -->
<form>
  <div class="custom-control custom-radio">
    <input type="radio" class="custom-control-input" id="customRadio" name="example1" value="customEx">
    <!-- El valor de 'input id' y 'label for' debe coincidir -->
    <label class="custom-control-label" for="customRadio">Custom radio</label>
  </div>
</form>
```

#### Menú de selección alternativo

La clase `.custom-select` crea un menú desplegabla al ancho completo, más bonito que el estilo por defecto:

```html
<form>
  <select name="cars" class="custom-select">
    <option selected>Custom Select Menu</option>
    <option value="volvo">Volvo</option>
    <option value="fiat">Fiat</option>
    <option value="audi">Audi</option>
  </select>
</form>
```

Se puede ajustar el tamaño con las clases `.custom-select-sm` y `.custom-select-lg`

#### Rango y subida de archivo alternativos

Los cambios son similares al ejemplo anterior: 

- ancho completo
- mejor estilo

```html
<!-- rango -->
<form>
  <label for="customRange">Custom range</label>
  <input type="range" class="custom-range" id="customRange" name="points1">
</form>

<!-- subida de archivo -->
<form>
  <div class="custom-file">
    <input type="file" class="custom-file-input" id="customFile">
    <label class="custom-file-label" for="customFile">Choose file</label>
  </div>
</form>

<script>
// Añade este código para que aparezca el nombre del archivo al seleccionarlo
$(".custom-file-input").on("change", function() {
  var fileName = $(this).val().split("\\").pop();
  $(this).siblings(".custom-file-label").addClass("selected").html(fileName);
});
</script>
```

### Galerías

Para crear un _carrusel_ de imágenes (en la cabecera habitualmente), usa el siguiente código:

```html
<div id="demo" class="carousel slide" data-ride="carousel">

  <!-- Indicadores -->
  <ul class="carousel-indicators">
    <li data-target="#demo" data-slide-to="0" class="active"></li>
    <li data-target="#demo" data-slide-to="1"></li>
    <li data-target="#demo" data-slide-to="2"></li>
  </ul>

  <!-- El pase de diapositivas o "carrusel" -->
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="la.jpg" alt="Los Angeles">
    </div>
    <div class="carousel-item">
      <img src="chicago.jpg" alt="Chicago">
    </div>
    <div class="carousel-item">
      <img src="ny.jpg" alt="New York">
    </div>
  </div>

  <!-- Controles -->
  <a class="carousel-control-prev" href="#demo" data-slide="prev">
    <span class="carousel-control-prev-icon"></span>
  </a>
  <a class="carousel-control-next" href="#demo" data-slide="next">
    <span class="carousel-control-next-icon"></span>
  </a>

</div>
```

Explicación del efecto de cada clase (más información en [w3schools](https://www.w3schools.com/bootstrap4/bootstrap_ref_js_carousel.asp)):

| Clase | Explicación |
|-------------------------------|-----------------------------------------------------------|
| `.carousel`                   | crear el contenedor del carrusel                                                                                   |
| `.carousel-indicators`        | Añade indicadores (los puntitos abajo de cada diapositiva, que indican _cuántas_ diapositivas hay y en cuál estás) |
| `.carousel-inner`             | Añade los desplazamientos                                                                                          |
| `.slide`                      | Añade el efecto de transición y la animación al pasar de una a otra                                                |
| `.carousel-item`              | Especifica el contenido de cada diapositiva                                                                        |
| `.carousel-control-prev`      | Botón a la diapositiva anterior                                                                                    |
| `.carousel-control-prev-icon` | Icono del botón                                                                                                    |
| `.carousel-control-next`      | Botón a la diapositiva siguiente                                                                                   |
| `.carousel-control-next-icon` | Icono del botón                                                                                                    |

### Modales
### Pistas (Tooltips)
### Popovers
### Toast
### Scrollspy
### Utilidades
### Flex
### Iconos
### Objetos media
### Filtros

## Grid

## Temas

## Ref

