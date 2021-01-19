# Métodos de autenticación en PHP

!!! info "Fuentes"
  - https://diego.com.es/autenticacion-http

HTTP soporta el uso de varios mecanismos de identificación para controlar el acceso a páginas u otros resources. Estos mecanismos se basan en el uso del **código de estado 401** y el response header **WWW-Authenticate**.

- **Basic**. El cliente envía el nombre de usuario y la contraseña sin encriptar en codificación textual base64. Sólo debe usarse con HTTPs, ya que la contraseña puede ser fácilmente capturada.
- **Digest**. El cliente envía la contraseña en forma de hash. Es mucho más difícil de obtener los valores de usuario y contraseña.

## Autenticación Basic

Cuando intentamos acceder a un recurso en internet, estamos enviando una **petición HTTP** al servidor. Este recurso puede ser cualquier cosa almacenada en un servidor, y puede estar _protegido_; es decir, se necesita **usuario** y **contraseña** para acceder.

Si un servidor recibe un HTTP request sin identificar para un recurso protegido, se envía una respuesta con **código de estado 401** (_Access Denied_).

En este caso se puede forzar a usar **autenticación básica** estableciendo la cabecera de respuesta así:

=== "Código"
    ```conf
    HTTP/1.1 401 Access Denied
    WWW-Authenticate: Basic realm="Mi servidor"
    Content-Lenght: 0
    ```
=== "Explicación"
    - La palabra _Basic_ en WWW-Authenticate selecciona el mecanismo de identificación que el cliente HTTP debe usar para acceder el resource.
    - El atributo realm se puede establecer a cualquier valor para identificar el área protegida, y puede usarse por los clientes HTTP para **manejar las contraseñas**.

!!! note "Caja de login"
    La mayoría de navegadores mostrarán una caja de login cuando se recibe la respuesta anterior, permitiendo al usuario introducir un usuario y una contraseña.

Esta información se usa para reintentar la petición con un la siguiente cabecera:

=== "Código"
    ```conf
    GET /areasegura/ HTTP/1.1
    Host: www.ejemplo.com
    Authorization: Basic bXl1aefi128djGFzcw==
    ```
=== "Explicación"
    La autenticación especifica el **mecanismo de autenticación**, en este caso Basic, seguido de un usuario y contraseña.

    !!! note "Falsa seguridad"
        La contraseña parece encriptada, pero simplemente es una **codificación en base64** (`bXl1aefi128djGFzcw==` es `'myuser:mypass'`). Cualquiera que pueda interceptar el request puede averiguar el usuario y la contraseña.

## Autenticación Digest

Con el método de autenticación _Digest_ lo que se envía al servidor para identificar es un código creado mediante una función _hash_ usando el password y otros bits de información. Enviar un hash evita el principal problema de la autenticación Basic, que es enviar los datos sin encriptar.

!!! info "Funciones hash"
    Una función criptográfica hash- usualmente conocida como “hash”- es un algoritmo matemático que transforma cualquier bloque arbitrario de datos en una nueva serie de caracteres con una longitud fija. Independientemente de la longitud de los datos de entrada, el valor hash de salida tendrá siempre la misma longitud.

