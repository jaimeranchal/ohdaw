# ProFTPd: un servidor FTP para linux

!!! info "Fuentes"
    - [Tutorial básico (redeszone.net)](https://www.redeszone.net/tutoriales/servidores/proftpd/)
    - [Tutorial de ionos.es](https://www.ionos.es/digitalguide/servidores/configuracion/servidor-ftp-en-debian/)
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

El archivo de configuración por defecto está en `/etc/proftpd/proftpd.conf`. Su estructura es muy similar a la de otros servidores, como Apache.

- Las opciones se ajustan mediante **directivas** que establecen diferentes **contextos** o actúan sólo en ellos.
- Para **activar** o **desactivar** opciones se puede usar _on_ / _off_: `RequireValidShell on`.

!!! note "Configuración extra"
    En vez de un archivo proftpd.conf puedes usar también tu propio archivo de configuración y almacenarlo en el directorio `/etc/proftpd/conf.d/`. Este directorio permanece intacto cuando el software FTP se actualiza, de tal modo que se consigue reducir el riesgo de perder las configuraciones.
    Para añadir otros archivos de configuración se usa la directiva `Include /ruta/archivo.conf`.

=== "Contextos"
    - configuración del servidor
    - global
    - VirtualHost
    - Anonymous
    - Limit
    - Directory
    - .ftpaccess
=== "Módulos"
    - `mod_auth`: autenticación
    - `mod_log`: soporte para archivos de registro
    - `mod_ldap`: soporte para autenticación con LDAP
    - `mod_sql`: soporte para SQL
    - `mod_tls`: soporte para el protocolo TLS/SSL
=== "Directivas"

<hr>

Veamos algunos ejemplos de configuraciones habituales:

### Hostname y directorio FTP

```conf
# Indicación de hostname y mensaje de bienvenida
ServerName  "hostname/ip-address"
DisplayLogin  "El inicio de sesión en el servidor FTP en Debian se ha realizado con éxito”  

# Directivas generales de inicio de sesión
<Global>
  # Solo permite el acceso con shells definidos en /etc/shells
  RequireValidShell  on
  # No aceptar Root-Log-in 
  RootLogin  off
  # Indicación del directorio FTP al que debe acceder el usuario
  DefaultRoot  Directorio
</Global>

# Definir usuario/grupo de usuarios autorizados al inicio de sesión en FTP 
<Limit LOGIN>
  # El inicio de sesión solo se permite a los usuarios del grupo ftpuser 
  # En vez de una larga lista se ignora al grupo con (!)
  DenyGroup  !ftpuser
</Limit>
```

Si queremos que los nuevos usuarios accedan solamente a su directorio personal:

```conf
# Permitir a los usuarios solamente el acceso al directorio inicial 
DefaultRoot ~
```

### Crear usuarios

ProFTPd usa los **usuarios del sistema** por defecto.

=== "Añadir usuario"
    ```bash
    sudo adduser usuario
    sudo passwd usuario
    ```

    Si quieres **limitar el acceso** de los usuarios de FTP exclusivamente al servidor y no a todo el sistema, se pueden crear definiendo `/bin/false` como shell de inicio de sesión.

    Primero insertamos `bin/false` en la lista de shells disponibles:

    ```bash
    sudo echo "/bin/false" >> /etc/shells
    # Ahora creamos un nuevo usuario
    sudo adduser user1 --shell /bin/false --home /home/user1
    ```
=== "Directivas"
    ```conf
    <Directory /home/user1>
        # Asigna todos los derechos al propietario
        Umask 022
        # Evita sobreescribir archivos guardados
        AllowOverwrite off
        # Limita el acceso al usuario 1
        <Limit LOGIN>
            AllowUser user1
            DenyAll
        </Limit>
        # Limita el uso de comandos excepto al usuario 1
        <Limit ALL>
            AllowUser user1
            DenyAll
        </Limit>
    </Directory>
    ```

### Acceso anónimo

Útil cuando queremos crear un directorio con descargas públicas, por ejemplo.

=== "Asignar permisos"
    ```bash
    sudo chmod 755 /home/ftpdescargas
    ```
=== "Crear usuario"
    ```bash
    # Creamos un usuario "ftp" y su grupo
    sudo adduser ftp ftpgroup
    ```
=== "Directivas"
    ```conf
    <Anonymous ~ftp>
        User    ftp
        Group   ftpgroup

        # Posibles perfiles de login para clientes
        UserAlias anonymous ftp

        # Número máximo de clientes
        MaxClientes 10
        # Ocultar propiedades de usuario y grupo
        DirFakeUser on ftp
        DirFakeGroup on ftp
        RequireValidShell off

        # Sin permiso de escritura para usuarios anónimos
        <Directory *>
            <Limit WRITE>
                DenyAll
            </Limit>
        </Directory>
    </Anonymous>
    ```

### Encriptación SSL/TLS

El protocolo FTP transfiere los datos en **texto plano** por defecto. Para añadir seguridad podemos **encriptar** el acceso con SSL/TLS, usando **OpenSSL**, por ejemplol.

=== "Requisitos"
    Primero hay que instalar OpenSSL:

    ```bash
    sudo apt install openssl
    ```
=== "Generar Certificado y clave"
    ```bash
    # Creamos una carpeta en el directorio de proftpd
    # para guardar el certificado:
    sudo mkdir /etc/proftpd/ssl

    # Generamos un nuevo certificado válido durante un año
    openssl req -new -x509 -days 365 -nodes 
        -out /etc/proftpd/ssl/proftpd.cert.pem 
        -keyout /etc/proftpd/ssl/proftpd.key.pem
    ```
    
    Se solicitará información adicional:

    - **Country Name**: código de país (_ES_ para España, p.e)
    - **State or Province Name**: estado o provincia.
    - **Locality Name**: ciudad.
    - **Organization Name**: nombre de la compañía en la que trabajas, o tu nombre.
    - **Organizational Unit Name**: departamento, dentro de tu compañía (si lo hay)
    - **Common Name**: nombre del _dominio_ a proteger (`ftp.example.com`, p.e)
    - **Email Address**: una dirección de correo electrónico
=== "Directivas"
    ```conf
    # Activamos el módulo TLS
    <IfModule mod_tls.c>
        TLSEngine   on
        TLSLog      /var/log/proftpd/tls.log
        TLSProtocol TLSv1 TLSv1.1 TLSv.1.2
        TLSRSACertificateFile       /etc/proftpd/ssl/proftpd.cert.pem
        TLSRSACertificateKeyFile    /etc/proftpd/ssl/proftpd.key.pem
        # No verifica el certificado del Cliente
        TLSVerifyClient off
        # Requiere encriptación para establecer la conexión
        TLSRequired     on
    </IfModule>
    ```
=== "Lado del cliente"
    Con todo lo anterior configurado se necesita un cliente que soporte conexión encriptada. Uno muy popular, es [FileZilla](https://www.ionos.es/digitalguide/servidores/configuracion/filezilla-la-solucion-para-el-cliente-ftp/); dentro de éste, hay que elegir `FTPS ("FTP a través de TLS/SSL explícito")` como **tipo de servidor**. En la primera conexión se nos pedirá aceptar el certificado.

