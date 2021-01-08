# Eventos

Un evento es cualquier cosa que ocurra o que haga el usuario en el navegador: click en un botón, seleccionar una caja de texto, escribir en ella o simplemente que termine de cargarse la página.

Estos eventos pueden _capturarse_ para **disparar** la ejecución de un script o parte de él.

## Definir o registrar eventos

Puede hacerse de tres maneras:

1. En línea, con el _atributo_ `onclick=`.
2. En código mediante una referencia _tradicional_:

    ```javascript
    // SIN PARÉNTESIS, si no, se ejecutaría nada más cargar el script
    elemento.onclick = hacerAlgo; 
    ```
3. Con el modelo de registro avanzado de eventos.

## Registro de eventos avanzado

Este último método es el recomendado actualmente. El método `addEventListener()` permite definir tantas acciones como queremos para un determinado evento. Admite **tres parámetros**:

1. evento
2. función
3. orden de ejecución:
    - en captura (_true_)
    - en burbujeo (_false_)

=== "Sintaxis"
    ```javascript
    elemento.addEventListener("click", funcion, false|true);
    ```
=== "Ejemplo"
    ```javascript
    document.getElementById("mienlace").addEventListener('click', alertar, false);

    function alertar() {
        // usamos 'this' para que nos valga en cualquier objeto que lo invoque
        alert("Te conectaremos con la página: "+this.href);
    }
    ```

Para eliminar acciones asociadas a un evento, usamos `removeEventListener()`:

```javascript
elemento.removeEventListener('evento', funcion, false|true);
```