El estándar de los métodos Basic y Digest Authentication se puede ver en el [RFC 2617](http://tools.ietf.org/html/rfc2617), que sustituye al RFC 2069. 

### Segun el RFC 2069 

Se basa en el uso de tres valores generados o almacenados en el servidor que se usan para identificar y validar la petición:

- **nonce**: único para cada petición. Se usa en el _lado del cliente_ para generar el hash que luego se enviará de vuelta al servidor.
- **opaque**: el servidor lo genera y espera que el cliente lo devuelva tal cual.
- **realm**: área de acceso a la que se intenta acceder. También se usa en el _lado del cliente_ para generar el hash que luego se enviará de vuelta al servidor.

```php
<?php
$nonce = md5(uniqid());
$opaque = md5(uniqid());
$realm = 'Acceso privado a ejemplo.com/privado';
?>
```

Todos estos valores se emplean para crear la directiva WWW-Authenticate y enviarla como respuesta al cliente:

```php
<?php
if(empty($_SERVER['PHP_AUTH_DIGEST'])){
    header('HTTP/1.1 401 Unauthorized');
    header(sprintf('WWW-Authenticate: Digest realm="%s", nonce="%s", opaque="%s"', $realm, $nonce, $opaque));
    header('Content-Type: text/html');
    echo '<p>Necesitas autenticarte.</p>';
    exit;
}
?>
```

Cuando el cliente recibe esta respuesta, tiene que computarla y devolver un hash.

Lo hace devolviendo el nombre de usuario, el realm y el password agrupados en un hash de la siguiente forma:

| Nombre   | Valor                             |
|----------|-----------------------------------|
| A1       | `md5("username:realm:password")`  |
| A2       | `md5("requestMethod:requestURI")` |
| response | `md5("A1:nonce:A2")`              |

El cliente envía en el _header Authorization_:

```php
<?php
Authorization: Digest username="%s", realm="%s", nonce="%s", opaque="%s", uri="%s", response="%s"
?>
```

Cuando el servidor recibe la respuesta, se toman los mismos pasos para comprobar la versión del hash del servidor con la recibida en la respuesta. Si coinciden, el request se considera autorizado.

```php
<?php
$A1 = md5("$username:$realm:$password");
$A2 = md5($_SERVER['REQUEST_METHOD'] . ":$uri");
$response = md5("$A1:$nonce:$A2");
?>
```

Para proceder con la identificación a través de una base de datos, el valor A1 puede guardarse. Si se cambia el realm el hash resultará inválido.

### RFC 2617

En el RFC 2617 se incluyeron nuevas características: 

- **qop: quality protection**. Especificado en el header WWW-Authenticate, puede tener un valor de "auth" o "auth-int". Si no se especifica ninguno, por defecto es "auth", Digest Authentication para autenticación de un cliente. Si se especifica "auth-int" se incluye un nivel de protección de integridad en la respuesta, y el cliente debe incluir también el request body como parte del mensaje digest. Esto permite al servidor saber si el request ha sido manipulado entre el cliente y el servidor.
- **cnonce: client nonce**. Similar a nonce pero es generado por el cliente, que envía en la respuesta al servidor para poder comparar digests. Esto proporciona integridad en la respuesta y autenticación mutua. Cuando la directiva qop se envía por parte del servidor, el cliente debe incluir un valor cnonce.
- **nc: nounce count**. Es una cuenta hexadecimal del número de requests que el cliente ha enviado con el valor de un nonce. De esta forma el servidor puede protegerse frente a ataques de repetición.

Ahora analizar el A2 quedaría de la siguiente forma:

```php
<?php
if($qop == 'auth-int'){
    $A2 = md5($_SERVER['REQUEST_METHOD'] . ":$uri:" . md5($respBody));
} else {
    $A2 = md5($_SERVER['REQUEST_METHOD'] . ":$uri");
}
$response = md5("$A1:$nonce:$nc:$cnonce:$qop:$A2");
?>
```

El acceso con digest emplea un valor con hash y no el password legible, por lo que es mucho más seguro que la autenticación básica:

- El _nonce_ del servidor, que ha de ser único por cada request, cambiará el hash computado en cada request nuevo 
- el valor _nc_ que proporciona la RFC 2617 ayuda a prevenir **ataques de repetición**, que ocurren cuando un atacante intercepta los datos del request e intenta repetirlos como si fuera su propio request.

!!! warning "MD5 vs Bcrypt"
    MD5 no es un algoritmo especialmente seguro, con la tecnología moderna no es difícil obtener los valores de un hash md5. Es preferible Bcrypt.

    ```php
    <?php
    /*
     * Genera un hash seguro para una contraseña dada.
     * El coste se pasa al algoritmo blowfish
     * (Consulta el manual php de crypt para más info sobre el coste)
    */
    function generate_hash($password, $cost=11){
            /* Para generar la salt, primero genera suficientes bytes aleatorios
             * En base64 se devuelve un caracter por cada 6 bits, de modo que
             * necesitamos al menos 22*6/8=16.5 bytes; generamos 17.
             * A continuación obtenemos los primeros 22 caracteres en base64
             * /
            $salt=substr(base64_encode(openssl_random_pseudo_bytes(17)),0,22);
            /* Como blowfish usa una salt con los caracteres del alfabeto (./A-Za-z0-9)
             * hay que reemplazar todos los '+' en la cadena en base64 con '.'
             * Los signos '=' no se tocan
             * /
            $salt=str_replace("+",".",$salt);
            /* Ahora creamos una cadena con las opciones para la función crypt
             * separadas por signos de dólar
             */
            $param='$'.implode('$',array(
                    "2y", //selecciona la versión más segura de blowfish (>=PHP 5.3.7)
                    str_pad($cost,2,"0",STR_PAD_LEFT), //añade el coste en dos dígitos
                    $salt //añade la salt
            ));
          
            //codifica el hash
            return crypt($password,$param);
    }

    /*
     * Comprueba que la contraseña coincide con un hash generado por la función generate_hash
     * @param password: una contraseña encriptada con hash
     * @param entrada: la contraseña tal y como la introduce el usuario
    */
    function validate_pw($password, $entrada){
            /* Recrear la contraseña con el hash disponible como parámetro de opciones
             * debería producir el mismo hash que la contraseña que se le pasa
             */
            //return crypt($password, $hash)==$hash;

            // Usar la función hash_equals() es más seguro
            return hash_equals($password, crypt($entrada $password));
    }
    ?>
    ```

## Vulnerabilidades

No hay forma de que el servidor pueda verificar la identidad del cliente que solicita acceso con digest, lo que deja abierta la posibilidad de **ataques "man in the middle"**, donde un cliente cree que va a enviar los datos de autenticación al servidor pero envía la información a otra entidad desconocida.

La mejor seguridad para la autenticación es utilizar SSL y **encriptar las contraseñas con Bcrypt**. Se puede utilizar autenticación básica con SSL, pero en situaciones donde SSL no es posible, el acceso con digest es mucho mejor.
