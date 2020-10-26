# Autenticación y control de acceso
Cuando nos autenticamos en una web suele transferirse la información de autenticación a una base de datos, que puede existir en la misma máquina que el servidor web o en otra totalmente diferente. 

Las más usadas suelen ser SQL o <abbr title="Lightweight Directory Access Protocol">LDAP</abbr> ([OpenLDAP](http://www.openldap.org)). 

## Esquemas de autenticación HTTP

- **Esquema Basic**: el par usario-clave se codifica en BASE64 (débil). Mejor usarlo combinado con una conexión SSL(HTTPS).
- **Esquema Digest**: la clave no se envía directamente, sino que se usan funciones _Hash_ para enviar las credenciales.

El proceso es el siguiente:

1. Cuando se intenta acceder a un recurso, el servidor pide al cliente unas **credenciales** usando un esquema concreto.
2. El cliente **pide al usuario** las credenciales.
3. El cliente **envía los datos** al servidor, y espera su respuesta.

## Módulos de autenticación en Apache

Apache utiliza varios:

- dependiendo del **esquema** usado: 
    - `mod_auth_basic` 
    - `mod_auth_digest`.
- dependiendo de dónde guardemos los datos de usuario (**proveedor de autenticación**):
    - en una base de datos: `mod_authn_dbd` y `mod_dbd` 
    - en un servicio LDAP: `mod_authnz_ldapd`.
    - en un archivo en disco: `mod_authn_file`
- forma de limitar el **acceso a los recursos**:
    - denegar a usuarios concretos: `mod_authz_user`
    - denegar a un grupo de usuarios: `mod_authz_groupfile`

### Ejemplo

- Esquema: basic
- Proveedor de auth.: archivo en disco
- Restricción de acceso: usuarios concretos

=== "Archivo con usuarios"
    Creamos un archivo donde guardar usuarios y contraseñas con `htpasswd`:

    ```sh
    # La opción -c crea el archivo indicado si no existe
    htpasswd -c /etc/apache2/lista_usuarios usuario contraseña
    # Una vez creado el archivo no hace falta la opción -c
    htpasswd /etc/apache2/lista_usuarios usuario2 contraseña2
    ```

    !!! warning "Ubicación del archivo de autenticación"
        Es importante poner este archivo **fuera del `DocumentRoot`**, para tener más seguridad.
    
=== "En apache.conf"
    ```apacheconf
    # Indicamos el directorio donde se efectuará el control de acceso
    <Directory /var/www/html/directorioconcontroldeacceso>
       # Indicamos que el tipo de autenticacion es Basic
       AuthType Basic 
       # Indicamos el mensaje que se le mostrará al usuario 
       AuthName "Debe introducir usuario y contraseña para seguir."
       # Indicamos el proveedor de autenticación (en este caso un archivo)
       AuthBasicProvider file 
       # Indicamos el archivo donde se almacenan los usuario, creado previamente
       AuthUserFile /etc/apache2/lista_de_usuarios 
       # Indicamos que solo podrán acceder una serie de usuarios (que deben existir ya en la lista de usuarios)
       Require user unusuario otrousuario
       # Si queremos que todos los usuarios puedan acceder
       # Require valid-user
    </Directory>
    ```
    
=== "Con .htaccess"
    Primero añadimos la directiva _AllowOverride_ en la configuración general:

    ```apacheconf
    # Indicamos el directorio donde se efectuará el control de acceso
    <Directory /var/www/html/directorioconcontroldeacceso>
      AllowOverride AuthConfig  
    </Directory>
    ```

    Creamos un archivo `.htaccess` en el directorio cuyo acceso queramos limitar con el siguiente contenido:

    ```apacheconf
    # Contenido de un hipotético archivo .htaccess
    AuthType Basic 
    AuthName "Debe introducir usuario y contraseña para seguir."
    AuthBasicProvider file 
    AuthUserFile /etc/apache2/lista_de_usuarios 
    Require user valid-user
    ```

## Autenticación con LDAP

### Instalación y configuración del servidor OpenLDAP

```sh
sudo apt install slapd ldap-utils

# Lanzamos el asistente de configuración
sudo dpkg-reconfigure slapd
# Respondemos:
#   1. Sí (configurar LDAP)
#   2. Crea nueva base de datos
#   3. No (no usar LDAP v.2)

# Arranque y reinicio
sudo /etc/init.d/slapd start|restart
# Detener el servicio
sudo /etc/init.d/slapd stop
```

### Módulos necesarios

- `mod_ldap`
- `mod_authnz_ldapd`

```sh
# Activamos los módulos
sudo a2enmod ldap
sudo a2enmod authnz_ldap

# reiniciamos Apache
/etc/init.d/apache2 restart
```

### Configuración

=== "Crear un host virtual"
    ```apacheconf
    <VirtualHost *:80>
        DocumentRoot /var/www/autenticacion-ldap
        ServerName www.empresa-proyecto.panel-de-control.com
        ServerAlias www.autenticacion-ldap.empresa-proyecto.com
        <Directory /var/www/autenticacion-ldap>
            AllowOverride All
        </Directory>
        ErrorLog /var/log/apache2/error-autenticacion-ldap.log
        LogLevel warn
        CustomLog /var/log/apache2/access-autenticacion-ldap.log combined
    </VirtualHost> 
    ```
=== "Archivo .htaccess"
    ```apacheconf
    AuthName "Autenticacion por LDAP"
    AuthType Basic
    AuthBasicProvider ldap
    AuthzLDAPAuthoritative on
    AuthLDAPUrl ldap://127.0.0.1/ou=usuarios,dc=proyecto,dc=com?uid
    # Acceso denegado para todos excepto el usuario indicado
    Require ldap-user user1LDAP 
    ```
