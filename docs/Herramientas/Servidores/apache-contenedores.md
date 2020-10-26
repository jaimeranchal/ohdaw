# Secciones
Se puede **restringir** el ámbito de una o varias directivas según cumplan algunas **condiciones** (que exista algún archivo o tipo de archivo; que el sitio esté guardado en determinada carpeta; que se haya cargado algún módulo etc).

Estas condiciones se pueden aplicar usando _contenedores de secciones de configuración_ (**secciones**), o archivos `.htaccess`.

Un ejemplo:

```apacheconf
# Deniega el acceso al directorio `var/www/html`
# Apache devolverá un error 403
<Directory /var/www/html/configuracion>
    Require all denied
</Directory>
```

```apacheconf
# Deniega el acceso sólo a los archivos con extensión *.conf
# dentro de `/var/www`
<Directory /var/www/>
    <Files "*.conf">
        Require all denied
    </Files>    
</Directory>
```

!!! info "Lista de secciones"
    _(Fuente: [documentación oficial](http://httpd.apache.org/docs/current/sections.html))_

    ```apacheconf
    <Directory>     # según directorio
    <Files>         # archivos concretos
    <If>
    <IfDefine>
    <IfModule>
    <IfVersion>
    <Location>      # según URL
    <MDomainSet>
    <Proxy>
    <VirtualHost>

    # Los siguientes usan expresiones regulares
    <LocationMatch>
    <DirectoryMatch>
    <FilesMatch>
    <ProxyMatch>
    ```
Hay dos **tipos básicos** de contenedores:

1. La mayoría se evalúa _en cada petición_ y se aplican las directivas que coincidan con la condición de la sección.
2. Los contenedores `<IfDefine>`, `<IfModule>` y `<IfVersion>` se evalúan al _inicio_ y _reinicio_ del servidor. Si se cumple la condición en ese momento se aplican las directivas.
    - `<IfDefine>`: se aplican las directivas sólo si se ha definido un parámetro en la línea de comandos de `httpd`.
    ```apacheconf
    <IfDefine ClosedForNow>
        Redirect "/" "http://otherserver.example.com/"
    </IfDefine>
    ```
    - `<IfModule>`: se aplican sólo si ha cargado el módulo especificado en el servidor, ya esté:
        - compilado estáticamente en el servidor
        - compilado dinámicamente (la lína `LoadModule` debe aparecer _antes_ en el archivo de configuración).
        - No debe usarse con directivas que quieras que funcionen siempre.
    ```apacheconf
    <IfModule mod_mime_magic.c>
        MimeMagicFile "conf/magic"
    </IfModule>
    ```
    - `<IfVersion>`: se aplican según la versión del servidor. Se usa para pruebas sobre todo, y en redes muy grandes con diferentes servidores:
    ```apacheconf
    <IfVersion >= 2.4>
        # Ejecuta lo que sea si la versión es igual o superior a la 2.4
    </IfVersion>
    ```

## Sistema de archivos, espacio web y expresiones booleanas
Los contenedores más habituales son los que cambian la configuración dependiente del lugar en el sistema de archivos o en el espacio web.

!!! note "Conceptos"
    **Sistema de archivos**: direcciones de archivos desde la perspectiva de tu sistema: `/usr/local/apache2` o `c:/Program Files/Apache Group/Apache2`.

    **Espacio web**: es la vista de tu sitio tal y como la ofrece el servidor y es vista por el cliente. La ruta `/dir/` sería igual a `/usr/local/apache2/htdocs/dir`. El espacio web no siempre se solapa con el sistema de archivos, puesto que algunas páginas pueden generarse dinámicamente a partir de bases de datos u otros lugares.

### Contenedores de Sistema de Archivos
Las directivas `<Directory>` y `<Files>` junto con sus correspondientes versiones _regex_ restrigen el uso de directivas a partes del sistema de archivos.

- `<Directory>` se aplica al directorio indicado y todos sus subdirectorios. Se puede hacer lo mismo con archivos `.htaccess`.
```apacheconf
# Habilita los índices para el directorio /var/web/dir1 y subdirectorios
<Directory "/var/web/dir1">
    Options +Indexes
</Directory>
```
- `<Files>` se aplica a los nombres de archivo especificados, _independientemente del directorio_:
```apacheconf
# Deniega el acceso al archivo private.html, esté donde esté
<Files "private.html">
    Require all denied
</Files>
```

Ambos se pueden combinar para limitar el acceso a archivos en un directorio determinado:

```apacheconf
<Directory "/var/web/dir1">
    <Files "private.html">
        Require all denied
    </Files>
</Directory>
```

### Contenedores del Espacio Web
Aquí usamos la directiva `<Location>`:

```apacheconf
# Impide el acceso a cualquier dirección URL que empiece con '/private'
<LocationMatch "^/private">
    Require all denied
</LocationMatch>
```

!!! tip "Independiente del sistema de archivos"
    El siguiente ejemplo redirige una URL a un servidor interno Apache proporcionado por el módulo _status_. No es necesario que exista un archivo llamado `server-status` en el sistema:

    ```apacheconf
    <Location "/server-status">
        SetHandler server-status
    </Location>
    ```
