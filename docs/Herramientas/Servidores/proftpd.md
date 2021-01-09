# ProFTPd: un servidor FTP para linux

!!! info "Fuentes"
    - [Tutorial básico (redeszone.net)](https://www.redeszone.net/tutoriales/servidores/proftpd/)
    - [Documentación oficial (proftpd.org)](http://www.proftpd.org/docs/)

El [protocolo FTP](https://es.wikipedia.org/wiki/Protocolo_de_transferencia_de_archivos) (_File Transfer Protocol_) tiene ya sus años. Permite transferir archivos de forma local y remota, y es bastante rápido.

## Modos de instalación

- **inetd**: si vamos a usarlo poco.
- **standalone** (**independiente**): opción por defecto; uso habitual del servidor.

## Operaciones básicas

=== "SysVinit"
    ```bash
    # Iniciar
    /etc/init.d/proftpd start
    service proftpd start

    # Detener
    /etc/init.d/proftpd stop
    service proftpd stop

    # Reiniciar
    /etc/init.d/proftpd restart
    service proftpd restart

    # Recargar (actualizar conf)
    /etc/init.d/proftpd reload
    service proftpd reload

    # Estado
    /etc/init.d/proftpd status
    service proftpd status
    ```
=== "Systemd"
    ```bash
    # Iniciar
    systemctl start proftpd
    systemctl start proftpd.service

    # Detener
    systemctl stop proftpd
    systemctl stop proftpd.service

    # Reiniciar
    systemctl restart proftpd
    systemctl restart proftpd.service

    # Recargar (actualizar conf)
    systemctl reload proftpd
    systemctl reload proftpd.service

    # Estado
    systemctl status proftpd
    systemctl status proftpd.service
    ```

## Inicio automático

Para establecer que el servicio se inicie al arrancar el sistema:

=== "SysVinit"
    ```bash
    chkconfig proftpd on
    ```
=== "Systemd"
    ```bash
    systemctl enable proftpd
    systemctl enable proftpd.service
    ```

## Configuración

El archivo de configuración por defecto está en `/etc/proftpd/proftpd.conf`. Su estructura es muy similar a la de otros servidores, como Apache. Las opciones se ajustan mediante **directivas** que establecen diferentes **contextos** o actúan sólo en ellos.

=== "Contextos"
    - configuración del servidor
    - global
    - VirtualHost
    - Anonymous
    - Limit
    - Directory
    - .ftpaccess
=== "Módulos"
    Los más relevantes:

    - `mod_auth`: autenticación
    - `mod_log`: soporte para archivos de registro
    - `mod_ldap`: soporte para autenticación con LDAP
    - `mod_sql`: soporte para SQL
    - `mod_tls`: soporte para el protocolo TLS/SSL
=== "Directivas"


Se pueden añadir otros archivos de configuración con la directiva `Include /ruta/archivo.conf`.

### DefaultRoot

- Establece el directorio al que acceden los usuarios cuando inicien sesión.
- Puedes añadir tantos parámetros como usuarios o grupos diferentes necesites

```conf
# Sintaxis
DefaultRoot [dir] [grupo de usuarios con acceso a dir]

DefaultRoot /home/ftp A # A solo puede acceder a /home/ftp
DefaultRoot / B         # B puede acceder a todo el disco
```

### ServerName

Asigna un nombre al servidor

### AccessGrantMsg

Añade un mensaje de bienvenida.

```conf
AccessGrantMsg  "Bienvenido a mis archivos"
```

### AccessDenyMsg

Añade un mensaje de error al iniciar sesión.

```conf
AccessDenyMsg  "Acceso denegado"
```

## Usuarios

ProFTPd usa los **usuarios del sistema** por defecto.

=== "Añadir usuario"
    ```bash
    sudo adduser usuario
    sudo passwd usuario
    ```
