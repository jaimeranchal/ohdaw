# Directivas
Una directiva es una **regla de funcionamiento** que imponemos a Apache. Las directivas tienen dos partes:

1. _nombre_ de la directiva (`Include` p.e.)
2. unos _parámetros_ (`ports.conf`)

!!! info "Sintaxis"
    - **una directiva por línea**
    - si ocupa más podemos acabar la línea en `\` y continuar en la siguiente.
    - Los parámetros son sensibles a mayúsculas y minúsculas, las directivas, no.

## Contexto
Esto indica dónde se acepta la directiva en los ficheros de configuración. Es una lista separada por comas para uno o más de los siguientes valores:

- **server config**: Esto indica que la directiva puede usarse en los ficheros de configuración del servidor (p.ej., httpd.conf), pero not dentro de cualquier contenedor <VirtualHost> o <Directory>. No se permite en ficheros .htaccess de ninguna manera.
- **virtual host**: Este contexto significa que la directiva puede aparecer dentro de un contenedor <VirtualHost> en el fichero de configuración del servidor.
directory
    - Una directiva marcada como válida en este contexto puede usarse dentro de contenedores <Directory>, <Location>, <Files>, <If>, <Proxy> en los ficheros de configuración del servidor, sujeta a las restricciones destacadas en las Secciones de Configuración.
- **.htaccess**: Si una directiva es válida en este contexto, significa que puede aparecer dentro de ficheros .htaccess de contexto de directorio. Aunque podría no ser procesada, dependiendo de la configuración activa de AllowOverride en ese momento.
    
La directiva _solo_ se permite dentro del contexto designado; si intenta usarlo en algún otro, obtendrá un error de configuración que impedirá que el servidor gestione correctamente las solicitudes en ese contexto, o impedirá que el servidor pueda funcionar completamente -- p.ej., el servidor no arrancará.

Las ubicaciones válidas para la directiva son actualmente el resultado de un Boolean OR de todos los contextos listados. En otras palabras, una directiva que está marcada como válida en "server config, .htaccess" puede usarse en el fichero httpd.conf y en ficheros .htaccess, pero no dentro de contenedores <Directory> o <VirtualHost>.

## DocumentRoot
Indica la carpeta raíz donde se ubica el sitio que queremos servir.

## ServerRoot
Indica el directorio donde está la configuración de Apache. Generalmente no hay que ponerlo,

## Include e IncludeOptional
Indica la ubicación de archivos de configuración extra.

## Listen

Permite especificar en qué puertos se escuchan las peticiones HTTP.

- Suele estar en `/etc/apache2/ports.conf`
- Se puede especificar más de un puerto

```apacheconf
Listen 80
Listen 8080
# Si se tiene más de una IP
# Listen [ip]:[puerto]
Listen 192.168.0.1:80
Listen 192.168.0.2:8080
```

## Options

Permite acceder al directorio de archivos si no existe un archivo _index.html_

- Parámetros `+ | -Indexes`

## AllowOverride

Indica sobreescritura de directivas a través de archivos `.htaccess`

- Sólo para una sección _Directory_
- Opciones:
    - `AllowOverride All`: Todas las directivas aplicables dentro del contexto `.htaccess`.
    - `AllowOverride AuthConfig`: Solo directivas de tipo autenticación y control de acceso, como AuthUserFile o Require.
    - `AllowOverride AuthConfig Indexes`: Se pueden usar directivas de tipo de autenticación y relativas a la indexación de documentos (por ejemplo, para cambiar el _DocumentIndex_ de una carpeta concreta).


## ServerName
Nombre del servidor web configurado; normalmente dentro de una sección `<VirtualHost/>`. Si no es un dominio registrado, hay que añadirlo en `/etc/hosts`.

## ServerAdmin
Dirección de correo del administrador.

## ErrorLog
Indica la ubicación del _log_ de errores: `/var/log/httpd/error_log`

## KeepAlive
Indica si se admiten conexiones _persistentes_. Opciones: `on | off`

## Timeout
## ServerTokens
