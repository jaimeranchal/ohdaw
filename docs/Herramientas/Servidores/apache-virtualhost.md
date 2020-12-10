# VirtualHost

!!! note "Fuente"
    Documentación oficial de [Apache](http://httpd.apache.org/docs/current/sections.html

## Introducción
El término _VirtualHost_ se refiere a un servidor "virtual". Es una forma de alojar **más de una web** en un mismo servidor, cada una con su configuración independiente. Pueden estar basadas en una IP (cada web tiene una IP distinta), o en nombre (una misma IP da acceso a varios nombres de dominio).

### Directivas de configuración
A la hora de configurar un _VirtualHost_ usamos fundamentalmente:

- `<Virtualhost>`
- `ServerName`
- `ServerAlias`
- `ServerPath`

!!! tip "Debug"
    Si quieres ver qué puede fallar en tu host virtual, puedes usar la opción `-S` en la línea de comandos:

    ```sh
    apachectl -S
    # en windows
    httpd.exe -S
    ```

    Este comando arroja una descripción de cómo Apache ha evaluado el archivo de configuración.
    
## VH basados en nombre

Un [servidor virtual basado en nombre](https://httpd.apache.org/docs/2.4/vhosts/name-based.html) relega al cliente la función de devolver el nombre del host como parte de las cabeceras HTTP. Esta opción es normalmente más simple, porque sólo hay que configurar un servidor DNS que traduzca cada nombre de host a la IP correcta y luego configurar el servidor Apache HTTP para que reconozca los diferentes nombres de host.

```apacheconf
<VirtualHost *:80>
    # This first-listed virtual host is also the default for *:80
    ServerName www.example.com
    ServerAlias example.com 
    DocumentRoot "/www/domain"
</VirtualHost>

<VirtualHost *:80>
    ServerName other.example.com
    DocumentRoot "/www/otherdomain"
</VirtualHost>
```

## VH basados en IP
Utilizan una dirección IP de conexión para elegir qué servidor virtual ofrecer. Por lo tanto necesitas IPs separadas.

```apacheconf
<VirtualHost 172.20.30.40:80>
    ServerAdmin webmaster@www1.example.com
    DocumentRoot "/www/vhosts/www1"
    ServerName www1.example.com
    ErrorLog "/www/logs/www1/error_log"
    CustomLog "/www/logs/www1/access_log" combined
</VirtualHost>

<VirtualHost 172.20.30.50:80>
    ServerAdmin webmaster@www2.example.org
    DocumentRoot "/www/vhosts/www2"
    ServerName www2.example.org
    ErrorLog "/www/logs/www2/error_log"
    CustomLog "/www/logs/www2/access_log" combined
</VirtualHost>
```


## Ejemplos

=== "Con SSL y https"
    ```conf
    <IfModule mod_ssl.c>
    #<VirtualHost _default_:443>
    <VirtualHost 192.168.200.250:443>
        ServerAdmin web-autenticacion@empresa-proyecto.com
        ServerName web-con-autenticacion-basic.empresa-proyecto.com
        DocumentRoot /var/www/web-con-autenticacion-basic
        <Directory /var/www/web-con-autentitacion-basic/>
            AllowOverride AuthConfig 

            Options Indexes FollowSymLinks MultiViews
            Order allow,deny
            allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error-web-autenticacion-basic.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/ssl_access-web-autenticacion-basic.log combined
        SSLEngine on
        SSLCertificateFile    /etc/apache2/tus-ssl/tupaginaweb.crt
        SSLCertificateKeyFile /etc/apache2/tus-ssl/tupaginaweb.key
    </VirtualHost>
    </IfModule>
    ```
=== "htaccess"
    ```conf
    AuthType Basic
    AuthName "Web con Autenticacion Basic"
    AuthUserFile /etc/apache2/web.htpasswd
    ##Require valid-user 
    Require user user1
    ```
    
