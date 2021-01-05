# Manejo de cookies en JavaScript

Las _cookies_ son **datos** almacenados de forma **temporal** en **pequeños archivos de texto** en tu ordenador.

Se inventaron para que las páginas web pudieran almacenar información sobre el usuario: nombre, contraseña, intentos...

Cuando el navegador solicita una web del servidor, las cookies se añaden a la petición HTTP. De este modo, el servidor puede _recordar_ los datos que incluyan.

## Destripando una cookie

Aunque son archivos de texto el servidor sólo reconoce información guardada en formato _clave-valor_:

```conf
username = John Doe
password = 1234cage
```

## Crear y modificar cookies

Para trabajar con cookies se usa la propiedad `document.cookie`. Si queremos crear una, por ejemplo, usamos:

```javascript
document.cookie = "username = John Doe";
document.cookie = "username = John Doe;" + 
    "expires = Thu, 18 Dec 2020 12:00:00 UTC";
document.cookie = "username = John Doe;" +
    "expires = Thu, 18 Dec 2020 12:00:00 UTC;" +
    "path = /";
```

!!! info "Parámetros"
    Podemos añadir una **fecha de expiración** para las cookies y la ruta (página) a la que pertenece con los parámetros _expires_ y _path_.
    
    - _expires_: la fecha siempre debe ir en formato UTC; por defecto, las cookies expiran **al cerrar el navegador**.
    - _path_: por defecto, las cookies se asignan a la web que ejecuta el script.

Para modificar una cookie existente basta con **reescribir** su contenido:

```javascript
document.cookie = "username = Perico Palotes";
```

## Borrar

Cuando queramos borrar una cookie fijamos como parámetro de expiración una fecha anterior a la actual:

```javascript
document.cookie = "username = John Doe;" +
    "expires = Thu, 1 Jan 1970 12:00:00 UTC;" +
    "path = /";
```

!!! note "Ruta necesaria"
    Para evitar que se borre la cookie equivocada, algunos navegadores exijen que se añada la _ruta_ antes de eliminarla.

## Lectura

Una cookie no es un objeto, ni un array; si accedemos directamente a su contenido, mostrará únicamente una **cadena de texto**. 

Por otra parte, aunque podemos definir tantas cookies como queramos, la propiedad `document.cookie` guarda _todas_, una a continuación de la otra. Si queremos recuperar una en concreto tendremos que escribir una **función de búsqueda de cadena** para un valor específico.

## Ejemplo

Vamos a crear una cookie para guardar el nombre de un visitante:

- Al entrar, se le pedirá que rellene su nombre y se guardará en una cookie
- La siguiente vez que entre, su nombre aparecerá en el mensaje de bienvenida.

Para ello vamos a crear **tres funciones**:

=== "1. Crear la cookie"
    ```javascript
    let setCookie = (cname, cvalue, exdays) => {
        // fecha actual
        let d = new Date(); 
        // fija la fecha de expiración en milisegundos
        d.setTime(d.getTime() + (exdays*24*60*60*1000));
        let expires = "expires=" + d.toUTCString();
        document.cookie = `${cname} = cvalue; ${expires}; path=/`;
    }
    ```
=== "2. Recuperar datos"
    ```javascript
    let getCookie = (cname) => {
        let name = cname + "="; //texto a buscar
        // decodifica la cookie para poder trabajar con signos especiales 
        let decodedCookie = decodeURIComponent(document.cookie);
        // Separa la cadena en líneas por cada punto y coma
        let ca = decodedCookie.split(';'); //array

        for(let i = 0; i < ca.length; i++) {
            // lee cada línea
            let c = ca[i];
            while(c.charAt(0) == ' ') {
                c = c.substring(1);
            }
            if(c.indexOf(name) == 0) {
                // si hay coincidencia, extrae la línea de principio (1) a fin
                return c.substring(name.length, c.length);
            }
        }
        return "";
    }
    ```
=== "3. Comprobar cookies"
    ```javascript
    let checkCookie = () => {
        let username = getCookie("username");
        if (username != "") {
            alert(`Bienvenido de nuevo ${username}`);
        } else {
            username = prompt("Introduce tu nombre por favor:", "");
            if (username != "" && username != null) {
                // si se ha introducido un nombre, crea una cookie durante un año
                setCookie("username", username, 365);
            }
        }
    }
    ```
