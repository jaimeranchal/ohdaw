# Efectos con jQuery

## Mostrar y ocultar

Métodos `hide()` y `show()` o `toggle()` (para alternar entre ambos):

```javascript
$(selector).hide|show(speed,callback);
```

- _speed_: velocidad a la que se produce la animación (`slow`, `fast` o milisegundos)
- _callback_: función que se ejecutará después de que se produzca la animación de esconder o mostrar.

```javascript
// esconder después de una animación (1seg)
$("button").click(function(){
    $("p").hide(1000);
});

// mostrar después de animación (1seg)
$("button").click(function(){
    $("p").show(1000);
});

// alternar
$("button").click(function(){
    $("p").toggle(1000);
});
```

## Desvanecer

Hay **cuatro métodos** para crear un efecto de desvanecimiento:

- `fadeIn()`
- `fadeOut()`
- `fadeToggle()`: alterna entre _in_ y _out_

Todos admiten los mismos parámetros:

- _speed_: velocidad a la que se produce la animación (`slow`, `fast` o milisegundos)
- _callback_: función que se ejecutará después de que se produzca la animación de esconder o mostrar.

```javascript
$("button").click(function(){
    $("#div1").fadeToggle();
    $("#div2").fadeToggle("slow");
    $("#div3").fadeToggle(3000);
});
```

El cuarto método `fadeTo()` permite desvanecer un elemento hasta una **opacidad** concreta (entre 0 y 1):

```javascript
// sintaxis
$(selector).fadeTo(speed,opacity,callback);

// ejemplo
$("button").click(function(){
    $("#div1").fadeTo("slow", 0.15);
    $("#div2").fadeTo("slow", 0.4);
    $("#div3").fadeTo("slow", 0.7);
});
```

## Diapositiva

El efecto de _deslizamiento_ se consigue con tres **métodos**, con los parámetros vistos hasta ahora (_speed_ y _callback_):

- `slideDown()`
- `slideUp()`
- `slideToggle()`

```javascript
$("#flip").click(function(){
    $("#panel").slideToggle();
});
```

## Animaciones

El método `animate()` se usa para crear animaciones _personalizadas_:

```javascript
$(selector).animate({params},speed,callback);
```

El parámetro `params` es un conjunto de **propiedades CSS**.

```javascript
// una sola propiedad
$("button").click(function(){
    $("div").animate({left: '250px'});
}); 

// varias propiedades
$("button").click(function(){
    $("div").animate({
        left: '250px',
        opacity: '0.5',
        height: '150px',
        width: '150px'
    });
}); 
```

!!! note "Limitaciones"
    Los nombres de propiedades CSS que tengan un _guión_ deben convertirse a **CamelCase**:

    - `padding-left > paddingLeft`

    Por otra parte, para manejar **animaciones de colores** hay que instalar un [plugin](http://plugins.jquery.com/) adicional

### Valores de propiedades

jQuery amplía un poco las reglas de CSS:

=== "Relativos"
    ```javascript
    // valores relativos al tamaño del elemento actual
    $("button").click(function(){
        $("div").animate({
            left: '250px',
            height: '+=150px',
            width: '+=150px'
        });
    });
    ```
=== "Predefinidos"
    ```javascript
    // También se puede establecer como valor 'show', 'hide'  o 'toggle'
    $("button").click(function(){
        $("div").animate({
            height: 'toggle'
        });
    })
    ```

### En cola

Por defecto jQuery permite _apilar_ la ejecución de varias animaciones. Es decir, que se pueden **encadenar** animaciones:

```javascript
// ejemplo 1
$("button").click(function(){
    let div = $("div");
    div.animate({height: '300px', opacity: '0.4'}, "slow");
    div.animate({width: '300px', opacity: '0.8'}, "slow");
    div.animate({height: '100px', opacity: '0.4'}, "slow");
    div.animate({width: '100px', opacity: '0.8'}, "slow");
});

// ejemplo 2
$("button").click(function(){
    let div = $("div");
    div.animate({left: '100px'}, "slow");
    div.animate({fontSize: '3em'}, "slow");
}); 
```
