# Servidores FTP

## El Protocolo de Transferencia de Archivos

FTP es un acrónimo de _File Transfer Protocol_, un protocolo de transferencia de archivos entre sistemas conectados a una red TCP, basado en la arquitectura **cliente-servidor**. 

Desde un equipo cliente nos conectamos a un servidor para **enviar** o **descargar** datos, de forma independiente al sistema operativo.

### ¿Cómo funciona?

El protocolo FTP funciona en la **capa de aplicación**; requiere **dos puertos** TCP:

- **Puerto 21**: controla la conexión
- **Puerto 20** (o _mayor de 1024_): puerto de datos

## Usuarios

- Usuarios **anónimos**: acceso y permisos _limitados_; la contraseña usada es simbólica.
- Usuarios **del sistema**: creados por el propio servicio FTP (o su administrador); para que solo tengan acceso a los datos del servidor se los _enjaula_, limitando sus permisos.

!!! note "Autenticación"
    Podemos usar varios métodos:
    
    - por fichero
    - LDAP
    - RADIUS
    - Bases de datos...

## Tipos de transferencia de archivos

A la hora de transferir se distinguen dos tipos de archivos:

1. Archivos **ASCII** (texto plano): `.txt`, `.ps`, `.html`...
2. Archivos **binarios** (resto de archivos)

## Comandos básicos

Una lista de comandos básicos cuando trabajemos con ftp **por consola**:

=== "Remoto"
    - `cd`: cambia de directorio en el sistema remoto
    - `dir`: directorio remoto actual
    - `pwd`: directorio remoto activo
    - `rmdir`: elimina un directorio vacío en el servidor
    - `mkdir`: crea un directorio en el servidor
=== "Local"
    - `!cd`: cambia de directorio en el sistema local (sin salir de la consola ftp)
    - `!pwd`: directorio activo local (desde donde iniciamos la conexión al servidor ftp)
    - `!ls`: contenido del directorio local actual.

    !!! note "Pista"
        Para ejecutar un comando referido a ubicaciones o archivos en el directorio local (sin cerrar la conexión ftp), se antepone una **exclamación** (`!`) al comando.

=== "Generales"
    - `get archivo`: descarga un archivo
    - `mget archivos`: descarga varios archivos
    - `put archivo`: sube un archivo al servidor
    - `mput archivos`: sube varios archivos

## Usando una interfaz gráfica (FileZilla)
