# Fundamentos de JavaScript

Lo primero es conocer la sintaxis básica del lenguaje, cómo codificar lógica condicional y escribir nuestras primeras funciones.

!!! abstract "Contenidos"
    1. [Sintaxis](./sintaxis.md)
    1. [Variables](./variables.md)
    1. [Tipos de datos](./tipos.md)
    1. [Operadores](./operadores.md)
    1. [Operador especial `??`](./operador-nullish-coalescing.md)
    1. [Comparar variables](./comparar.md)
    1. [Condicionales](./condicionales.md)
    1. [Sentencia switch](./switch.md)
    1. [Funciones](./funciones.md)

## "Hola mundo"
JavaScript sirve para darle _funcionalidad_ a la web, y por lo tanto, está estrechamente relacionado con HTML. Vamos a ver un ejemplo sencillo que muestra un mensaje en una ventana emergente cuando entramos a una página web:

```html
<!DOCTYPE HTML>
<html>

<body>

  <p>Before the script...</p>

  <script>
    alert( 'Hello, world!' );
  </script>

  <p>...After the script.</p>

</body>

</html>
```

La etiqueta `<script>` contiene código que se ejecuta cuando el navegador procesa la etiqueta

!!! info "Sintaxis "moderna"
    Antes de que JS se convirtiera en un estándar de facto, la etiqueta `<script>` tenía un par de atributos:

    **Atributo _type_**
    :   Obligatorio en v. &lt; HTML5 (type="text/javascript"). El nuevo estándar cambió ligeramente el sentido de la etiqueta para permitirle usar módulos JS.

    **Atributo _language_**
    :   Cuando se usaban otros lenguajes aparte de JS...


!!! note "Bloquear la ejecución con comentarios"
    Un recurso usado antiguamente era comentar el script dentro de la etiqueta, para esconderlo de los navegadores. **No funciona** con los navegadores de los últimos 15 años:

    ```html
    <script type="text/javascript"><!--
    ...
    //--></script>
    ```

### Scripts externos

Como ocurre con las hojas de estilo y similares, podemos invocar un script remitiendo a su localización con el atributo src:

```html
<script src="/path/to/script.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"></script>
<!-- Varios scripts -->
<script src="/js/script1.js"></script>
<script src="/js/script2.js"></script>
```

Como norma, solo se incluyen en el documento HTML los scripts simples, para evitar saturar la caché. Si lo referenciamos, solo se descargará una vez, aunque se utilice en varios lugares.

Si se usa _src_, se ignora el contenido de la etiqueta

```html
<script src="file.js">
  alert(1); // Esta línea se ignora
</script>

<!-- Posible versión correcta -->
<script src="file.js"></script>
<script>
  alert(1);
</script>
```

## Limitaciones

- No puede **modificar** o **acceder** a las preferencias del navegador, de la ventana principal o de la impresión.
- No puede **enviar _email_** de forma invisible
- No puede acceder al **sistema de ficheros**
- No puede capturar datos de un servidor para retransmitirlos
- No puede interactuar directamente con lenguajes del servidor
- No puede acceder a páginas en **otros dominios**.
- No puede **proteger** el origen de las imágenes de la página

## Seguridad

!!! warning "Siempre visible"
    El código JavaScript **no puede protegerse**; estrategias alternativas son:

    - Incluir mensaje de Copyright
    - Ofuscar el código
    - Promocionar el código
