# Archivos de registro

Existen varias directivas para crear archivos de registro. El formato por defecto es CLF (_Common Logon Format_):

```
192.168.200.100 - - [05/May/2011:17:19:18 +0200] "GET /index.html HTTP/1.1" 200 20
```

!!! info "Sintaxis de CLF"
    - Cada línea identifica una petición al servidor
    - contiene varios campos separados por espacios.
    - Un campo vacío se indica con `-`

    | Campos               | Definición                                                                                                        | Ejemplo                                                               |
    |----------------------|-------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
    | `host (%h)`          | Identifica el equipo cliente que solicita la información en el navegador                                          | 192.168.200.100                                                       |
    | `ident (%l)`         | Información del cliente cuando la máquina de éste ejecuta identd y la directiva IdentityCheck está activada       |                                                                       |
    | `authuser ( %u)`     | Nombre de usuario en caso que la URL solicitada requiera autenticación HTTP                                       |                                                                       |
    | `date ( %t)`         | Fecha y hora en el que se produce la solicitud al servidor. Va encerrado entre corchetesy tiene su propio formato | `[dia/mes/año:hora:minuto:segundo zona] [05/May/2011:17:19:18 +0200]` |
    | `request (%r)`       | Página web que está solicitando                                                                                   | /index.html                                                           |
    | `status ( %s o %>s)` | Identifica el código de estado  HTTP de tres dígitos que se devuelve al cliente                                   | 200                                                                   |
    | `Bytes (%b)`         | Sin tener en cuenta las cabeceras  HTTP el número de bytes devueltos al cliente                                   | 20                                                                    |

## Directivas
Son **cuatro**:

- LogFormat
- ErrorLog
- TransferLog
- CustomLog

### LogFormat
Indicar el **formato** que tendrá el archivo de registro (log). Podemos definir qué campos incluir en cada entrada del archivo de log.

- Suele usarse con la directiva _TransferLog_ y _CustomLog_. 
- Se pueden crear varios formatos de log con un nombre especial (**alias**).

```apacheconf
# Sintaxis
LogFormat format|alias_logformat [alias_logformat]
# ejemplo
LogFormat "%h %l %u %t \"%r\" %>s %0" common
```

!!! note "alias globales"
    Los alias definidos con _LogFormat_ son globales, y por lo tanto, visibles dentro de cualquier host virtual.

### CustomLog	
Indica el **nombre del archivo** de registro (log) o el **programa** al que se enviará la información de registro de acceso.

- Se parece a _TransferLog_ pero permite personalizar el formato de registro:
    - empleando códigos de formato
    - indicando un formato de log declarado con _LogFormat_.

```apacheconf
CustomLog nombre_archivo_log | tubería  formato|alias_logformat

# Ejemplo
CustomLog logs/acceso_a_empresa1.log "%h %l %u %t \"%r\" %>s %b"
CustomLog logs/acceso_a_empresa1.log common # usando un formato ya definido

```

!!! info "Alias predefinidos"
    Apache incluye en sistemas Debian una serie de alias de formato ya definidos:

    ```apacheconf
    LogFormat "%v:%p %h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" vhost_combined
    LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %O" common
    LogFormat "%{Referer}i -> %U" referer
    LogFormat "%{User-agent}i" agent
    ```

### TransferLog	
Como _CustomLog_, sin permitir especificar el formato.

```apacheconf
# Sintaxis
TransferLog nombre_archivo_log | tubería_hacia_programa

    # Normalmente se indica primero el formato de log a usar:
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-agent}i\""

    # Y después el archivo donde se guardarán los logs:
    TransferLog logs/acceso_a_empresa1.log
```
### ErrorLog
Permite registrar todos los errores que encuentre Apache:

- en un archivo de registro 
- en syslog

```apacheconf
# Sintaxis
ErrorLog nombre_fichero_registro
# Ejemplo
ErrorLog logs/acceso_a_empresa1.log
```

## Rotar archivos de registro (Archivo)
Cada cierto tiempo es conveniente depurar los registros, comprimirlos y archivarlos. Se puede hacer de dos maneras:

- Con **rotatelogs**, un programa de Apache.
- Con **logrotate**, un herramienta normalmente instalada en sistemas GNU/Linux

!!! note "¿Cuánto tiempo?"
    Por cuestiones legales, los logs deben conservarse como mínimo **durante un año**.

### Con _rotatelogs_

```apacheconf
# Rota 'access.log' cada 24h
CustomLog "|/usr/bin/rotatelogs /var/log/apache2/access.log 86400" common

# Rota 'access.log' cada ver que alcanza los 5MB
CustomLog "|/usr/bin/rotatelogs /var/log/apache2/access.log 5M" common

# Rota 'access.log' cada ver que alcanza los 5MB, usando el sufijo de formato
# YYYY-mm-dd-HH_MM_SS
CustomLog "|/usr/bin/rotatelogs /var/log/errorlog.%Y-%m-%d-%H_%M_%S 5M" common

# Rota 'access.log' cada ver que alcanza los 5MB
# Mantiene un histórico de 10 archivos: si hay más de 10, se borran los más
# antiguos
CustomLog "|/usr/bin/rotatelogs -n 10 /var/log/apache2/access.log 5M" common
# NOTA: la opción -n solo está disponible en Apache version += 2.4.5
```

### Con _logrotate_

El programa _logrotate_ rota, comprime y envía archivos de registro a diario, semanalmente, mensualmente o según el tamaño del archivo. Suele emplearse en una tarea diaria del _cron_. 

En sistemas Debian usa los siguientes archivos de configuración:

- `/etc/logrotate.conf` : define los parámetros globales, esto es, los parámetros por defecto de logrotate.
- `/etc/logrotate.d/apache2`: define el rotado de logs para Apache. Todo lo que no esté configurado aquí usa los valores globales

```sh
# Comprobar la correcta configuración de la rotación de un log
/usr/sbin/logrotate -d /etc/logrotate.d/apache2 

# Forzar la ejecución de logrotate
/usr/sbin/logrotate -f /etc/logrotate.conf

# Script básico para ejecutar logrotate diariamente en el cron.
# Podemos encontrar un ejemplo de este código en /etc/cron.daily/logrotate.

    #!/bin/sh
     
    test -x /usr/sbin/logrotate || exit 0
     
    /usr/sbin/logrotate /etc/logrotate.conf 

# Ejemplo en el que se ha añadido directamente en archivo /etc/crontab el trabajo a ejecutar por cron
# (para editar este archivo es mejor usar el comando crontab -e).

# Rotar logs de apache con logrotate a las 3 am 
0 03 * * * root /usr/sbin/logrotate /etc/logrotate.conf > /dev/null 2>&1 
```

